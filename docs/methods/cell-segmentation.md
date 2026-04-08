# Cell Segmentation

**Pipeline question:** How do we identify individual cell boundaries in spatial omics data to assign transcripts or protein signals to specific cells?

## Overview

Cell segmentation is arguably the most consequential step in spatial omics analysis — every downstream result depends on correctly assigning molecular signals to individual cells. Errors propagate silently: over-segmentation splits real cells into fragments that look like novel cell types, while under-segmentation merges neighbors into chimeric profiles. The field has developed three broad strategies: image-based methods that segment nuclear/membrane stains, transcript-based methods that cluster molecules directly, and hybrid approaches that combine both signals.

## Image-Based Methods

### Cellpose / Cellpose 2 / Cellpose 3
- **Paper:** [Nature Methods, 2021](https://doi.org/10.1038/s41592-020-01018-x) / [Nature Methods, 2022](https://doi.org/10.1038/s41592-022-01663-4) / [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02233-6)
- **Code:** [github.com/MouseLand/cellpose](https://github.com/MouseLand/cellpose)
- **Key innovation:** Gradient-flow representation of cell shapes that generalizes across cell morphologies; Cellpose 3 adds a transformer backbone and image restoration.
- **Strengths:**
    - Best-in-class generalization across tissue types without retraining
    - Human-in-the-loop training for custom models (Cellpose 2)
    - Built-in image denoising and deblurring (Cellpose 3)
- **Limitations:**
    - Requires nuclear or membrane staining images
    - Struggles with highly elongated or irregular cell shapes without fine-tuning
    - GPU recommended for large images
- **Technology compatibility:** Xenium, MERFISH, CosMx, CODEX, any platform with DAPI/membrane stains

### StarDist
- **Paper:** [MICCAI, 2018](https://doi.org/10.1007/978-3-030-00934-2_30)
- **Code:** [github.com/stardist/stardist](https://github.com/stardist/stardist)
- **Key innovation:** Predicts star-convex polygons for each nucleus, enabling fast and robust nuclear detection.
- **Strengths:**
    - Very fast inference
    - Excellent for round/convex nuclei (common in many tissues)
    - Well-established with large user base
- **Limitations:**
    - Star-convex assumption fails for highly irregular shapes
    - Nuclear segmentation only — no cytoplasm estimation
- **Technology compatibility:** Any platform with nuclear staining images

### Mesmer / DeepCell
- **Paper:** [Nature Biotechnology, 2022](https://doi.org/10.1038/s41587-021-01094-0)
- **Code:** [github.com/vanvalenlab/deepcell-tf](https://github.com/vanvalenlab/deepcell-tf)
- **Key innovation:** Panoptic segmentation model trained on TissueNet, the largest manually annotated cell segmentation dataset.
- **Strengths:**
    - Trained on diverse tissue types from TissueNet
    - Predicts both nuclear and whole-cell segmentation
    - Cloud deployment available via DeepCell.org
- **Limitations:**
    - Large model with significant memory requirements
    - Less flexible than Cellpose for custom fine-tuning
- **Technology compatibility:** CODEX, MIBI, CyCIF, any platform with nuclear + membrane stains

### CellSAM
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2023.11.17.567630)
- **Code:** [github.com/vanvalenlab/cellSAM](https://github.com/vanvalenlab/cellSAM)
- **Key innovation:** Adapts the Segment Anything Model (SAM) foundation model for cell segmentation, combining a cell-finding model with SAM's prompt-based segmentation.
- **Strengths:**
    - Zero-shot generalization across modalities
    - Foundation model approach reduces need for task-specific training
- **Limitations:**
    - Computationally expensive for large tissue sections
    - Performance varies with cell density and tissue type
- **Technology compatibility:** Xenium, MERFISH, CosMx, CODEX, brightfield, fluorescence

## Transcript-Based Methods

### Baysor
- **Paper:** [Nature Biotechnology, 2022](https://doi.org/10.1038/s41587-021-01044-w)
- **Code:** [github.com/kharchenkolab/Baysor](https://github.com/kharchenkolab/Baysor)
- **Key innovation:** Bayesian segmentation using transcript spatial distributions — assigns transcripts to cells without requiring any staining image.
- **Strengths:**
    - Works without nuclear/membrane staining
    - Can refine image-based segmentation using transcript patterns
    - Handles overlapping cells well
- **Limitations:**
    - Slower than image-based methods
    - Requires sufficient transcript density for reliable boundary estimation
    - Julia-based, which limits integration with Python/R ecosystems
- **Technology compatibility:** MERFISH, Xenium, seqFISH, any transcript-resolution platform

### Proseg
- **Paper:** [Nature Methods, 2025](https://doi.org/10.1038/s41592-025-02594-2)
- **Code:** [github.com/dcjones/proseg](https://github.com/dcjones/proseg)
- **Key innovation:** Probabilistic transcript-to-cell assignment using a generative model that jointly infers cell boundaries and expression profiles.
- **Strengths:**
    - Fast Rust implementation
    - Handles nuclear versus cytoplasmic transcript localization
    - Provides uncertainty in transcript assignment
- **Limitations:**
    - Relatively new with limited community validation
    - Requires sufficient gene panel diversity
- **Technology compatibility:** Xenium, MERFISH, CosMx

### ComSeg
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.12.12.628073)
- **Code:** [github.com/fish-quant/ComSeg](https://github.com/fish-quant/ComSeg)
- **Key innovation:** Community detection on transcript point clouds to identify cells without requiring prior segmentation.
- **Strengths:**
    - Graph-based approach naturally handles varying cell densities
    - No dependency on image data
- **Limitations:**
    - Emerging method with limited benchmarking
    - Scalability on very large datasets not fully characterized
- **Technology compatibility:** MERFISH, seqFISH, smFISH

### Segger
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.10.21.619329)
- **Code:** [github.com/EliHei2/segger_dev](https://github.com/EliHei2/segger_dev)
- **Key innovation:** Graph neural network that learns transcript-to-cell assignment as a link prediction task on spatial transcript graphs.
- **Strengths:**
    - Deep learning approach captures complex spatial patterns
    - Scalable to large datasets
- **Limitations:**
    - Requires training data or pretrained models
    - GNN complexity adds overhead
- **Technology compatibility:** Xenium, MERFISH

### Bering
- **Paper:** [Nature Communications, 2024](https://doi.org/10.1038/s41467-024-47981-7)
- **Code:** [github.com/Bering-Bhavsar/Bering](https://github.com/jian-shu-lab/Bering)
- **Key innovation:** Graph neural network for joint cell segmentation and annotation from spatial transcriptomics data.
- **Strengths:**
    - Simultaneously segments and annotates cells
    - Leverages single-cell reference data
- **Limitations:**
    - Requires scRNA-seq reference for annotation guidance
    - Complex training pipeline
- **Technology compatibility:** MERFISH, Xenium, seqFISH

### ClusterMap
- **Paper:** [Nature Methods, 2021](https://doi.org/10.1038/s41592-021-01236-x)
- **Code:** [github.com/wanglab-broad/ClusterMap](https://github.com/wanglab-broad/ClusterMap)
- **Key innovation:** Clusters spatially-resolved transcripts into cells using a density-based approach on physical and gene-expression coordinates.
- **Strengths:**
    - No staining image required
    - Conceptually simple density-based clustering
- **Limitations:**
    - Sensitive to density parameters
    - Struggles with tightly packed tissues
- **Technology compatibility:** MERFISH, STARmap, seqFISH

### BIDCell
- **Paper:** [Nature Communications, 2024](https://doi.org/10.1038/s41467-024-47186-6)
- **Code:** [github.com/SydneyBioX/BIDCell](https://github.com/SydneyBioX/BIDCell)
- **Key innovation:** Deep learning segmentation guided by biological priors — uses cell-type-specific gene expression to inform segmentation boundaries.
- **Strengths:**
    - Biological-prior-driven segmentation improves cell-type purity
    - Self-supervised training reduces annotation burden
- **Limitations:**
    - Training requires gene panel information
    - Performance depends on quality of biological priors
- **Technology compatibility:** Xenium, MERFISH, CosMx

## Segmentation-Free Approaches

### SSAM
- **Paper:** [Nature Communications, 2021](https://doi.org/10.1038/s41467-021-21349-x)
- **Code:** [github.com/eilslabs/ssam](https://github.com/eilslabs/ssam)
- **Key innovation:** Analyzes spatial gene expression as a continuous field via kernel density estimation, bypassing cell segmentation entirely.
- **Strengths:**
    - Avoids all segmentation artifacts
    - Works well for tissue-level domain identification
- **Limitations:**
    - Cannot provide single-cell resolution results
    - Loses cell-level heterogeneity information
- **Technology compatibility:** MERFISH, seqFISH, osmFISH

### Points2Regions
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.07.02.601326)
- **Code:** [github.com/wahlby-lab/Points2Regions](https://github.com/wahlby-lab/Points2Regions)
- **Key innovation:** Directly maps transcript point clouds to tissue regions without intermediate cell segmentation.
- **Strengths:**
    - Fast and scalable
    - Useful when cell boundaries are unreliable
- **Limitations:**
    - Provides region-level, not cell-level, results
    - Less informative for cell-type-specific analyses
- **Technology compatibility:** Any transcript-resolution platform

## Visium HD-Specific Methods

### Bin2Cell
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.09.02.610703)
- **Code:** [github.com/10XGenomics/spaceranger](https://github.com/10XGenomics/spaceranger)
- **Key innovation:** Assigns Visium HD 2-micron bins to cells using paired H&E images, bridging sequencing-based and imaging-based spatial data.
- **Strengths:**
    - Native 10x Genomics solution for Visium HD
    - Leverages existing H&E infrastructure
- **Limitations:**
    - Dependent on H&E image quality
    - Limited to Visium HD platform
- **Technology compatibility:** Visium HD

### ENACT
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.06.04.597233)
- **Code:** [github.com/BiomedicalMachineLearning/ENACT](https://github.com/BiomedicalMachineLearning/ENACT)
- **Key innovation:** Enhanced cell segmentation for Visium HD using graph-based bin-to-cell assignment with morphological features.
- **Strengths:**
    - Improves on default Visium HD cell assignment
    - Uses graph neural networks for spatial reasoning
- **Limitations:**
    - Visium HD-specific
    - Requires paired histology images
- **Technology compatibility:** Visium HD

### STHD
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.06.20.599803)
- **Code:** [github.com/siyuh/STHD](https://github.com/siyuh/STHD)
- **Key innovation:** Directly analyzes Visium HD bin-level data for cell-type annotation without requiring prior cell segmentation.
- **Strengths:**
    - Avoids segmentation errors in Visium HD analysis
    - Fast bin-level annotation
- **Limitations:**
    - Bin-level results may not correspond to individual cells
    - Platform-specific
- **Technology compatibility:** Visium HD

## Benchmark Summary

Systematic benchmarks consistently show that **Cellpose 2/3** is the best general-purpose tool for image-based nuclear segmentation across tissue types and staining protocols. For transcript-only segmentation, **Baysor** remains the gold standard, though **Proseg** shows competitive speed with comparable accuracy. **Hybrid approaches** — using image-based segmentation refined by transcript assignment (e.g., Cellpose + Baysor) — often outperform either strategy alone, particularly in tissues with complex morphology.

!!! tip "Best practice"
    When both staining images and transcripts are available, run image-based segmentation first (Cellpose 3), then refine transcript assignment with Baysor or Proseg. Compare results with and without refinement to assess improvement.

!!! warning "Segmentation errors are silent"
    Unlike many analysis steps where errors produce obviously wrong results, segmentation errors are invisible in downstream analysis. Over-segmented cells simply appear as a new "cell type." Always visually inspect segmentation results on representative tissue regions before proceeding.

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| DAPI/membrane stains available | General cell segmentation | Cellpose 3 | Best generalization, built-in image restoration |
| Round nuclei, speed needed | Fast nuclear detection | StarDist | Fastest inference for convex nuclei |
| Transcript-only, no staining | Segment from molecules | Baysor | Most validated transcript-based method |
| Transcripts + staining | Highest accuracy | Cellpose + Baysor (hybrid) | Image boundaries refined by molecular signal |
| CODEX/CyCIF with membrane markers | Whole-cell segmentation | Mesmer/DeepCell | Trained on TissueNet with membrane data |
| Visium HD bins | Assign bins to cells | Bin2Cell or ENACT | Purpose-built for Visium HD |
| Cell boundaries unreliable | Skip segmentation | SSAM or Points2Regions | Continuous field or region-level analysis |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| Cellpose 3 | - | - | Yes | Yes | Yes | Yes | - |
| StarDist | - | - | Yes | Yes | Yes | Yes | - |
| Mesmer/DeepCell | - | - | Yes | Yes | Yes | Yes | - |
| CellSAM | - | - | Yes | Yes | Yes | Yes | - |
| Baysor | - | - | Yes | Yes | Yes | - | - |
| Proseg | - | - | Yes | Yes | Yes | - | - |
| BIDCell | - | - | Yes | Yes | Yes | - | - |
| ClusterMap | - | - | - | Yes | - | - | - |
| SSAM | - | - | - | Yes | - | - | - |
| Bin2Cell | - | Yes | - | - | - | - | - |
| ENACT | - | Yes | - | - | - | - | - |
| STHD | - | Yes | - | - | - | - | - |
