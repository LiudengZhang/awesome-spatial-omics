# 3D Tissue Reconstruction

**Pipeline question:** How do we align and reconstruct three-dimensional tissue architecture from serial 2D spatial transcriptomics sections?

## Overview

Most spatial omics experiments produce 2D snapshots of tissue sections. 3D reconstruction methods align serial sections into volumetric representations, recovering the three-dimensional tissue architecture lost during sectioning. This is essential for understanding spatial organization that spans multiple tissue planes — vascular networks, tumor invasion fronts, and organ-scale gradients. The core computational challenge is registering sections that may differ in orientation, deformation, and missing tissue.

## Key Methods

### PASTE / PASTE2 / PASTE3
- **Paper:** [Nature Methods, 2022](https://doi.org/10.1038/s41592-022-01459-6) / [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02375-7)
- **Code:** [github.com/raphael-group/paste](https://github.com/raphael-group/paste)
- **Key innovation:** Optimal transport-based alignment of spatial transcriptomics sections using both gene expression and spatial coordinates; PASTE2 adds partial alignment for partially overlapping sections; PASTE3 adds scalability.
- **Strengths:**
    - Principled optimal transport framework
    - Handles both full and partial section overlap
    - PASTE3 scales to large datasets through GPU acceleration
    - Well-maintained with active development
- **Limitations:**
    - Optimal transport assumes a bijective mapping that may not hold for real tissue deformations
    - Performance degrades with large gaps between sections
    - Expression similarity assumes consecutive sections are transcriptomically similar
- **Technology compatibility:** Visium, Slide-seq, Stereo-seq

### STalign
- **Paper:** [Nature Communications, 2023](https://doi.org/10.1038/s41467-023-43915-7)
- **Code:** [github.com/JEFworks-Lab/STalign](https://github.com/JEFworks-Lab/STalign)
- **Key innovation:** Diffeomorphic alignment using Large Deformation Diffeomorphic Metric Mapping (LDDMM), which guarantees smooth, invertible transformations between sections.
- **Strengths:**
    - Diffeomorphic transformations are physically plausible — no folding or tearing
    - Handles nonlinear tissue deformations
    - Works with both image-based and transcript-based alignment
- **Limitations:**
    - LDDMM is computationally expensive
    - Requires good initialization for large deformations
    - Parameter tuning for regularization strength
- **Technology compatibility:** Visium, MERFISH, Xenium, any spatial platform with coordinates

### Spateo
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.08.08.607249)
- **Code:** [github.com/aristoteleo/spateo-release](https://github.com/aristoteleo/spateo-release)
- **Key innovation:** Comprehensive spatial transcriptomics framework that includes 3D reconstruction via mesh generation, morphometric analysis, and spatiotemporal modeling.
- **Strengths:**
    - Full 3D reconstruction pipeline from alignment to mesh generation
    - Includes downstream 3D spatial analysis tools
    - Models spatiotemporal dynamics in 3D
- **Limitations:**
    - Large framework with steep learning curve
    - 3D components still under active development
- **Technology compatibility:** Stereo-seq, Visium, Slide-seq

### SPIRAL
- **Paper:** [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02284-x)
- **Code:** [github.com/guott15/SPIRAL](https://github.com/guott15/SPIRAL)
- **Key innovation:** Spatial domain-guided 3D reconstruction that uses identified spatial domains as landmarks for aligning serial sections.
- **Strengths:**
    - Domain-guided alignment is biologically motivated
    - Reduces alignment ambiguity by leveraging tissue structure
- **Limitations:**
    - Requires spatial domain detection as a prerequisite
    - Performance depends on domain detection quality
- **Technology compatibility:** Visium, Slide-seq

### CalicoST
- **Paper:** [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02438-x)
- **Code:** [github.com/raphael-group/CalicoST](https://github.com/raphael-group/CalicoST)
- **Key innovation:** Detects allele-specific copy number aberrations in spatial transcriptomics data and aligns them across serial sections for 3D reconstruction of tumor clonal architecture.
- **Strengths:**
    - Combines copy number detection with 3D reconstruction
    - Reveals 3D tumor clonal architecture
    - Unique niche — no other method does spatial CNA + 3D alignment
- **Limitations:**
    - Specific to cancer/tumor applications
    - Requires sufficient read depth for allele-specific analysis
- **Technology compatibility:** Visium

### iStar
- **Paper:** [Nature, 2024](https://doi.org/10.1038/s41586-024-07504-y)
- **Code:** [github.com/CSOgroup/iStar](https://github.com/CSOgroup/iStar)
- **Key innovation:** Super-resolution spatial transcriptomics via image-guided expression imputation — enhances spatial resolution by predicting gene expression at sub-spot resolution using paired histology images.
- **Strengths:**
    - Achieves sub-spot resolution without higher-resolution technology
    - Leverages H&E images available for most Visium experiments
    - Useful as a preprocessing step before 3D reconstruction
- **Limitations:**
    - Imputed expression is predicted, not measured
    - Accuracy limited by histology-expression correlation
- **Technology compatibility:** Visium

### TOAST
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.03.07.583949)
- **Code:** [github.com/bstkj/TOAST](https://github.com/bstkj/TOAST)
- **Key innovation:** 3D alignment framework using tissue landmark registration with probabilistic matching of spatial features across sections.
- **Strengths:**
    - Landmark-based alignment is robust to tissue deformation
    - Probabilistic matching handles ambiguous correspondences
- **Limitations:**
    - Requires identifiable landmarks across sections
    - Preprint with limited validation
- **Technology compatibility:** Visium, Slide-seq

### SANTO
- **Paper:** [Nature Communications, 2024](https://doi.org/10.1038/s41467-024-49569-9)
- **Code:** [github.com/MIS-Lab/SANTO](https://github.com/MIS-Lab/SANTO)
- **Key innovation:** Spatial ANnotation Transfer and 3D recOnstruction — jointly performs cross-section annotation transfer and 3D reconstruction.
- **Strengths:**
    - Joint annotation and reconstruction avoids sequential error propagation
    - Transfers cell-type labels across aligned sections
- **Limitations:**
    - Multi-task optimization can be complex to tune
    - Performance depends on section quality
- **Technology compatibility:** Visium, Slide-seq

### TRACER
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.12.01.626230)
- **Code:** [github.com/Bao-Lab/TRACER](https://github.com/Bao-Lab/TRACER)
- **Key innovation:** Topology-aware 3D reconstruction that preserves topological features (holes, connected components) during section alignment.
- **Strengths:**
    - Topology preservation prevents biologically implausible reconstructions
    - Robust to section-to-section variation
- **Limitations:**
    - Topological constraints add computational complexity
    - Preprint stage
- **Technology compatibility:** Visium, Slide-seq

## Benchmark Summary

**PASTE** is the most established and widely cited method for spatial transcriptomics 3D reconstruction, with PASTE3 addressing earlier scalability limitations. **STalign** provides the most physically principled alignment through diffeomorphic mapping but at higher computational cost. The choice between optimal transport (PASTE) and diffeomorphic (STalign) approaches depends on the expected deformation type: PASTE handles well when sections are relatively similar, while STalign excels with significant nonlinear tissue deformations.

!!! tip "Section quality determines reconstruction quality"
    No computational method can compensate for poor tissue sectioning. Consistent section thickness, minimal tissue folding, and careful orientation during cutting are prerequisites for successful 3D reconstruction. Inspect each section visually before computational alignment.

!!! warning "3D reconstruction amplifies 2D errors"
    Any errors in 2D preprocessing (segmentation, domain detection) propagate and compound through 3D alignment. Validate 2D analyses on individual sections before attempting reconstruction.

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| Serial Visium sections | Standard 3D alignment | PASTE/PASTE3 | Most established, optimal transport framework |
| Sections with large deformations | Physically plausible alignment | STalign | Diffeomorphic mapping handles nonlinear deformation |
| Stereo-seq serial sections | Full 3D reconstruction pipeline | Spateo | End-to-end framework including mesh generation |
| Tumor serial sections | 3D clonal architecture | CalicoST | Combines CNA detection with 3D alignment |
| Visium with H&E | Enhance resolution before alignment | iStar | Image-guided sub-spot resolution enhancement |
| Cross-section annotation | Transfer labels across sections | SANTO | Joint annotation transfer and reconstruction |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| PASTE/2/3 | Yes | - | - | - | - | - | Yes |
| STalign | Yes | - | Yes | Yes | - | - | - |
| Spateo | Yes | - | - | - | - | - | Yes |
| SPIRAL | Yes | - | - | - | - | - | - |
| CalicoST | Yes | - | - | - | - | - | - |
| iStar | Yes | - | - | - | - | - | - |
| TOAST | Yes | - | - | - | - | - | - |
| SANTO | Yes | - | - | - | - | - | - |
| TRACER | Yes | - | - | - | - | - | - |
