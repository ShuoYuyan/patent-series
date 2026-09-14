# Patent Document

## Invention Title: Tree Architecture and Path Index Based Data Storage and Query System

### Technical Field

The present invention relates to the fields of data storage and query technology, specifically to a tree architecture and path index based data storage and query system for on-demand growth storage and O(1) path queries of unified multi-domain data.

### Background Art

Traditional data storage systems have the following limitations:

1. **Fixed structure**: Relational databases require predefined table structures; document databases support dynamic structures but query efficiency degrades with data volume.
2. **Low query efficiency**: JOIN operations in relational databases grow with data scale; full table scans in document databases are inefficient.
3. **Space waste**: Traditional formats include fixed structures that occupy space even when unused.
4. **Difficult path queries**: Path queries on tree-structured data (JSON/XML) require traversal with O(n) complexity.
5. **Multi-format fragmentation**: Different domains use different data formats (JSON/XML/YAML/binary), incompatible with each other.

### Summary of Invention

The present invention proposes a tree architecture and path index based data storage and query system, achieving on-demand growth storage and O(1) path queries.

#### Core Innovations

1. **Tree architecture**: Data organized in tree structure, initially only root node (zero space occupation), branches created on demand (on-demand growth).
2. **Path index**: Path-to-node mapping index for O(1) path queries.
3. **Self-contained nodes**: Each node self-contains NDData (multi-dimensional data) and TVLStyle (style), no external references needed.
4. **Dimension coordinate positioning**: Tree paths combined with NDData dimension coordinates for precise data location down to punctuation level.

#### Technical Solution

```
TDF Tree = {
    Root: TDFTreeNode,
    PathIndex: PathIndex,
    NodeIndex: NodeIndex
}

TDFTreeNode = {
    ID: string,
    ParentID: string,
    NodeName: string,
    NodeType: NodeType,
    NDData: NDData,
    TVLStyle: TVLStyle,
    Children: [TDFTreeNode],
    Metadata: NodeMetadata
}
```

#### On-Demand Growth Mechanism

Initial state: Tree has only root node (zero space)

Add module: root → wenshu
Add element: root → wenshu → string_1
Add style: root → wenshu → string_1 → style

#### O(1) Path Query

```
Query: get wenshu.string[0].style.font
1. Parse path: wenshu.string[0].style.font
2. Look up PathIndex.PathMap["wenshu.string[0].style.font"]
3. Return corresponding node directly

Time complexity: O(1) (hash lookup)
Traditional traversal: O(n) (must traverse all nodes)
```

### Beneficial Effects

1. **Zero space occupation startup**: Initial state has only root node, on-demand growth, extremely space efficient.
2. **O(1) path queries**: Path index enables constant-time queries, 5-10x faster than traditional traversal.
3. **On-demand growth**: Nodes created only when needed, avoiding pre-allocation waste.
4. **Self-contained nodes**: High data integrity, no external references.
5. **Unified format**: All domain data uses same tree architecture, eliminating format fragmentation.

### Detailed Description

**Embodiment 1: Office Document Storage**
```
Initial: root (zero space)
Add document module: root → wenshu
Add paragraph: root → wenshu → string_1
Add style: root → wenshu → string_1 → style
```

**Embodiment 2: Print Data Storage**
```
root → pfd → page[0] → colorplate[0] → element[0] → style
root → pfd → page[0] → colorplate[0] → element[0] → content (NDData)
```

**Embodiment 3: Animation Data Storage**
```
root → animation → object[0] → relation → sine(amplitude=50)
```

---

## License and Authorization

**Copyright Ownership**: All theories and product patents/invention patents/copyrights based on this theoretical system are owned by Chen Qin of Sichuan Longying Technology Co., Ltd.

**License Terms**: This patent document is open-sourced under the **GNU AFFERO GENERAL PUBLIC LICENSE v3.0 (AGPL-3.0)**, with the following additional terms:

1. **Attribution Required**: Anyone using or referencing this patent document must clearly cite the source, including the inventor's name, affiliation, and original document link.
2. **Contribution Back**: Improvements should be contributed back to the repository. Derivative works or enhanced versions based on this patent document must be contributed back to this repository and must not be privatized.
3. **Non-Commercial Free Use**: Non-commercial use is free of charge, including personal learning, academic research, scientific research projects, open source community contributions, etc.
4. **Commercial Authorization Required**: Commercial use requires written authorization and payment of licensing fees. Commercial use includes but is not limited to: using the technical solutions in commercial products, commercial services, commercial systems, commercial consulting, etc.
5. **Legal Protection**: This product is protected by international law. Infringement will be prosecuted to the fullest extent of the law.
6. **No Plagiarism**: Plagiarizing the content of this patent document and claiming it as one's own is prohibited. Removing or altering original copyright information is prohibited.

**Inventor**: 陈钦 (cq800229@qq.com)  
**Affiliation**: 四川龙映科技有限公司  
**Filing Date**: 2026-09-14
