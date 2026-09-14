# Patent Document

## Invention Title: Multi-Dimensional Data Dimension Broadcast and Penetration Drilling Operation Methods

### Technical Field

The present invention relates to the field of data operation technology, specifically to dimension broadcast and penetration drilling operation methods for multi-dimensional data, enabling efficient batch data processing and multi-level data queries.

### Background Art

Traditional data operation methods have the following limitations:

1. **Difficult batch operations**: Traditional methods require loop traversal, inefficient and verbose.
2. **Complex conditional operations**: Filtering and operating on specific data points requires complex filter logic.
3. **Difficult multi-level queries**: Penetrating from summary to detail data requires multiple queries.
4. **Inefficient aggregation**: Multi-dimensional grouping aggregation requires pre-defined rules.
5. **Cumbersome data projection**: Extracting specific dimension data requires complex extraction logic.

### Summary of Invention

The present invention proposes dimension broadcast and penetration drilling operation methods for multi-dimensional data.

#### Core Innovations

1. **Dimension broadcast**: Broadcast a single operation to all dimension elements of N-dimensional data, one statement operates thousands of data points.
2. **Conditional broadcast**: On top of broadcast, filter specific data points by conditions for operation.
3. **Penetration drilling**: Penetrate from summary dimensions to detail dimensions layer by layer, enabling multi-level data queries.
4. **Dimension projection**: Extract specified dimension subset from N-dimensional data, forming new data view.
5. **Dimension pivot**: Group by multiple dimensions, supporting flexible report analysis.

#### Technical Solution

```
Dimension Broadcast:
  broadcast path = value
  → Broadcast value to all data points matching path

Conditional Broadcast:
  broadcast path[@condition] = value
  → Broadcast to data points satisfying condition

Penetration Drilling:
  project path as alias     → Extract dimension subset
  filter path where condition → Filter data points
  pivot path rows=[dim] cols=[dim] vals=[dim] agg=aggFunc
  → Group by multiple dimensions

Roll Up:
  roll_up path count dim    → Aggregate dimension data
```

### Beneficial Effects

1. **Efficient batch**: Internal parallelization of dimension broadcast, 100-10000x efficiency improvement.
2. **Precise conditions**: Conditional broadcast supports predicate filtering.
3. **Multi-level penetration**: Penetration drilling supports layer-by-layer queries from summary to detail.
4. **Flexible aggregation**: Dimension pivot supports multi-dimensional grouping.
5. **Unified syntax**: Broadcast, projection, filter, pivot use unified syntax.
6. **Cross-domain universal**: Same operation methods apply to all domains.

### Detailed Description

**Embodiment 1: Print Batch Operations**
```tcl
broadcast pfd.page[*].bleed = 3mm
broadcast pfd.page[*].colorplate[4].enabled = true
broadcast pfd.page[*].colorplate[0..3].element[*].fill = "#000000"
```

**Embodiment 2: Animation Batch Operations**
```tcl
broadcast objects[*].color = hsl(${index}*36, 70%, 50%)
broadcast objects[*].relation = sine(amplitude=50, frequency=1)
broadcast objects[@.size>100].strokeWidth = 2
```

**Embodiment 3: Data Penetration Analysis**
```tcl
project pfd.page[*].colorplate[*].element[*].ndData.data[0] as positions
filter positions where position.x > 100
roll_up pfd.page[*] count elements
pivot pfd.page[*].colorplate[*] rows=[colorplate] cols=[page] vals=[count] agg=SUM
```

---

## License and Authorization

This patent document is open-sourced under the **GNU AFFERO GENERAL PUBLIC LICENSE v3.0 (AGPL-3.0)**, with the following additional terms:

1. **Attribution**: When using this patent document or its technical solutions, you must clearly cite the source, including the inventor's name, affiliation, and original document link.
2. **Contribution Back**: Improvements, derivative works, or enhanced versions based on this patent document must be contributed back to this repository and must not be privatized.
3. **Non-Commercial Free Use**: Non-profit use is free of charge, including personal learning, academic research, scientific research projects, open source community contributions, etc.
4. **Commercial Authorization**: Commercial use requires written authorization and payment of licensing fees. Commercial use includes but is not limited to: using the technical solutions in commercial products, commercial services, commercial systems, commercial consulting, etc.
5. **No Plagiarism**: Plagiarizing the content of this patent document and claiming it as one's own is prohibited. Removing or altering original copyright information is prohibited.

**Inventor**: 陈钦 (cq800229@qq.com)  
**Affiliation**: 四川龙映科技有限公司  
**Filing Date**: 2026-09-14
