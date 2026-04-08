# Squidpy

**Verdict:** The Swiss Army knife for spatial omics analysis -- comprehensive, well-maintained, and the community default.

**Citation:** Palla G, Spitzer H, Klein M, et al. "Squidpy: a scalable framework for spatial omics analysis." *Nature Methods* 19, 171--178 (2022). [DOI: 10.1038/s41592-021-01358-2](https://doi.org/10.1038/s41592-021-01358-2)

## Problem Setup

By 2021, spatial omics analysis required stitching together disparate tools: one package for building spatial graphs, another for neighborhood enrichment, another for spatial statistics, and another for image feature extraction. Each tool had its own data format, API conventions, and dependencies. The field needed a unified framework that covered the most common spatial analysis tasks in a single package, built on the scverse ecosystem so it could interoperate with scanpy, AnnData, and the broader Python single-cell analysis stack.

## Method

Squidpy provides a modular analysis framework organized around two core modules: **graph** (for spatial neighborhood analysis) and **image** (for tissue image processing). The graph module constructs spatial neighbor graphs from coordinates, then provides functions for neighborhood enrichment analysis, spatial autocorrelation (Moran's I, Geary's C), co-occurrence analysis, centrality scores, and interaction matrices. The image module extracts features from H&E or fluorescence images using pretrained CNNs, segmentation, and custom feature extractors.

The data model centers on AnnData: spatial coordinates are stored in `obsm`, spatial graphs in `obsp`, and analysis results in `obs` or `uns`. This design means every Squidpy output is immediately available to scanpy for downstream analysis (differential expression, clustering, visualization). Image data is handled through an `ImageContainer` class that supports lazy loading and tiling for large images.

Key analysis capabilities include: neighborhood enrichment testing (are two cell types co-located more than expected by chance?), ligand-receptor interaction scoring using databases from OmniPath, spatially variable gene detection via Moran's I, and spatial autocorrelation analysis. The framework also provides plotting functions that overlay analysis results on tissue images.

Squidpy 2.0 integrates with [SpatialData](spatialdata.md), adopting the SpatialData object as an alternative input format alongside AnnData, enabling analysis of multimodal spatial data including images, segmentation masks, and transcript coordinates in a unified framework.

## Evaluation

Squidpy was demonstrated on Visium, Slide-seq, MERFISH, and imaging mass cytometry datasets, covering both sequencing-based and imaging-based platforms. The paper shows end-to-end workflows for each platform, from spatial graph construction through neighborhood analysis to visualization. Computational benchmarks show that Squidpy scales to datasets with 50,000+ cells for graph-based analyses, with image processing scaling limited by available memory.

Community adoption has been the strongest validation: Squidpy is the most widely used spatial analysis framework in the Python ecosystem, with hundreds of citations and active development by the scverse team.

## Honest Assessment

**Strengths:**

- Comprehensive coverage of common spatial analysis tasks in a single, well-documented package, eliminating the need to stitch together multiple tools.
- Native AnnData integration means results flow seamlessly into the scanpy ecosystem for downstream analysis, reducing format conversion overhead.
- Active maintenance by the scverse team with regular releases, responsive issue tracking, and community engagement.
- SpatialData integration in version 2.0 future-proofs the framework for multimodal spatial data from next-generation platforms.

**Limitations:**

- Was not originally designed for subcellular-resolution imaging data (MERFISH, Xenium) -- the AnnData-centric model works best when the unit of analysis is a cell or spot, not a transcript.
- Built-in spatial statistics (Moran's I, neighborhood enrichment) are useful but relatively simple -- dedicated methods like [nnSVG](nnsvg.md) for SVG detection or [COMMOT](commot.md) for CCC provide more sophisticated analysis.
- Image feature extraction using pretrained CNNs is convenient but limited compared to specialized segmentation tools like [Cellpose](cellpose.md).
- Performance on very large datasets (>100,000 cells) requires careful memory management and may need chunked processing.

**Design Decision:** The bet is that a *comprehensive but modular* framework covering 80% of common tasks is more valuable than specialized tools that each do one thing perfectly. This is the same bet scanpy made for scRNA-seq, and it has proven correct: most analysts start with Squidpy and only reach for specialized tools when they need deeper analysis. The framework's real power is reducing time-to-first-result, not pushing the state of the art in any single method.
