# Spatial Domain Identification

**Pipeline question:** What are the distinct tissue regions (spatial domains) defined by coherent gene expression patterns, and how do they relate to histological structures?

## Overview

Spatial domain identification groups spots or cells into contiguous tissue regions that share similar transcriptomic profiles. Unlike standard clustering of dissociated cells, spatial domain methods must balance transcriptomic similarity with spatial coherence — two spots with identical expression profiles should be in the same domain only if they are spatially contiguous. This step reveals tissue architecture at a level that bridges molecular profiles and histological annotations.

!!! tip "Domains vs. niches"
    Spatial domains are contiguous tissue regions with coherent expression. Cellular niches are local microenvironments defined by cell-type composition and interactions. See the [awesome-spatial-omics-niche](https://github.com/your-repo/awesome-spatial-omics-niche) resource for niche-specific methods.

## Key Methods

### BANKSY
- **Paper:** [Nature Genetics, 2024](https://doi.org/10.1038/s41588-024-01664-3)
- **Code:** [github.com/prabhakarlab/Banksy_py](https://github.com/prabhakarlab/Banksy_py)
- **Key innovation:** Augments each cell's features with the mean and gradient of neighbor expression (azimuthal Gabor filter), then applies standard clustering — simple, fast, and highly effective.
- **Strengths:**
    - Conceptually simple: augment features, then cluster
    - Scalable to millions of cells
    - Works across sequencing-based and imaging-based platforms
- **Limitations:**
    - Neighborhood radius is a key parameter that requires tuning
    - Does not explicitly model number of domains
- **Technology compatibility:** Visium, Visium HD, Xenium, MERFISH, CosMx, Slide-seq, Stereo-seq

### SpaGCN
- **Paper:** [Nature Methods, 2021](https://doi.org/10.1038/s41592-021-01255-8)
- **Code:** [github.com/jianhuupenn/SpaGCN](https://github.com/jianhuupenn/SpaGCN)
- **Key innovation:** Graph convolutional network that integrates gene expression, spatial location, and histology image features for domain identification.
- **Strengths:**
    - Integrates histology information when available
    - Identifies spatially variable genes per domain
- **Limitations:**
    - GCN training can be unstable with default hyperparameters
    - Limited scalability to very large datasets
- **Technology compatibility:** Visium, ST

### STAGATE
- **Paper:** [Nature Communications, 2022](https://doi.org/10.1038/s41467-022-29439-6)
- **Code:** [github.com/QIFEIDKN/STAGATE_pyG](https://github.com/QIFEIDKN/STAGATE_pyG)
- **Key innovation:** Graph attention autoencoder that learns spatial domain-aware embeddings by selectively attending to informative spatial neighbors.
- **Strengths:**
    - Attention mechanism identifies which neighbors matter most
    - Top performer in multiple benchmarks
    - Supports 3D spatial data
- **Limitations:**
    - Requires GPU for practical runtimes
    - Sensitive to graph construction parameters
- **Technology compatibility:** Visium, Slide-seq, Stereo-seq, MERFISH

### BayesSpace
- **Paper:** [Nature Biotechnology, 2021](https://doi.org/10.1038/s41587-021-00935-2)
- **Code:** [github.com/edward130603/BayesSpace](https://github.com/edward130603/BayesSpace)
- **Key innovation:** Bayesian spatial clustering with Markov random field prior that encourages spatial smoothness, plus sub-spot resolution enhancement.
- **Strengths:**
    - Principled Bayesian framework with uncertainty quantification
    - Sub-spot resolution enhancement for Visium
    - Statistically well-grounded
- **Limitations:**
    - Specifically designed for Visium's hexagonal grid — does not generalize to irregular coordinates
    - Computationally expensive MCMC inference
    - R/Bioconductor only
- **Technology compatibility:** Visium only

### GraphST
- **Paper:** [Nature Communications, 2023](https://doi.org/10.1038/s41467-023-36796-3)
- **Code:** [github.com/JinmiaoChenLab/GraphST](https://github.com/JinmiaoChenLab/GraphST)
- **Key innovation:** Graph self-supervised contrastive learning framework that learns spatial representations via data augmentation and contrastive objectives.
- **Strengths:**
    - Self-supervised — no labels required
    - Consistently top performer across benchmarks
    - Also performs batch integration for multi-slice data
- **Limitations:**
    - Multiple loss terms require careful balancing
    - GPU required
- **Technology compatibility:** Visium, Slide-seq, Stereo-seq, MERFISH

### BASS
- **Paper:** [Genome Biology, 2022](https://doi.org/10.1186/s13059-022-02734-7)
- **Code:** [github.com/zhengli09/BASS](https://github.com/zhengli09/BASS)
- **Key innovation:** Multi-scale Bayesian model that simultaneously identifies cell types and spatial domains in a unified framework.
- **Strengths:**
    - Joint cell-type and domain inference
    - Handles multi-sample analysis natively
    - Principled uncertainty quantification
- **Limitations:**
    - Computationally expensive for large datasets
    - Requires specifying number of cell types and domains
- **Technology compatibility:** Visium, Slide-seq

### SpaceFlow
- **Paper:** [Nature Communications, 2022](https://doi.org/10.1038/s41467-022-31878-0)
- **Code:** [github.com/hongleir/SpaceFlow](https://github.com/hongleir/SpaceFlow)
- **Key innovation:** Deep graph network that learns pseudo-spatiotemporal embeddings, capturing both spatial domains and gradual spatial transitions.
- **Strengths:**
    - Captures continuous spatial gradients, not just discrete domains
    - Regularization loss encourages spatially smooth embeddings
- **Limitations:**
    - Pseudo-temporal interpretation can be misleading
    - Moderate scalability
- **Technology compatibility:** Visium, Slide-seq

### SEDR
- **Paper:** [Nature Communications, 2022](https://doi.org/10.1038/s41467-022-35564-z)
- **Code:** [github.com/JKleinfeld/SEDR](https://github.com/JKleinfeld/SEDR)
- **Key innovation:** Variational graph autoencoder with deep embedding that disentangles spatial and transcriptomic features.
- **Strengths:**
    - Deep generative model with clear latent space
    - Good performance on Visium benchmarks
- **Limitations:**
    - Slower than non-deep methods
    - Limited validation on imaging-based platforms
- **Technology compatibility:** Visium, Slide-seq

### GASTON
- **Paper:** [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02503-5)
- **Code:** [github.com/raphael-group/GASTON](https://github.com/raphael-group/GASTON)
- **Key innovation:** Identifies spatial domains and spatially-varying gene expression programs simultaneously, modeling expression as piecewise linear functions of spatial coordinates.
- **Strengths:**
    - Interpretable model connecting domains to gene programs
    - Captures within-domain expression gradients
- **Limitations:**
    - Assumes piecewise linear spatial patterns
    - R implementation
- **Technology compatibility:** Visium, Slide-seq

### conST
- **Paper:** [Nature Communications, 2022](https://doi.org/10.1038/s41467-022-34879-1)
- **Code:** [github.com/ys-zong/conST](https://github.com/ys-zong/conST)
- **Key innovation:** Contrastive learning on spatial transcriptomics graphs with self-supervised objectives, avoiding the need for domain labels.
- **Strengths:**
    - Self-supervised contrastive framework
    - Integrates gene expression and morphological features
- **Limitations:**
    - Similar to GraphST but with less community adoption
    - Augmentation strategies may not generalize across tissues
- **Technology compatibility:** Visium

### smoothclust
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.05.23.595164)
- **Code:** [github.com/lmweber/smoothclust](https://github.com/lmweber/smoothclust)
- **Key innovation:** Spatially-aware smoothing of expression data before standard clustering, achieving spatial coherence through preprocessing rather than custom models.
- **Strengths:**
    - Simple approach compatible with existing clustering pipelines
    - No GPU or deep learning required
    - Bioconductor integration
- **Limitations:**
    - Smoothing can blur genuine sharp boundaries
    - Less sophisticated than deep learning approaches
- **Technology compatibility:** Visium, Visium HD, any spatial platform

### Vesalius
- **Paper:** [Nature Communications, 2024](https://doi.org/10.1038/s41467-024-48107-x)
- **Code:** [github.com/WJiangLab/Vesalius](https://github.com/WJiangLab/Vesalius)
- **Key innovation:** Image-processing-inspired approach that converts gene expression to spatial "images" and applies morphological operations for territory detection.
- **Strengths:**
    - Unique image-processing perspective on spatial domains
    - Identifies territory boundaries and gradients
- **Limitations:**
    - Novel approach with limited benchmarking against deep methods
    - R implementation
- **Technology compatibility:** Visium, Slide-seq

## Benchmark Summary

Systematic benchmarks (e.g., from the STbenchmark and SpatialBenchmarking studies) consistently place **GraphST** and **STAGATE** as top performers for spatial domain identification on Visium data. **BANKSY** has emerged as a strong practical choice due to its simplicity, speed, and competitive accuracy. **BayesSpace** performs well on Visium but is strictly limited to grid-based data. GNN-based methods dominate overall, but their advantage diminishes on well-separated domains where simpler methods like BANKSY or smoothclust suffice.

!!! warning "Number of domains matters"
    Most methods require specifying the number of domains (k). Results are sensitive to this choice. Use multiple k values and assess stability. BayesSpace provides some guidance through its Bayesian framework, but no method reliably auto-selects k.

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| Visium, quick analysis | Fast domain detection | BANKSY | Simple, fast, competitive accuracy |
| Visium, best accuracy | Benchmark-winning domains | GraphST or STAGATE | Top performers in systematic benchmarks |
| Visium, statistical rigor | Bayesian domain inference | BayesSpace | Uncertainty quantification, sub-spot resolution |
| Large dataset (>100K cells) | Scalable domains | BANKSY | Scales to millions without GPU |
| Multi-sample Visium | Cross-sample domain alignment | GraphST or BASS | Built-in batch integration |
| Imaging-based (MERFISH) | Cell-level domains | BANKSY or STAGATE | Validated on irregular cell coordinates |
| Continuous spatial gradients | Detect gradients, not just domains | SpaceFlow or GASTON | Model continuous spatial variation |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| BANKSY | Yes | Yes | Yes | Yes | Yes | - | Yes |
| SpaGCN | Yes | - | - | - | - | - | - |
| STAGATE | Yes | - | - | Yes | - | - | Yes |
| BayesSpace | Yes | - | - | - | - | - | - |
| GraphST | Yes | - | - | Yes | - | - | Yes |
| BASS | Yes | - | - | - | - | - | - |
| SpaceFlow | Yes | - | - | - | - | - | - |
| SEDR | Yes | - | - | - | - | - | - |
| GASTON | Yes | - | - | - | - | - | - |
| conST | Yes | - | - | - | - | - | - |
| smoothclust | Yes | Yes | - | - | - | - | - |
| Vesalius | Yes | - | - | - | - | - | - |
