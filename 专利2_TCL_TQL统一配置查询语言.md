# 发明专利文档

## 发明名称: 统一配置查询语言(TCL/TQL)及数据操作方法

### 技术领域

本发明属于编程语言与数据操作技术领域，具体涉及一种基于路径表达式的统一配置查询语言(TCL/TQL)及其数据操作方法，适用于多领域统一数据操控。

### 背景技术

传统数据操作存在以下问题：

1. **语言碎片化**：不同领域使用不同查询语言（SQL/JSONPath/XPath/TCL等），学习和维护成本高。
2. **操作不一致**：查询、修改、删除、批量操作使用不同语法，增加认知负担。
3. **缺乏维度感知**：传统查询语言不理解数据的多维结构，无法表达维度级别的操作。
4. **批量操作困难**：传统方式需要循环遍历，无法一条语句操作多个数据点。
5. **路径表达不统一**：不同系统使用不同路径表示法（点号/斜杠/方括号），互不兼容。

### 发明内容

本发明提出一种统一配置查询语言（TCL/TQL），通过路径表达式统一实现对N维数据的查询、修改、批量操作和穿透钻取。

#### 核心创新

1. **路径表达式统一**：使用统一的路径语法（模块.元素.属性.索引）定位任意维度的任意数据点。
2. **操作类型统一**：查询(get)、修改(set)、添加(add)、删除(del)、批量(broadcast)使用同一语法。
3. **维度感知**：路径表达式直接映射到NDData的维度结构，操作结果与维度结构一致。
4. **批量操作**：通过通配符(*)和维度广播机制，一条语句操作多个数据点。
5. **穿透钻取**：支持从汇总维度到明细维度的逐层穿透查询。

#### 技术方案

```
TCL路径语法:
  模块.元素.属性[索引]

操作语法:
  get 路径              // 查询
  set 路径 = 值         // 修改
  add 路径 {数据}       // 添加
  del 路径[索引]        // 删除
  broadcast 路径 = 值   // 广播
```

#### 路径解析

路径 `pfd.page[1].colorplate[0].element[5].attribute.trapping` 解析为：

```
TCLPath {
    Module: "pfd",           // 模块维度
    Element: "page",          // 页面维度
    Indices: [1, 0, 5],      // 各维度索引
    Property: "trapping",     // 属性
    Depth: 4                  // 穿透深度
}
```

#### 维度广播

```tcl
# 将值广播到所有页面的所有色版的所有元素
broadcast pfd.page[*].colorplate[*].element[*].fill = "#000000"

# 条件广播
broadcast pfd.page[*].colorplate[*].element[@.fill=="#FF0000"].stroke = "#000000"
```

#### 穿透钻取

```tcl
# 从汇总维度穿透到明细维度
project pfd.page[*].colorplate[*].element[*].ndData.data as positions
filter pfd.page[*].colorplate[*].element[*] where ndData.data[0][2] != 28.5
roll_up pfd.page[*] count elements
pivot pfd.page[*].colorplate[*] rows=[type] cols=[layer] vals=[count] agg=SUM
```

### 有益效果

1. **统一操作接口**：一种语言覆盖查询、修改、添加、删除、批量操作，消除语言碎片化。
2. **维度感知**：路径表达式直接映射NDData维度结构，操作结果与维度结构一致。
3. **高效批量**：通配符+维度广播机制，一条语句操作数千个数据点，效率提升100-10000倍。
4. **精确控制**：路径表达式可精确定位到任意维度的任意数据点，控制精度达标点级别。
5. **跨领域通用**：同一套TCL/TQL语法适用于办公文档、印刷设计、动画创作、印刷标准等不同领域。
6. **AI友好**：TCL语法接近自然语言，易于AI解析和生成。

### 附图说明

图1：TCL语法结构图
图2：路径解析流程图
图3：维度广播操作示意图
图4：穿透钻取操作流程图

### 具体实施方式

**实施例1：办公文档操作**

```tcl
# 查询所有文档的所有段落的第一句话
get wenshu.string[*].content[0]

# 修改所有文档的所有段落的字体
set wenshu.string[*].style.font = "黑体"

# 批量添加段落
add wenshu[0].string {content: "新段落", style: {font: "宋体"}}
```

**实施例2：印刷操作**

```tcl
# 设置所有页面CMYK版黑色文字为纯黑
set pfd.page[*].colorplate[0].element[*][@.fill=="#000000"].fill = "#000000"

# 查询第3页所有专色的元素数量
get pfd.page[3].colorplate[*].element.count

# 设置所有页面的出血
set pfd.page[*].bleed = 3mm
```

**实施例3：动画操作**

```tcl
# 一行代码实现复杂动画
objects[*].relation=sine(amplitude=50, frequency=1, phase=${index}*0.3)

# 批量设置颜色
objects[*].color=hsl(${index}*36, 70%, 50%)
```

---

