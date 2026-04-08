# Preprocessing & Quality Control

**Pipeline question:** How do we clean spatial omics data — removing noise, correcting artifacts, and ensuring downstream analyses are not confounded by technical variation?

## Overview

Preprocessing and quality control in spatial omics extend well beyond standard scRNA-seq filtering. Spatial technologies introduce unique artifacts: tissue permeabilization causes mRNA diffusion across spot boundaries, imaging noise contaminates transcript detection, and tissue handling creates spatial batch effects. This step ensures that the signal used in all downstream analyses faithfully represents biology rather than technical noise.

## Key Methods

### SpotClean
- **Paper:** [Nature Communications, 2022](https://doi.org/10.1038/s41467-022-30587-y)
- **Code:** [github.com/zijianni/SpotClean](https://github.com/zijianni/SpotClean)
- **Key innovation:** Models mRNA bleeding across Visium spots using a contamination model and deconvolves the observed counts.
- **Strengths:**
    - Directly addresses the spot-level diffusion artifact unique to Visium
    - Probabilistic framework provides uncertainty estimates
- **Limitations:**
    - Designed specifically for Visium; not directly applicable to imaging-based platforms
    - Computationally intensive on large tissue sections
- **Technology compatibility:** Visium

### SpotSweeper
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.06.06.597765)
- **Code:** [github.com/MicTott/SpotSweeper](https://github.com/MicTott/SpotSweeper)
- **Key innovation:** Identifies and flags spatially-aware quality control outliers by leveraging local neighborhood statistics rather than global thresholds.
- **Strengths:**
    - Detects artifacts that global QC thresholds miss (e.g., local tissue damage)
    - Integrates with Bioconductor SpatialExperiment workflows
- **Limitations:**
    - R/Bioconductor only
    - Requires careful parameter tuning for different tissue types
- **Technology compatibility:** Visium, Visium HD, ST

### SpaNorm
- **Paper:** [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02516-0)
- **Code:** [github.com/PYangLab/SpaNorm](https://github.com/PYangLab/SpaNorm)
- **Key innovation:** Gene-length and library-size normalization designed for spatial data, accounting for spatial variation in capture efficiency.
- **Strengths:**
    - Explicitly models spatial variation in sequencing depth
    - Works as a drop-in replacement for standard normalization
- **Limitations:**
    - Focused on sequencing-based spatial platforms
    - Relatively new; limited independent validation
- **Technology compatibility:** Visium, Slide-seq, Stereo-seq

### Sprod
- **Paper:** [Nature Methods, 2022](https://doi.org/10.1038/s41592-022-01560-w)
- **Code:** [github.com/yachimay/Sprod](https://github.com/yachimay/Sprod)
- **Key innovation:** Denoises spatial transcriptomics data using latent graph representations that incorporate both expression and spatial information.
- **Strengths:**
    - Jointly uses spatial location and expression for denoising
    - Improves downstream clustering and DE results
- **Limitations:**
    - Graph construction can be slow for high-resolution datasets
    - Performance depends on neighborhood size parameter
- **Technology compatibility:** Visium, Slide-seq

### DenoIST
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.10.25.620264)
- **Code:** [github.com/Bao-Lab/DenoIST](https://github.com/Bao-Lab/DenoIST)
- **Key innovation:** Deep learning-based denoising for imaging-based spatial transcriptomics, targeting spot detection noise in FISH-based platforms.
- **Strengths:**
    - Specifically addresses false positive/negative transcript detection in MERFISH and similar platforms
    - Learns platform-specific noise patterns
- **Limitations:**
    - Requires training data or pretrained models per platform
    - Limited to imaging-based spatial transcriptomics
- **Technology compatibility:** MERFISH, seqFISH

### sopa
- **Paper:** [Nature Communications, 2024](https://doi.org/10.1038/s41467-024-48981-z)
- **Code:** [github.com/gustaveroussy/sopa](https://github.com/gustaveroussy/sopa)
- **Key innovation:** A unified preprocessing pipeline for imaging-based spatial omics that handles tiling, segmentation, aggregation, and QC in a scalable framework.
- **Strengths:**
    - Technology-agnostic: works with Xenium, MERFISH, CosMx, CODEX, and more
    - Snakemake-based pipeline enables reproducible and parallelized processing
    - Actively maintained with strong community adoption
- **Limitations:**
    - Pipeline complexity can be steep for simple analyses
    - Primarily focused on imaging-based platforms
- **Technology compatibility:** Xenium, MERFISH, CosMx, CODEX, Stereo-seq, PhenoCycler

### cellAdmix
- **Paper:** [bioRxiv, 2025](https://doi.org/10.1101/2025.02.25.640205)
- **Code:** [github.com/cellAdmix/cellAdmix](https://github.com/cellAdmix/cellAdmix)
- **Key innovation:** Detects and corrects cell admixture — contamination from neighboring cells — in imaging-based spatial data.
- **Strengths:**
    - Addresses a specific artifact of imperfect segmentation
    - Improves cell-type assignment accuracy
- **Limitations:**
    - Emerging tool; limited benchmarking across platforms
    - Requires prior cell-type annotation or reference
- **Technology compatibility:** Xenium, MERFISH, CosMx

### MisTIC
- **Paper:** [NAR Genomics and Bioinformatics, 2022](https://doi.org/10.1093/nargab/lqac061)
- **Code:** [github.com/MISTICPipeline/MisTIC](https://github.com/MISTICtool/MisTIC)
- **Key innovation:** Visualization tool for multiplex imaging QC, enabling rapid assessment of channel crosstalk and staining quality.
- **Strengths:**
    - Interactive visualization for identifying imaging artifacts
    - Useful for protein-level spatial platforms
- **Limitations:**
    - QC/visualization only — does not correct artifacts
    - Focused on multiplex imaging (CODEX, CyCIF)
- **Technology compatibility:** CODEX, CyCIF, mIHC

### ResolVI
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.12.05.627022)
- **Code:** [github.com/scverse/scvi-tools](https://github.com/scverse/scvi-tools)
- **Key innovation:** Variational inference model within scvi-tools that jointly models segmentation errors and background noise for imaging-based spatial data.
- **Strengths:**
    - Probabilistic framework handles multiple noise sources simultaneously
    - Integrated into the scvi-tools ecosystem
- **Limitations:**
    - Requires GPU for practical runtimes
    - Still in preprint stage
- **Technology compatibility:** MERFISH, Xenium, CosMx

### SPLIT
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.11.27.625788)
- **Code:** [github.com/sggao/SPLIT](https://github.com/sggao/SPLIT)
- **Key innovation:** Identifies and separates signal from noise in spatial transcriptomics at the gene level using spatial autocorrelation patterns.
- **Strengths:**
    - Gene-level QC rather than spot/cell-level
    - Platform-agnostic approach
- **Limitations:**
    - Preprint stage with limited validation
    - May be overly conservative for lowly-expressed genes
- **Technology compatibility:** Visium, MERFISH, Xenium

### TISSUE
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.05.06.592773)
- **Code:** [github.com/leiwangUCSD/TISSUE](https://github.com/leiwangUCSD/TISSUE)
- **Key innovation:** Tissue-level quality scoring that provides prediction intervals for gene expression, quantifying uncertainty per spot.
- **Strengths:**
    - Provides uncertainty quantification, not just point estimates
    - Can guide which regions to trust in downstream analyses
- **Limitations:**
    - Adds computational overhead for uncertainty estimation
    - New method with limited adoption
- **Technology compatibility:** Visium, Slide-seq

## Benchmark Summary

No formal, systematic benchmark exists for spatial omics preprocessing tools. The field is fragmented because different technologies produce fundamentally different artifacts: sequencing-based platforms (Visium) suffer from mRNA diffusion, while imaging-based platforms (MERFISH, Xenium) suffer from segmentation-coupled noise. **SpotClean** is the most validated tool for Visium-specific diffusion correction, while **sopa** has emerged as the most widely adopted preprocessing pipeline for imaging-based platforms due to its technology-agnostic design and active maintenance.

!!! tip "Practical recommendation"
    Start with standard scRNA-seq QC metrics (total counts, gene counts, mitochondrial fraction) applied spatially. Add SpotClean for Visium data and sopa for imaging-based data. Use SpotSweeper to catch spatially-localized artifacts that global thresholds miss.

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| Visium with visible bleeding | Remove mRNA diffusion | SpotClean | Only tool specifically modeling Visium spot contamination |
| Any sequencing-based spatial | Spatially-aware normalization | SpaNorm | Accounts for spatial capture efficiency variation |
| Imaging-based (Xenium, MERFISH) | Full preprocessing pipeline | sopa | Technology-agnostic, scalable, actively maintained |
| Imaging-based with poor segmentation | Correct cell admixture | cellAdmix or ResolVI | Both address segmentation-induced contamination |
| Any platform | Detect local tissue artifacts | SpotSweeper | Spatial outlier detection catches what global QC misses |
| Multiplex imaging (CODEX) | Visual QC of channels | MisTIC | Interactive channel crosstalk assessment |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| SpotClean | Yes | - | - | - | - | - | - |
| SpotSweeper | Yes | Yes | - | - | - | - | - |
| SpaNorm | Yes | - | - | - | - | - | Yes |
| Sprod | Yes | - | - | - | - | - | - |
| DenoIST | - | - | - | Yes | - | - | - |
| sopa | - | - | Yes | Yes | Yes | Yes | Yes |
| cellAdmix | - | - | Yes | Yes | Yes | - | - |
| MisTIC | - | - | - | - | - | Yes | - |
| ResolVI | - | - | Yes | Yes | Yes | - | - |
| SPLIT | Yes | - | Yes | Yes | - | - | - |
| TISSUE | Yes | - | - | - | - | - | - |

!!! warning "No universal preprocessing exists"
    Unlike scRNA-seq where scanpy/Seurat pipelines are near-universal, spatial omics preprocessing must be tailored to the specific technology. Always verify that the chosen tool was designed for or validated on the platform being used.
