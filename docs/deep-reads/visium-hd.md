# Visium HD

**Verdict:** Near single-cell resolution at whole-transcriptome scale, but the analysis ecosystem has not caught up yet.

**Citation:** 10x Genomics. "Visium HD Spatial Gene Expression." Product launch and technical documentation (2023--2024). Oliveira MF et al., "Characterization of the tumor immune microenvironment in human tissue using Visium HD." *bioRxiv* (2023). [DOI: 10.1101/2023.12.07.570603](https://doi.org/10.1101/2023.12.07.570603)

## Problem Setup

The original Visium platform captured whole-transcriptome data at 55-micron spot resolution -- roughly 1--10 cells per spot. This created a fundamental ambiguity: gene expression measured at each spot was a mixture of multiple cell types, requiring computational deconvolution (see [Cell2location](cell2location.md)) to estimate cell-type composition. Visium HD aims to close this gap by shrinking the capture resolution to 2-micron bins, approaching the scale of individual cells and potentially eliminating the need for deconvolution entirely.

## Method

Visium HD uses a continuous lawn of barcoded oligonucleotides printed at 2um resolution on the capture slide, replacing the discrete 55um spots of standard Visium. Tissue sections are placed on this lawn, mRNA is captured, and spatial barcodes encode location. The raw data consists of 2um bins that can be computationally aggregated into larger bins (8um, 16um) depending on the analysis need.

The key architectural difference from standard Visium is that there are no gaps between capture areas -- the entire tissue section is profiled. This also means the data volume increases dramatically: a single Visium HD experiment can generate tens of millions of bins, each with sparse gene counts. The recommended workflow aggregates 2um bins into 8um bins for most analyses, balancing resolution against sparsity.

The technology remains sequencing-based and whole-transcriptome, preserving the main advantage of the Visium platform over imaging-based methods like MERFISH or Xenium, which are limited to targeted gene panels.

## Evaluation

Early publications demonstrate that 8um bins can resolve individual cells in tissues with well-separated cells (e.g., mouse brain), though dense tissues (e.g., lymph nodes) still show mixing at this resolution. The whole-transcriptome coverage detects 10,000+ genes per tissue section. Comparison with matched Xenium data on the same tissue shows concordant spatial patterns, with Visium HD providing broader gene coverage and Xenium providing cleaner single-cell resolution.

The main evaluation gap is computational: most existing spatial analysis tools ([Squidpy](squidpy.md), [GraphST](graphst.md), [nnSVG](nnsvg.md)) were designed for datasets with thousands of spots, not millions of bins. Running standard workflows on Visium HD data requires either aggressive aggregation or new scalable implementations.

## Honest Assessment

**Strengths:**

- The largest resolution improvement in the Visium platform's history -- 2um bins bring sequencing-based spatial transcriptomics into near single-cell territory.
- Whole-transcriptome coverage maintained, unlike imaging-based competitors that require gene panel selection.
- Continuous capture area eliminates the gaps between spots, enabling analysis of any tissue region.
- Compatible with existing Visium sample preparation workflows, lowering the adoption barrier.

**Limitations:**

- At 2um resolution, individual bins are extremely sparse -- most bins contain counts for only a handful of genes, making aggregation to 8um practically mandatory.
- The analysis tool ecosystem has not caught up: standard pipelines designed for ~5,000 spots struggle with millions of bins in terms of memory and runtime.
- Whether 8um aggregated bins truly achieve single-cell resolution depends heavily on tissue type and cell density.
- Cost per experiment remains high, and data storage requirements are substantial.

**Design Decision:** The bet is that pushing sequencing-based resolution to near single-cell scale, even with sparse per-bin counts, is more valuable than the cleaner single-cell data from imaging-based platforms -- because whole-transcriptome coverage enables discovery of unexpected genes. Whether this bet pays off depends entirely on whether analysis tools can handle the data scale, making infrastructure tools like [SpatialData](spatialdata.md) critical.
