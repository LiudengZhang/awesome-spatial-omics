# Cell-Type Annotation

**Pipeline question:** What cell type is each spatially-resolved cell, and how confident can that assignment be given limited gene panels?

## Overview

Cell-type annotation in spatial omics assigns biological identities to segmented cells or deconvolved spots. Unlike scRNA-seq where thousands of genes inform clustering and annotation, imaging-based spatial platforms (Xenium, MERFISH, CosMx) measure only hundreds of genes, making annotation more challenging. Dedicated spatial annotation methods leverage spatial context — the identity of neighboring cells — to improve assignments beyond what expression alone can achieve.

## Key Methods

### STELLAR
- **Paper:** [Nature Methods, 2022](https://doi.org/10.1038/s41592-022-01651-8)
- **Code:** [github.com/snap-stanford/stellar](https://github.com/snap-stanford/stellar)
- **Key innovation:** Graph neural network that transfers cell-type labels from annotated scRNA-seq to spatial data while automatically discovering novel cell types not present in the reference.
- **Strengths:**
    - Discovers novel spatial cell types absent from the reference
    - Leverages spatial graph structure for annotation
    - Most cited spatial annotation method
- **Limitations:**
    - Requires scRNA-seq reference with annotations
    - GNN training requires GPU
    - Novel type discovery can be overly aggressive
- **Technology compatibility:** MERFISH, Xenium, CosMx, any cell-resolution spatial platform

### InSituType
- **Paper:** [Nature Biotechnology, 2022](https://doi.org/10.1038/s41587-022-01282-6)
- **Code:** [github.com/Nanostring-Biostats/InSituType](https://github.com/Nanostring-Biostats/InSituType)
- **Key innovation:** Probabilistic cell-typing combining supervised (reference-guided) and unsupervised clustering, designed for the limited gene panels of imaging-based spatial platforms.
- **Strengths:**
    - Purpose-built for limited-panel spatial data
    - Provides posterior probabilities for type assignments
    - Semi-supervised mode handles missing cell types
- **Limitations:**
    - Originally designed for CosMx; less validated on other platforms
    - Requires careful reference selection for supervised mode
- **Technology compatibility:** CosMx, Xenium, MERFISH

### STEM
- **Paper:** [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02211-y)
- **Code:** [github.com/jlakkis/STEM](https://github.com/jlakkis/STEM)
- **Key innovation:** Foundation model approach for spatial cell-type annotation — trains on large-scale scRNA-seq and transfers to spatial data through a shared embedding space.
- **Strengths:**
    - Pre-trained model reduces need for dataset-specific training
    - Handles heterogeneous gene panels
- **Limitations:**
    - Foundation model approach is new with limited community validation
    - Performance on rare cell types is unclear
- **Technology compatibility:** Xenium, MERFISH, CosMx

### CelloType
- **Paper:** [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02235-4)
- **Code:** [github.com/AIMLab-UBC/CelloType](https://github.com/AIMLab-UBC/CelloType)
- **Key innovation:** Transformer-based model that annotates cells using spatial context and cell morphology from imaging data, not just gene expression.
- **Strengths:**
    - Incorporates morphological features from tissue images
    - Attention mechanism captures long-range spatial dependencies
- **Limitations:**
    - Requires paired expression and imaging data
    - Complex architecture with significant training requirements
- **Technology compatibility:** Xenium, MERFISH, any platform with paired images

### STALocator
- **Paper:** [Nucleic Acids Research, 2023](https://doi.org/10.1093/nar/gkad295)
- **Code:** [github.com/DongqingSun96/STALocator](https://github.com/DongqingSun96/STALocator)
- **Key innovation:** Deep learning method that locates spatial domains and annotates cell types by learning a shared representation between scRNA-seq and spatial data.
- **Strengths:**
    - Joint domain detection and cell annotation
    - Works with spot-level (Visium) and cell-level data
- **Limitations:**
    - Deep learning overhead for modest annotation tasks
    - Limited scalability evaluation
- **Technology compatibility:** Visium, Slide-seq, MERFISH

### TACIT
- **Paper:** [Nature Communications, 2024](https://doi.org/10.1038/s41467-024-47807-6)
- **Code:** [github.com/ChenMengjie/TACIT](https://github.com/ChenMengjie/TACIT)
- **Key innovation:** Annotation method that handles the technology gap between scRNA-seq references and spatial data by learning technology-invariant features.
- **Strengths:**
    - Explicitly addresses batch effects between reference and spatial data
    - Robust to gene panel differences
- **Limitations:**
    - Newer method with limited independent validation
    - Performance depends on overlap between reference and panel genes
- **Technology compatibility:** Xenium, MERFISH, CosMx

### TransST
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.06.12.598725)
- **Code:** [github.com/zy-wang-joe/TransST](https://github.com/zy-wang-joe/TransST)
- **Key innovation:** Transfer learning framework that adapts scRNA-seq-trained classifiers to spatial transcriptomics data through domain adaptation.
- **Strengths:**
    - Domain adaptation handles platform-specific biases
    - Leverages well-annotated scRNA-seq atlases
- **Limitations:**
    - Preprint stage
    - Transfer learning success depends on reference similarity
- **Technology compatibility:** Visium, MERFISH

### ABCT
- **Paper:** [Bioinformatics, 2024](https://doi.org/10.1093/bioinformatics/btae300)
- **Code:** [github.com/ChiuSky/ABCT](https://github.com/ChiuSky/ABCT)
- **Key innovation:** Annotation by combining transcript data with tissue context, using graph-based spatial smoothing to improve cell-type calls.
- **Strengths:**
    - Spatial smoothing reduces misannotation of isolated cells
    - Lightweight compared to deep learning methods
- **Limitations:**
    - Spatial smoothing may incorrectly reassign rare cells
    - Limited evaluation across platforms
- **Technology compatibility:** Xenium, MERFISH

## Benchmark Summary

No formal large-scale benchmark exists for spatial cell-type annotation. **STELLAR** is the most cited and widely used method, particularly for its ability to discover novel cell types. **InSituType** is the pragmatic choice for CosMx data and other imaging-based platforms. For Visium data, deconvolution methods (see [Deconvolution](deconvolution.md)) are typically preferred over direct annotation methods since spots contain cell mixtures.

!!! tip "Practical annotation strategy"
    For imaging-based data: start with standard clustering (Leiden on spatial-aware PCA) and manual marker-based annotation. Use STELLAR or InSituType when automated, reference-guided annotation is needed. Always validate against known marker genes.

!!! warning "Gene panel limitations"
    Imaging-based platforms measure 100-1000 genes selected for specific biological questions. Cell types not represented by panel genes will be misannotated or invisible. Always verify that the panel covers markers for expected cell types.

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| Cell-resolution spatial + reference | Annotate with novel type discovery | STELLAR | GNN-based, discovers unknown types |
| CosMx data | Platform-optimized annotation | InSituType | Purpose-built for CosMx gene panels |
| Any imaging-based + images | Use morphology for annotation | CelloType | Incorporates cell morphology |
| Visium spot-level | Cell-type annotation | Use deconvolution methods | Spots contain mixtures — see [Deconvolution](deconvolution.md) |
| Cross-platform transfer | Handle technology batch effects | TACIT | Technology-invariant feature learning |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| STELLAR | - | - | Yes | Yes | Yes | - | - |
| InSituType | - | - | Yes | Yes | Yes | - | - |
| STEM | - | - | Yes | Yes | Yes | - | - |
| CelloType | - | - | Yes | Yes | - | - | - |
| STALocator | Yes | - | - | Yes | - | - | - |
| TACIT | - | - | Yes | Yes | Yes | - | - |
| TransST | Yes | - | - | Yes | - | - | - |
| ABCT | - | - | Yes | Yes | - | - | - |
