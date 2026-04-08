# Cell-Cell Communication

**Pipeline question:** Which cells are signaling to which other cells, through what ligand-receptor interactions, and how does spatial proximity shape these communications?

## Overview

Cell-cell communication (CCC) analysis infers signaling interactions between cells from gene expression data. In spatial omics, CCC gains a critical advantage over dissociated scRNA-seq: physical proximity between sender and receiver cells can be directly measured rather than assumed. Spatial CCC methods exploit this to distinguish plausible interactions (between neighboring cells) from implausible ones (between cells too far apart to physically interact), dramatically reducing false-positive interaction calls.

## Key Methods

### CellChat v2
- **Paper:** [Nature Communications, 2021](https://doi.org/10.1038/s41467-021-21246-9) / [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02158-2)
- **Code:** [github.com/jinworks/CellChat](https://github.com/jinworks/CellChat)
- **Key innovation:** Curated ligand-receptor database (CellChatDB) with multi-subunit complex modeling; v2 adds spatial mapping, multi-dataset comparison, and interaction strength quantification.
- **Strengths:**
    - Most comprehensive LR database including multi-subunit complexes
    - v2 integrates spatial coordinates for distance-aware interaction calling
    - Publication-ready visualization suite
- **Limitations:**
    - Database-driven — cannot discover novel interactions
    - Spatial mode added in v2 but not the primary design focus
    - R only
- **Technology compatibility:** Visium, Xenium, MERFISH, CosMx (v2 spatial mode)

### LIANA+
- **Paper:** [Nature Cell Biology, 2024](https://doi.org/10.1038/s41556-024-01548-4)
- **Code:** [github.com/saezlab/liana-py](https://github.com/saezlab/liana-py)
- **Key innovation:** Meta-framework that runs and integrates results from multiple CCC methods (CellPhoneDB, NATMI, Connectome, etc.) through a consensus scoring approach.
- **Strengths:**
    - Aggregates results across methods to reduce method-specific biases
    - Supports spatial CCC via integration with MISTy
    - Active development with rich Python/scverse integration
- **Limitations:**
    - Meta-framework complexity can obscure method-specific assumptions
    - Performance depends on the weakest constituent methods
- **Technology compatibility:** Visium, Xenium, MERFISH, CosMx, any platform via MISTy integration

### COMMOT
- **Paper:** [Nature Methods, 2023](https://doi.org/10.1038/s41592-022-01728-4)
- **Code:** [github.com/zcang/COMMOT](https://github.com/zcang/COMMOT)
- **Key innovation:** Optimal transport framework that infers directed cell-cell communication flows based on spatial distance constraints and ligand-receptor co-expression.
- **Strengths:**
    - Explicitly spatial — models communication as a transport problem across physical space
    - Infers directionality of signaling
    - Visualizes communication as spatial flow fields
- **Limitations:**
    - Computationally expensive for large datasets
    - Optimal transport assumptions may not match all signaling mechanisms (e.g., long-range paracrine)
- **Technology compatibility:** MERFISH, Xenium, Slide-seq, any cell-resolution spatial data

### SpaTalk
- **Paper:** [Nature Communications, 2022](https://doi.org/10.1038/s41467-022-32111-8)
- **Code:** [github.com/ZJUFanLab/SpaTalk](https://github.com/ZJUFanLab/SpaTalk)
- **Key innovation:** Models the full ligand-receptor-target pathway (not just LR pairs) by integrating downstream signaling cascades with spatial cell-cell interactions.
- **Strengths:**
    - Goes beyond LR pairs to infer downstream target gene activation
    - Spatial distance filtering reduces false positives
- **Limitations:**
    - Pathway inference adds assumptions and complexity
    - R only
- **Technology compatibility:** Visium, Slide-seq

### MISTy
- **Paper:** [Genome Biology, 2022](https://doi.org/10.1186/s13059-022-02663-5)
- **Code:** [github.com/saezlab/mistyR](https://github.com/saezlab/mistyR)
- **Key innovation:** Multi-view learning framework that models how a cell's state is explained by its intrinsic features, immediate neighbors (juxtacrine), and tissue-level context (paracrine) at multiple spatial scales.
- **Strengths:**
    - Models multiple spatial scales simultaneously
    - Does not rely on predefined LR databases
    - Quantifies how much spatial context explains each gene's expression
- **Limitations:**
    - Does not identify specific LR pairs — complementary to LR-based methods
    - Computationally intensive for many features
- **Technology compatibility:** Visium, Xenium, MERFISH, any spatial platform

### NicheNet
- **Paper:** [Nature Methods, 2020](https://doi.org/10.1038/s41592-019-0667-5)
- **Code:** [github.com/saeyslab/nichenetr](https://github.com/saeyslab/nichenetr)
- **Key innovation:** Predicts which ligands from sender cells regulate target genes in receiver cells by integrating prior knowledge of signaling and gene regulatory networks.
- **Strengths:**
    - Connects ligands to downstream gene regulation
    - Strong prior knowledge integration
    - Well-established in the field
- **Limitations:**
    - Originally designed for scRNA-seq, not spatial — spatial extensions exist but are not native
    - Heavily dependent on prior knowledge database quality
    - R only
- **Technology compatibility:** Can be applied to any spatial data but not spatially-native

### SpatialDM
- **Paper:** [Nature Communications, 2023](https://doi.org/10.1038/s41467-023-43608-5)
- **Code:** [github.com/gao-lab/SpatialDM](https://github.com/gao-lab/SpatialDM)
- **Key innovation:** Statistical framework that tests for spatial co-localization of ligand-receptor pairs, using bivariate spatial statistics (Moran's bivariate I) to identify interactions.
- **Strengths:**
    - Rigorous statistical testing with well-calibrated p-values
    - Identifies spatially localized hotspots of LR activity
    - Python implementation
- **Limitations:**
    - Tests co-localization, not causation
    - Limited to pairwise LR interactions
- **Technology compatibility:** Visium, Slide-seq, MERFISH

### FlowSig
- **Paper:** [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02380-w)
- **Code:** [github.com/axelalmet/FlowSig](https://github.com/axelalmet/FlowSig)
- **Key innovation:** Causal inference framework that learns directed signaling flow graphs from spatial omics data, moving beyond correlation to infer causal communication structure.
- **Strengths:**
    - Causal inference distinguishes drivers from responders
    - Learns information flow across the tissue
- **Limitations:**
    - Causal assumptions may not hold in all biological contexts
    - Requires sufficient spatial coverage
- **Technology compatibility:** Visium, MERFISH

### DeepLinc
- **Paper:** [Genome Biology, 2022](https://doi.org/10.1186/s13059-022-02692-0)
- **Code:** [github.com/jing-xuan/DeepLinc](https://github.com/jing-xuan/DeepLinc)
- **Key innovation:** Deep learning framework that infers cell-cell interaction networks from spatial gene expression, learning nonlinear spatial communication patterns.
- **Strengths:**
    - Captures nonlinear interaction effects
    - Data-driven without relying on predefined LR databases
- **Limitations:**
    - Black-box model — limited interpretability
    - Requires training data
- **Technology compatibility:** MERFISH, Slide-seq

### ncem
- **Paper:** [Nature Methods, 2022](https://doi.org/10.1038/s41592-021-01385-3)
- **Code:** [github.com/theislab/ncem](https://github.com/theislab/ncem)
- **Key innovation:** Neural conditional expression models that learn how a cell's expression depends on the types and states of its spatial neighbors.
- **Strengths:**
    - Quantifies how much cell-type neighborhoods influence gene expression
    - Flexible neural architecture
    - Python/scverse ecosystem
- **Limitations:**
    - Does not identify specific LR pairs
    - Requires cell-type annotations as input
- **Technology compatibility:** MERFISH, Xenium, any cell-resolution data

### DeepTalk
- **Paper:** [Nature Communications, 2024](https://doi.org/10.1038/s41467-024-44984-4)
- **Code:** [github.com/JiangBioLab/DeepTalk](https://github.com/JiangBioLab/DeepTalk)
- **Key innovation:** Graph neural network that jointly deconvolves cell types and infers cell-cell communications from spatial transcriptomics data.
- **Strengths:**
    - Joint deconvolution and CCC avoids error propagation between steps
    - Deep learning captures complex interactions
- **Limitations:**
    - Complex model requiring careful training
    - Limited to sequencing-based spatial data
- **Technology compatibility:** Visium

### CellAgentChat
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.07.05.602294)
- **Code:** [github.com/LiLabAtVT/CellAgentChat](https://github.com/LiLabAtVT/CellAgentChat)
- **Key innovation:** Agent-based modeling framework where cells act as autonomous agents that communicate through spatial signaling rules.
- **Strengths:**
    - Simulation-based approach can model dynamic signaling
    - Captures emergent communication patterns
- **Limitations:**
    - Agent-based modeling adds complexity and requires parameter specification
    - Emerging method with limited validation
- **Technology compatibility:** Visium, MERFISH

## Benchmark Summary

No single method dominates spatial CCC analysis because the methods answer fundamentally different questions. **CellChat v2** is the most widely used for ligand-receptor interaction detection and provides the richest visualization. **COMMOT** is the best purely spatial-aware method, using optimal transport to model physical communication constraints. **MISTy** excels at modeling how spatial context at multiple scales influences cell state, but does not identify specific LR pairs. The **LR database is often the true bottleneck** — all database-dependent methods are limited by database completeness and accuracy.

!!! tip "Combine approaches"
    Use CellChat v2 or LIANA+ for LR interaction identification, then MISTy to quantify spatial context effects at multiple scales. This combination captures both specific interactions and broader spatial influence patterns.

!!! warning "Correlation is not causation"
    Co-expression of a ligand in cell A and a receptor in cell B near each other does not prove they are communicating. Spatial CCC methods identify plausible interactions — validation requires perturbation experiments.

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| Any spatial data | Identify LR interactions | CellChat v2 | Most comprehensive database, best visualizations |
| Any spatial data | Consensus across methods | LIANA+ | Aggregates multiple CCC methods |
| Cell-resolution spatial | Spatially-directed communication | COMMOT | Optimal transport models physical signaling |
| Any spatial data | Multi-scale spatial context | MISTy | Separates juxtacrine and paracrine effects |
| Any spatial data | LR-to-target gene regulation | NicheNet or SpaTalk | Connect ligands to downstream gene changes |
| Cell-resolution spatial | Causal signaling flow | FlowSig | Infers directed signaling structure |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| CellChat v2 | Yes | - | Yes | Yes | Yes | - | - |
| LIANA+ | Yes | - | Yes | Yes | Yes | - | - |
| COMMOT | - | - | Yes | Yes | - | - | - |
| SpaTalk | Yes | - | - | - | - | - | - |
| MISTy | Yes | - | Yes | Yes | - | - | - |
| NicheNet | Yes | - | Yes | Yes | Yes | - | - |
| SpatialDM | Yes | - | - | Yes | - | - | - |
| FlowSig | Yes | - | - | Yes | - | - | - |
| ncem | - | - | Yes | Yes | - | - | - |
| DeepTalk | Yes | - | - | - | - | - | - |
| CellAgentChat | Yes | - | - | Yes | - | - | - |
