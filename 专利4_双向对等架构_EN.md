# Patent Document

## Invention Title: Bidirectional Equivalence Architecture for Data Description and Manipulation

### Technical Field

The present invention relates to the field of software architecture technology, specifically to a bidirectional equivalence architecture between data description layer (NDData) and data manipulation layer (TCL/TQL), achieving complete equivalence and unification.

### Background Art

In traditional software architecture, data description (what data looks like) and data manipulation (how to operate data) are separated:

1. **Description and operation separation**: Data described in JSON/XML, operations in API/SQL, two incompatible languages.
2. **Heavy cognitive burden**: Developers must master both data description and manipulation languages.
3. **Consistency hard to guarantee**: Data description and operations use different abstraction levels.
4. **AI understanding difficulty**: AI must understand two different languages, increasing complexity and error rates.
5. **Limited extensibility**: Adding operation types requires modifying operation language; adding data types requires modifying description language.

### Summary of Invention

The present invention proposes a bidirectional equivalence architecture achieving complete equivalence between data description layer and data manipulation layer.

#### Core Innovations

1. **Bidirectional equivalence**: Data description (NDData) and data operation (TCL/TQL) are completely equivalent — every ND data point has a corresponding TCL path, and every TCL statement corresponds to specific ND data.
2. **Unified abstraction**: Data description and data operation use the same abstraction (dimension + path), differing only in focus (description: "what it is", operation: "how to operate").
3. **Deterministic mapping**: From TCL path to ND data, there is a deterministic mapping — same path + same operation = same result.
4. **Orthogonality**: NDData manages "what data is", TCL manages "how to operate data"; responsibilities are naturally orthogonal and do not interfere with each other.

#### Technical Solution

```
Bidirectional Equivalence Architecture:

NDData Description Layer:
  ┌─────────────────────────────┐
  │  NDData: What data is         │
  │  ├── Dimensions: Dimension defs│
  │  ├── Shape: Data shape         │
  │  └── Data: Actual values       │
  └─────────────────────────────┘
           ↕ Complete equivalence
TCL Operation Layer:
  ┌─────────────────────────────┐
  │  TCL/TQL: How to operate data  │
  │  ├── get: Query                │
  │  ├── set: Modify               │
  │  ├── add: Add                  │
  │  ├── del: Delete               │
  │  └── broadcast: Broadcast      │
  └─────────────────────────────┘
```

### Beneficial Effects

1. **Reduced cognitive burden**: One abstraction (dimension + path) covers both description and operation.
2. **Guaranteed consistency**: Architecture ensures consistency between data description and operation.
3. **AI-friendly**: AI only needs to understand one abstraction to operate data.
4. **Predictability**: Same path + same operation = same result.
5. **Composability**: NDData and TCL can be independently extended.
6. **Cross-domain universal**: Same bidirectional equivalence architecture applies to all domains.

### Detailed Description

**Embodiment 1: Office Document**
```
NDData: wenshu.string[0].content = "Hello World"
TCL: get wenshu.string[0].content
Same path structure for description and operation
```

**Embodiment 2: Print Data**
```
NDData: pfd.page[0].colorplate[0].element[0].fill = "#FF0000"
TCL: set pfd.page[0].colorplate[0].element[0].fill = "#000000"
```

**Embodiment 3: Animation Data**
```
NDData: objects[0].position.x = 100
TCL: set objects[0].position.x = 200
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
