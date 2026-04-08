# Multi-Modal Integration

**Pipeline question:** How do we combine spatial transcriptomics with other data modalities — chromatin accessibility, protein expression, histology, or dissociated scRNA-seq — to build a more complete picture of tissue biology?

## Overview

Spatial omics technologies increasingly generate or pair with multiple data types: RNA expression combined with protein (CITE-seq, CosMx), chromatin accessibility (spatial ATAC), histology images, or matched scRNA-seq. Multi-modal integration methods align these complementary views to reveal relationships invisible in any single modality — for example, connecting spatial gene expression patterns to chromatin states or linking protein markers to transcriptomic subtypes.

## Key Methods

### SpatialGLUE
- **Paper:** [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02316-6)
- **Code:** [github.com/JinmiaoChenLab/SpatialGLUE](https://github.com/JinmiaoChenLab/SpatialGLUE)
- **Key innovation:** Graph dual-attention framework that integrates two spatial omics modalities (e.g., RNA + protein, RNA + ATAC) by learning modality-specific and shared spatial representations.
- **Strengths:**
    - Designed specifically for multi-modal spatial data
    - Dual-attention mechanism handles modality-specific noise
    - Demonstrated on spatial RNA + protein and spatial RNA + ATAC
- **Limitations:**
    - Limited to two modalities at a time
    - Requires spatially-matched multi-modal data
    - GPU required
- **Technology compatibility:** Spatial CITE-seq, SPOTS, spatial ATAC, any co-measured spatial modalities

### HEST
- **Paper:** [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02553-7)
- **Code:** [github.com/mahmoodlab/HEST](https://github.com/mahmoodlab/HEST)
- **Key innovation:** Large-scale collection and framework for integrating H&E histology images with spatial transcriptomics, enabling histology-guided spatial analysis and gene expression prediction from images.
- **Strengths:**
    - Standardized dataset of 1,000+ paired H&E + spatial transcriptomics samples
    - Enables training of histology-to-expression prediction models
    - Connects pathology and molecular spatial data
- **Limitations:**
    - Histology-to-expression prediction has inherent accuracy ceilings
    - Dataset focus on Visium platform
- **Technology compatibility:** Visium, any platform with paired histology

### moscot
- **Paper:** [Nature, 2024](https://doi.org/10.1038/s41586-024-08453-2)
- **Code:** [github.com/theislab/moscot](https://github.com/theislab/moscot)
- **Key innovation:** Multi-omics single-cell optimal transport framework for aligning cells across modalities, time points, and spatial coordinates using unbalanced optimal transport.
- **Strengths:**
    - Unified optimal transport framework for many alignment tasks
    - Handles spatial + temporal + multi-modal integration
    - Scalable to large datasets through entropic regularization
    - Part of the scverse ecosystem
- **Limitations:**
    - Optimal transport parameters require tuning
    - Complex framework with a steep learning curve
- **Technology compatibility:** Visium, Slide-seq, MERFISH, any spatial platform

### MISO
- **Paper:** [Nature Biotechnology, 2024](https://doi.org/10.1038/s41587-024-02443-x)
- **Code:** [github.com/KlugerLab/MISO](https://github.com/KlugerLab/MISO)
- **Key innovation:** Multi-resolution framework that integrates spot-level and single-cell spatial data, bridging the resolution gap between Visium and imaging-based platforms.
- **Strengths:**
    - Addresses the resolution gap between technologies
    - Can enhance Visium resolution using imaging-based data as reference
- **Limitations:**
    - Requires data from multiple platforms on similar tissue
    - Alignment across platforms introduces new assumptions
- **Technology compatibility:** Visium + MERFISH, Visium + Xenium (cross-platform)

### SpaGE
- **Paper:** [Nucleic Acids Research, 2020](https://doi.org/10.1093/nar/gkaa740)
- **Code:** [github.com/tabdelaal/SpaGE](https://github.com/tabdelaal/SpaGE)
- **Key innovation:** Integrates scRNA-seq with spatial data to predict unmeasured genes in the spatial dataset using domain adaptation.
- **Strengths:**
    - Imputes unmeasured genes in spatial data from scRNA-seq
    - Simple and fast computation
- **Limitations:**
    - Imputation accuracy decreases for genes with low spatial autocorrelation
    - Assumes scRNA-seq and spatial data share the same biological space
- **Technology compatibility:** Visium, MERFISH, seqFISH

### PRECAST
- **Paper:** [Nature Communications, 2023](https://doi.org/10.1038/s41467-023-35947-w)
- **Code:** [github.com/feiyoung/PRECAST](https://github.com/feiyoung/PRECAST)
- **Key innovation:** Probabilistic framework for joint spatial domain detection and batch integration across multiple spatial transcriptomics samples.
- **Strengths:**
    - Jointly integrates multiple samples with batch correction
    - Probabilistic framework provides uncertainty
    - Scalable to many samples
- **Limitations:**
    - Focused on within-modality (RNA) batch integration rather than cross-modality
    - R only
- **Technology compatibility:** Visium, Slide-seq

### Starfysh
- **Paper:** [Nature Biotechnology, 2024](https://doi.org/10.1038/s41587-024-02173-8)
- **Code:** [github.com/azizilab/starfysh](https://github.com/azizilab/starfysh)
- **Key innovation:** Semi-supervised deconvolution framework that integrates spatial transcriptomics with archetype analysis and histology for tissue-level multi-modal integration.
- **Strengths:**
    - Combines expression, spatial coordinates, and histology
    - Discovers tissue archetypes beyond predefined cell types
    - Semi-supervised mode handles incomplete annotations
- **Limitations:**
    - Archetype interpretation requires domain knowledge
    - Complex model with many components
- **Technology compatibility:** Visium

### MOFA-FLEX
- **Paper:** [Genome Biology, 2024](https://doi.org/10.1186/s13059-024-03224-6)
- **Code:** [github.com/bioFAM/MOFA2](https://github.com/bioFAM/MOFA2)
- **Key innovation:** Extension of MOFA+ for flexible multi-modal factor analysis that handles partially overlapping features and samples across modalities.
- **Strengths:**
    - Handles missing data and partial overlap between modalities
    - Interpretable factor model
    - Well-established framework
- **Limitations:**
    - Linear model may miss nonlinear cross-modal relationships
    - Not spatially-aware natively
- **Technology compatibility:** Any multi-modal data; spatial integration requires pairing with spatial methods

### LLOKI
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.11.19.624413)
- **Code:** [github.com/jfma-USTC/LLOKI](https://github.com/jfma-USTC/LLOKI)
- **Key innovation:** Language-model-based framework for integrating spatial omics with knowledge graphs, connecting molecular observations to curated biological knowledge.
- **Strengths:**
    - Connects spatial data to external knowledge bases
    - Language model enables natural-language queries of spatial results
- **Limitations:**
    - Emerging approach with limited validation
    - Language model interpretations require expert verification
- **Technology compatibility:** Any spatial platform

### pyWNN
- **Paper:** [Cell, 2021](https://doi.org/10.1016/j.cell.2021.04.048)
- **Code:** [github.com/scverse/muon](https://github.com/scverse/muon)
- **Key innovation:** Weighted nearest neighbor (WNN) integration from Seurat v4, reimplemented in Python within muon — weights each modality's contribution to cell similarity adaptively per cell.
- **Strengths:**
    - Per-cell modality weighting adapts to local data quality
    - Simple conceptual framework
    - Available in Python (muon) and R (Seurat)
- **Limitations:**
    - Not spatially-aware — treats cells as unordered
    - Works best for co-measured modalities (CITE-seq, multiome)
- **Technology compatibility:** Any multi-modal spatial data with co-measured modalities

## Benchmark Summary

Multi-modal spatial integration is a rapidly evolving area with limited systematic benchmarks. For **spatial RNA + protein** integration, SpatialGLUE is the current leader. For **Visium + scRNA-seq** integration, MOFA+ and Tangram (see [Deconvolution](deconvolution.md)) are most widely used. The totalVI model from scvi-tools remains strong for protein + RNA co-measured data. **True multi-modal spatial integration (>2 modalities) is still immature**, and most methods handle only two modalities at a time.

!!! tip "Start with the simplest integration"
    Before applying complex multi-modal methods, ask whether simple concatenation or WNN integration provides adequate results. Complex methods shine when modalities have different noise structures, but add overhead for well-behaved data.

!!! warning "Alignment assumptions"
    Cross-modal integration assumes that the modalities share a common biological space. When integrating spatial data with scRNA-seq from different donors, conditions, or preparation protocols, batch effects between modalities can dominate over biological signal.

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| Co-measured RNA + protein (spatial) | Joint spatial domain detection | SpatialGLUE | Purpose-built for multi-modal spatial data |
| Visium + H&E | Histology-guided spatial analysis | HEST or Starfysh | Connects pathology and molecular data |
| Multi-sample spatial | Batch integration across samples | PRECAST or moscot | Handle batch effects in spatial context |
| Spatial + scRNA-seq | Map scRNA-seq onto spatial data | Tangram (see Deconvolution) | Project single-cell modalities to space |
| Cross-platform spatial | Bridge resolution gaps | MISO | Integrates Visium with imaging-based data |
| Co-measured multi-modal | Simple weighted integration | pyWNN (muon) | Per-cell adaptive modality weighting |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| SpatialGLUE | Yes | - | - | - | - | Yes | - |
| HEST | Yes | - | - | - | - | - | - |
| moscot | Yes | - | - | Yes | - | - | - |
| MISO | Yes | - | Yes | Yes | - | - | - |
| SpaGE | Yes | - | - | Yes | - | - | - |
| PRECAST | Yes | - | - | - | - | - | - |
| Starfysh | Yes | - | - | - | - | - | - |
| MOFA-FLEX | Yes | - | - | - | - | - | - |
| pyWNN | Yes | - | Yes | Yes | Yes | Yes | - |
