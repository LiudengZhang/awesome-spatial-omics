# Spatial Deconvolution & Cell-Type Mapping

**Pipeline question:** What cell types are present at each spatial location, and in what proportions?

## Overview

Spot-level spatial technologies (Visium, Slide-seq) capture mixtures of multiple cells per measurement location. Deconvolution methods infer the cell-type composition of each spot using a single-cell RNA-seq reference. Even for single-cell-resolution platforms (Xenium, MERFISH), cell-type mapping tools assign identities to segmented cells. This step connects spatial data to the rich cell-type annotations available from scRNA-seq atlases.

## Key Methods

### Cell2location
- **Paper:** [Nature Biotechnology, 2022](https://doi.org/10.1038/s41587-021-01139-4)
- **Code:** [github.com/BayraktarLab/cell2location](https://github.com/BayraktarLab/cell2location)
- **Key innovation:** Hierarchical Bayesian model that decomposes spatial expression into cell-type contributions using reference gene expression signatures, with principled uncertainty estimation.
- **Strengths:**
    - Best overall accuracy in independent benchmarks
    - Provides posterior distributions, not just point estimates
    - Handles batch effects between reference and spatial data
- **Limitations:**
    - Requires GPU for practical runtimes
    - Training can be slow (hours for large datasets)
    - Sensitive to reference scRNA-seq quality
- **Technology compatibility:** Visium, Slide-seq, Stereo-seq

### RCTD / spacexr
- **Paper:** [Nature Biotechnology, 2022](https://doi.org/10.1038/s41587-021-00830-w)
- **Code:** [github.com/dmcable/spacexr](https://github.com/dmcable/spacexr)
- **Key innovation:** Supervised decomposition using Poisson regression with platform-specific gene expression references, supporting both full and doublet modes.
- **Strengths:**
    - Best speed-accuracy tradeoff — fast enough for routine use
    - Well-maintained R package (spacexr) with active development
    - Includes C-SIDE for spatially-aware differential expression
- **Limitations:**
    - Requires high-quality annotated scRNA-seq reference
    - Full deconvolution mode assumes maximum ~4 cell types per spot
    - R only
- **Technology compatibility:** Visium, Slide-seq, Visium HD

### CARD
- **Paper:** [Nature Biotechnology, 2022](https://doi.org/10.1038/s41587-022-01273-7)
- **Code:** [github.com/YingMa0107/CARD](https://github.com/YingMa0107/CARD)
- **Key innovation:** Conditional autoregressive model that leverages spatial correlation — nearby spots should have similar cell-type compositions.
- **Strengths:**
    - Spatial prior improves deconvolution at tissue boundaries
    - Also supports reference-free deconvolution mode
    - Fast computation
- **Limitations:**
    - Spatial smoothing may over-homogenize compositions near sharp boundaries
    - R only
- **Technology compatibility:** Visium, Slide-seq, ST

### Tangram
- **Paper:** [Nature Methods, 2021](https://doi.org/10.1038/s41592-021-01264-7)
- **Code:** [github.com/broadinstitute/Tangram](https://github.com/broadinstitute/Tangram)
- **Key innovation:** Maps individual scRNA-seq cells to spatial locations using optimal transport, enabling single-cell-resolution spatial reconstruction.
- **Strengths:**
    - Maps individual cells rather than proportions — preserves single-cell resolution
    - Can project any single-cell modality (RNA, ATAC, protein) onto spatial coordinates
    - Python/PyTorch implementation
- **Limitations:**
    - Mapping is not unique — multiple valid solutions may exist
    - Assumes scRNA-seq reference captures all cell types present in tissue
    - GPU recommended for large datasets
- **Technology compatibility:** Visium, Slide-seq, MERFISH, Xenium

### STdeconvolve
- **Paper:** [Nature Communications, 2022](https://doi.org/10.1038/s41467-022-28020-5)
- **Code:** [github.com/JEFworks-Lab/STdeconvolve](https://github.com/JEFworks-Lab/STdeconvolve)
- **Key innovation:** Reference-free deconvolution using Latent Dirichlet Allocation (LDA), discovering cell types directly from spatial data without a scRNA-seq reference.
- **Strengths:**
    - No reference required — useful when matched scRNA-seq is unavailable
    - Topic model approach is interpretable
    - Fast computation
- **Limitations:**
    - Discovered "topics" may not correspond to known cell types
    - Less accurate than reference-based methods when reference is available
- **Technology compatibility:** Visium, Slide-seq, ST

### SPOTlight
- **Paper:** [Nucleic Acids Research, 2021](https://doi.org/10.1093/nar/gkab043)
- **Code:** [github.com/MarcElosworthy/SPOTlight](https://github.com/MarcElosworthy/SPOTlight)
- **Key innovation:** NMF-based deconvolution using seeded topic models initialized with cell-type marker genes from scRNA-seq.
- **Strengths:**
    - Fast and lightweight
    - Simple NMF framework is interpretable
- **Limitations:**
    - Less accurate than probabilistic methods in benchmarks
    - Sensitive to marker gene selection
- **Technology compatibility:** Visium, ST

### CytoSPACE
- **Paper:** [Nature Biotechnology, 2023](https://doi.org/10.1038/s41587-023-01697-9)
- **Code:** [github.com/digitalcytometry/cytospace](https://github.com/digitalcytometry/cytospace)
- **Key innovation:** Optimal transport-based assignment of individual scRNA-seq cells to spatial locations at single-cell resolution.
- **Strengths:**
    - Single-cell resolution mapping (like Tangram but with different formulation)
    - Linear programming framework guarantees global optimum
    - Preserves cell-cell variability within types
- **Limitations:**
    - Requires that reference cell number matches or exceeds spot cell count
    - Computationally expensive for very large references
- **Technology compatibility:** Visium, Slide-seq

### DestVI
- **Paper:** [Nature Biotechnology, 2022](https://doi.org/10.1038/s41587-022-01272-8)
- **Code:** [github.com/scverse/scvi-tools](https://github.com/scverse/scvi-tools)
- **Key innovation:** Variational inference model that jointly deconvolves cell-type proportions and infers cell-type-specific gene expression per spot.
- **Strengths:**
    - Provides cell-type-specific expression per location, not just proportions
    - Part of the scvi-tools ecosystem
    - Deep generative model captures nonlinear effects
- **Limitations:**
    - Requires GPU and significant training time
    - Complex model with many hyperparameters
- **Technology compatibility:** Visium, Slide-seq

### spacedeconv
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.01.17.575523)
- **Code:** [github.com/omnideconv/spacedeconv](https://github.com/omnideconv/spacedeconv)
- **Key innovation:** Unified R framework for running and comparing multiple deconvolution methods through a single interface.
- **Strengths:**
    - Harmonized interface for 10+ deconvolution methods
    - Enables easy method comparison on the same data
    - Built-in evaluation metrics
- **Limitations:**
    - Wrapper — performance depends on underlying methods
    - R ecosystem may limit integration with Python workflows
- **Technology compatibility:** Visium, Slide-seq

### bulk2space
- **Paper:** [Nature Communications, 2023](https://doi.org/10.1038/s41467-023-36864-8)
- **Code:** [github.com/ZJUFanLab/bulk2space](https://github.com/ZJUFanLab/bulk2space)
- **Key innovation:** Generates spatially resolved single-cell data from bulk RNA-seq by leveraging spatial transcriptomics as a structural template.
- **Strengths:**
    - Enables spatial analysis of bulk RNA-seq data
    - Useful for large clinical cohorts without spatial data
- **Limitations:**
    - Generated spatial data is synthetic — results need careful validation
    - Accuracy depends on template spatial data quality
- **Technology compatibility:** Visium (as template), bulk RNA-seq (as input)

### TACCO
- **Paper:** [Nature Biotechnology, 2023](https://doi.org/10.1038/s41587-023-01657-3)
- **Code:** [github.com/simonwm/tacco](https://github.com/simonwm/tacco)
- **Key innovation:** Optimal transport-based framework for transferring categorical and continuous annotations from single-cell to spatial data.
- **Strengths:**
    - Transfers not just cell types but any continuous annotation
    - Flexible framework for multi-modal annotation transfer
    - Well-documented Python package
- **Limitations:**
    - Optimal transport can be slow for large datasets
    - Assumes reference and query share the same biological space
- **Technology compatibility:** Visium, Slide-seq, MERFISH

### InSituType
- **Paper:** [Nature Biotechnology, 2022](https://doi.org/10.1038/s41587-022-01282-6)
- **Code:** [github.com/Nanostring-Biostats/InSituType](https://github.com/Nanostring-Biostats/InSituType)
- **Key innovation:** Probabilistic cell-type assignment designed specifically for cell-level spatial data, combining supervised (reference-based) and unsupervised modes.
- **Strengths:**
    - Purpose-built for imaging-based platforms at cell resolution
    - Handles the limited gene panels of imaging platforms
    - Provides posterior probabilities for each cell-type assignment
- **Limitations:**
    - Designed for CosMx — less validated on other platforms
    - Supervised mode requires matched reference
- **Technology compatibility:** CosMx, Xenium, MERFISH

## Benchmark Summary

Independent benchmarks consistently rank **Cell2location** as the most accurate deconvolution method overall, particularly for complex tissues with many cell types. **RCTD** provides the best speed-accuracy tradeoff and is the recommended default for routine analyses. **Tangram** excels at mapping individual cells for multi-modal projection tasks. **Rare cell types remain the universal challenge** — all methods struggle with cell types comprising <5% of spots, and no method reliably detects very rare populations (<1%).

!!! warning "Reference quality is the bottleneck"
    The scRNA-seq reference matters more than the deconvolution algorithm. A high-quality, well-annotated reference from the same tissue type and species will outperform any method improvement. Always validate reference cell types before deconvolution.

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| Visium, have reference | Best accuracy | Cell2location | Top performer in benchmarks |
| Visium, need speed | Routine deconvolution | RCTD | Best speed-accuracy tradeoff |
| No scRNA-seq reference | Discover cell types | STdeconvolve | Reference-free LDA-based approach |
| Want single-cell mapping | Project scRNA-seq onto space | Tangram or CytoSPACE | Maps individual cells to locations |
| Imaging-based (CosMx) | Annotate segmented cells | InSituType | Designed for cell-resolution spatial data |
| Multiple methods | Compare deconvolution approaches | spacedeconv | Unified interface for method comparison |
| Want spatial-aware results | Leverage spatial smoothness | CARD | Spatial prior for deconvolution |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| Cell2location | Yes | - | - | - | - | - | Yes |
| RCTD | Yes | Yes | - | - | - | - | - |
| CARD | Yes | - | - | - | - | - | - |
| Tangram | Yes | - | Yes | Yes | - | - | - |
| STdeconvolve | Yes | - | - | - | - | - | - |
| SPOTlight | Yes | - | - | - | - | - | - |
| CytoSPACE | Yes | - | - | - | - | - | - |
| DestVI | Yes | - | - | - | - | - | - |
| TACCO | Yes | - | - | Yes | - | - | - |
| InSituType | - | - | Yes | Yes | Yes | - | - |