## 补充：DD路径段语法与三阶段流水线（基于`透明统一路径_DD穿透设计方案.md`）

### 补充2.1：DD路径段语法

| 语法 | 语义 | 示例 |
|------|------|------|
| `.fieldName` | 进入JSON对象字段 | `doc[0].user.name` |
| `[index]` | 进入JSON数组索引 | `doc[0].tags[0]` |
| `[*]` | 遍历JSON数组所有元素 | `doc[*].tags[*]` |
| `[start:end]` | JSON数组切片 | `doc[0].items[1:5]` |
| `[@predicate]` | JSON数组/对象谓词过滤 | `doc[*].items[@.price > 100]` |
| `..fieldName` | 递归下降搜索字段 | `doc..email` |

### 补充2.2：三阶段路径解析流水线

```
UnifiedPathParser:
  阶段1: TDPathParser    → TD树导航 (shucang.document[0])
  阶段2: NDPathParser    → ND多维定位 (.field[3])
  阶段3: DDPathParser    → 文档内部穿透 (.user.name)  [新增]
```

**语义规则**：
1. TD段：标识符 + [索引/*/切片] → TD树导航
2. ND段：进入ND容器后的轴名/轴序/通配/切片/谓词 → ND坐标
3. DD段：在DtypeObject/DtypeArray cell后继续.field/[index] → 文档内部导航

### 补充2.3：谓词掩码语义放宽

原始限制（示例E.4/F.5）：谓词筛选必须是最末路径段。

**新规则**：ND谓词后允许DD段，DD谓词后禁止继续路径。

```tcl
# 原禁止: get doc.cell[0].行[*][@.value > 40].foo  → 报错
# 新允许: get doc.cell[0].行[*][@.value > 40].foo  → ND谓词筛选后进入DD导航

# ND谓词筛选 + DD穿透:
get shucang.document[*].field[3].user.addr.city
# 语义: ND谓词筛选field[3]为Object的cell → DD穿透进入user.addr.city
```

### 补充2.4：上下文敏感谓词语法

```tcl
# ND层谓词：作用于ND cell的数值
get shucang.vector[*].embedding[@.value > 0.8]

# DD层谓词：作用于JSON对象字段
get shucang.document[*].data[@.price > 100]

# 显式区分(可选):
get shucang.document[*].data[@ND.value > 0.8]   # 显式ND层谓词
get shucang.document[*].data[@DD.price > 100]   # 显式DD层谓词
```

### 补充2.5：广播写入DD层语义规则

```tcl
# 广播规则:
# 1. 目标必须是已存在的JSON数组元素(不自动扩容)
# 2. 广播值类型必须与目标元素类型兼容
# 3. 如需追加/删除，使用add/del动词(TD层语义)

set doc[*].field[3].tags[*] = "new"      # 覆盖所有现有tags元素
add doc[0].field[3].tags "new_tag"       # TD层add：追加到JSON数组
del doc[0].field[3].tags[0]              # TD层del：删除JSON数组索引0
```

### 补充2.6：事务Undo栈扩展

```
UndoEntry扩展:
  TDPath    string      # shucang.document[0].field[3]
  NDCoord   []int64     # [3] (field索引)
  DDPath    []string    # ["user", "name"] (JSON内部路径)
  OldValue  interface{} # "张三"
  OpType    Verb        # set/add/del
```

### 补充2.7：跨领域应用扩展

| 领域 | DD穿透示例 |
|------|------------|
| 办公文档 | `get wenshu.doc[0].paragraph[3].style.font` → 穿透段落样式对象 |
| 印刷设计 | `get pfd.page[0].colorplate[0].element[5].attribute.trapping` → 穿透属性对象 |
| 动画创作 | `get objects[0].relation.sine.amplitude` → 穿透运动关系对象 |
| 印刷标准 | `get pfd.page[0].colorplate[0].element[5].attribute.overrides` → 穿透覆盖配置 |

---

## 许可与授权

本专利文档采用 **GNU AFFERO GENERAL PUBLIC LICENSE v3.0（AGPL-3.0）** 协议开源，并附加以下条款：

1. **引用规范**：使用本专利文档或其技术方案时，必须明确注明出处，包括发明人姓名、单位及原始文档链接。
2. **改进回馈**：基于本专利文档的改进、衍生作品或增强版本，必须回馈至本仓库，不得私有化。
3. **非商用无偿**：非盈利用途可无偿使用，包括个人学习、学术研究、科研项目、开源社区贡献等。
4. **商用授权**：商业用途需获得书面授权并支付授权费用。商业用途包括但不限于：将本技术方案用于商业产品、商业服务、商业系统、商业咨询等。
5. **禁止剽窃**：禁止剽窃本专利文档的内容并据为己有，禁止删除或篡改原始版权信息。

**发明人**: 陈钦 (cq800229@qq.com)  
**单位**: 四川龙映科技有限公司  
**申请日期**: 2026-09-14
