# Spatially-Aware Differential Expression

**Pipeline question:** Which genes are differentially expressed across spatial contexts — between spatial domains, near specific cell types, or along spatial gradients?

## Overview

Spatially-aware differential expression (DE) extends standard DE testing by incorporating spatial information. Rather than comparing predefined groups (as in bulk or scRNA-seq DE), spatial DE tests whether gene expression changes as a function of spatial position, neighborhood composition, or proximity to specific structures. This is distinct from spatially variable gene detection (which asks "is this gene spatially patterned?") — spatial DE asks "is this gene differentially expressed between spatial contexts?"

## Key Methods

### C-SIDE (spacexr)
- **Paper:** [Nature Biotechnology, 2022](https://doi.org/10.1038/s41587-021-00830-w)
- **Code:** [github.com/dmcable/spacexr](https://github.com/dmcable/spacexr)
- **Key innovation:** Cell-type-specific spatially-informed differential expression within the spacexr framework — tests for DE while accounting for cell-type mixtures in each spot.
- **Strengths:**
    - Handles cell-type mixtures natively (uses RCTD deconvolution)
    - Tests for cell-type-specific spatial expression changes
    - Well-integrated with RCTD deconvolution in a single package
- **Limitations:**
    - Requires prior deconvolution via RCTD
    - R only
    - Designed for spot-level (Visium) data
- **Technology compatibility:** Visium, Slide-seq

### Niche-DE
- **Paper:** [Nature Genetics, 2024](https://doi.org/10.1038/s41588-024-01994-2)
- **Code:** [github.com/KChen-lab/Niche-DE](https://github.com/KChen-lab/Niche-DE)
- **Key innovation:** Tests whether genes in a focal cell type are differentially expressed depending on the cell-type composition of the spatial neighborhood (niche).
- **Strengths:**
    - Directly tests the niche hypothesis — does neighborhood composition affect expression?
    - Identifies niche-associated genes per cell type
    - Handles both spot-level and cell-level data
- **Limitations:**
    - Requires accurate cell-type annotations or deconvolution
    - Statistical power depends on sufficient niche variation
- **Technology compatibility:** Visium, MERFISH, Xenium, any spatial platform with cell-type labels

### Vespucci
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.01.10.575115)
- **Code:** [github.com/smiranast/Vespucci](https://github.com/smiranast/Vespucci)
- **Key innovation:** Framework for DE analysis that models gene expression as a function of spatial covariates, enabling flexible spatial regression testing.
- **Strengths:**
    - Flexible covariate-based testing (spatial coordinates, distance to structure, etc.)
    - Can test complex spatial hypotheses
- **Limitations:**
    - Requires careful specification of spatial covariates
    - Preprint with limited community adoption
- **Technology compatibility:** Visium, Slide-seq

### CSDE
- **Paper:** [Bioinformatics, 2023](https://doi.org/10.1093/bioinformatics/btad545)
- **Code:** [github.com/Uauy-Lab/CSDE](https://github.com/Uauy-Lab/CSDE)
- **Key innovation:** Cell-type-specific differential expression that decomposes spot-level changes into cell-type-specific components without requiring single-cell resolution.
- **Strengths:**
    - Attributes spot-level DE to specific cell types
    - Works with spot-level data where individual cells are not resolved
- **Limitations:**
    - Accuracy depends on deconvolution quality
    - Limited validation on imaging-based platforms
- **Technology compatibility:** Visium, Slide-seq

### spatialGE
- **Paper:** [Bioinformatics, 2024](https://doi.org/10.1093/bioinformatics/btae032)
- **Code:** [github.com/FridleyLab/spatialGE](https://github.com/FridleyLab/spatialGE)
- **Key innovation:** R toolkit for spatial gene expression analysis that includes spatial DE testing alongside visualization and spatial statistics.
- **Strengths:**
    - Integrated toolkit covering multiple spatial analysis tasks
    - Good visualization capabilities
    - Designed for clinical/translational spatial studies
- **Limitations:**
    - Jack-of-all-trades — DE component is not as specialized as dedicated methods
    - R only
- **Technology compatibility:** Visium, CosMx, any spatial platform

## Benchmark Summary

No systematic benchmark currently compares spatial DE methods head-to-head. **C-SIDE** is the most established method through its integration with RCTD/spacexr, providing a natural deconvolution-to-DE pipeline for Visium data. **Niche-DE** addresses a unique and important question — niche-associated expression changes — that other methods do not directly test. For standard two-group spatial DE (e.g., tumor vs. stroma), Wilcoxon or edgeR/DESeq2 applied to spatial domains remains a practical and valid approach.

!!! tip "Standard DE tools still work"
    For comparing predefined spatial regions (e.g., tumor core vs. margin), standard scRNA-seq DE tools (Wilcoxon, edgeR, DESeq2) applied to spatial domain labels are effective. Spatial DE methods add value when the question involves gradients, niches, or cell-type-specific spatial effects.

!!! warning "Spatial autocorrelation inflates significance"
    Standard DE tests assume independent observations. Spatially adjacent spots are correlated, inflating test statistics and producing false positives. Methods like C-SIDE and Niche-DE account for this; standard tests applied to spatial data should use spatial bootstrap or downsampling for calibration.

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| Visium with RCTD deconvolution | Cell-type-specific spatial DE | C-SIDE | Integrated deconvolution-to-DE pipeline |
| Cell-resolution spatial | Test niche effects on expression | Niche-DE | Directly tests neighborhood composition effects |
| Any spatial data, two groups | Compare spatial regions | Standard DE (Wilcoxon/edgeR) | Simple, effective for discrete region comparisons |
| Spot-level, want cell-type DE | Decompose spot-level changes | CSDE | Attributes changes to cell types in mixtures |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| C-SIDE | Yes | - | - | - | - | - | - |
| Niche-DE | Yes | - | Yes | Yes | Yes | - | - |
| Vespucci | Yes | - | - | - | - | - | - |
| CSDE | Yes | - | - | - | - | - | - |
| spatialGE | Yes | - | - | - | Yes | - | - |
