# What Benchmarks Tell Us

Spatial omics benchmarks have matured significantly since 2022, with multiple independent studies now comparing tools for deconvolution, spatial domain detection, cell segmentation, and spatially variable gene identification. This page synthesizes the high-level findings across all benchmark categories.

## Winners by category

| Analysis task | Top performer(s) | Runner-up(s) | Key trade-off |
|---|---|---|---|
| Spatial domain detection | GraphST, STAGATE | BayesSpace (Visium only) | GNN methods need GPU; BayesSpace is CPU-friendly |
| Deconvolution | Cell2location, RCTD | Tangram (mapping), SPOTlight | Cell2location is slow but accurate; RCTD is fast |
| Image-based segmentation | Cellpose2 | Mesmer/DeepCell | Cellpose2 excels on DAPI; Mesmer better for multiplexed |
| Transcript-based segmentation | Baysor | pciSeq | Baysor + Cellpose prior gives best hybrid results |
| SVG detection (accuracy) | nnSVG | SpatialDE2, SPARK | nnSVG balances accuracy and scalability |
| SVG detection (speed) | SPARK-X | Moran's I | SPARK-X handles >50k spots efficiently |
| Cell-cell communication | No single winner | --- | Different methods answer fundamentally different questions |

## Cross-cutting themes

### GNN-based methods dominate spatial domain detection

Graph neural network approaches -- particularly GraphST and STAGATE -- consistently outperform classical methods across multiple benchmarks (Yuan et al. 2024, Dong et al. 2023). These methods naturally incorporate spatial graphs, making them well-suited for data where neighborhood relationships carry biological meaning. BayesSpace remains competitive on Visium data specifically, but its performance drops on imaging-based platforms where spot geometries differ.

See [Clustering Benchmarks](clustering-benchmarks.md) for detailed results.

### Deconvolution has clear frontrunners

Cell2location and RCTD emerge as the most reliable deconvolution methods across three independent benchmarks (Li et al. 2022, Nature Comm 2023, Briefings Bioinf 2023). Cell2location provides the best overall accuracy but requires GPU computation and careful reference preparation. RCTD offers the best speed-accuracy trade-off for large-scale studies. Tangram excels specifically at mapping single cells to spatial locations rather than estimating proportions.

See [Deconvolution Benchmarks](deconvolution-benchmarks.md) for detailed results.

### Segmentation quality determines downstream results

For imaging-based spatial technologies, segmentation is the single most impactful preprocessing step. Petukhov et al. (2023) demonstrated that using Baysor with a Cellpose prior outperforms either method alone, combining the strengths of image-based boundary detection with transcript-density modeling. Poor segmentation propagates errors to every downstream analysis.

See [Segmentation Benchmarks](segmentation-benchmarks.md) for detailed results.

### SVG detection: accuracy vs. speed is the real choice

nnSVG provides the best accuracy-scalability balance for spatially variable gene detection, but SPARK-X is dramatically faster on datasets exceeding 50,000 spots. Moran's I -- despite being a simple spatial autocorrelation statistic -- remains surprisingly competitive, raising the question of whether complex models are always necessary.

See [SVG Benchmarks](svg-benchmarks.md) for detailed results.

### Cell-cell communication resists simple ranking

Unlike other categories, CCC benchmarks have not produced a clear winner because different methods ask fundamentally different questions. Ligand-receptor methods (CellChat, CellPhoneDB) quantify potential signaling, while spatial proximity methods (MISTy, COMMOT) model actual spatial relationships. Optimal-transport approaches (COMMOT) capture different aspects than graph-based approaches (SpatialDM). The "best" method depends on whether the goal is hypothesis generation, mechanism prediction, or spatial pattern discovery.

## The DLPFC problem

!!! warning "Benchmark monoculture"
    The dorsolateral prefrontal cortex (DLPFC) dataset from Maynard et al. (2021) has become the de facto standard for benchmarking spatial domain detection tools. Nearly every clustering benchmark uses it. This creates a dangerous circularity: methods may be optimized for layered cortical architectures, which have clear laminar structure, while performing poorly on tissues with irregular or overlapping domains (e.g., tumors, lymph nodes, developing organs).

The DLPFC dataset has well-annotated cortical layers (L1--L6 plus white matter), making it an appealing ground truth. But cortical layers represent one of the easiest spatial domain structures: they are roughly parallel, non-overlapping, and well-separated in expression space. Real biological variation -- tumor microenvironments with intermixed cell states, developmental niches with continuous gradients -- is far more challenging.

Benchmarks that report only DLPFC performance should be interpreted cautiously. Studies that additionally test on simulated data with known ground truth (e.g., STdeconvolve simulations) or on tissues with expert pathologist annotations (e.g., breast cancer, embryo) provide more generalizable evidence.

## What is missing from current benchmarks

**Multi-technology comparisons.** Most benchmarks test methods on data from a single technology (typically Visium). Few systematically compare how the same method performs across Visium, MERFISH, Slide-seq, and Stereo-seq. Technology-specific biases in noise structure, resolution, and gene coverage mean that benchmark rankings may not transfer.

**Niche detection benchmarks.** Spatial niche identification -- defining local cellular neighborhoods and their composition -- lacks systematic benchmarking. Tools like Banksy, CellCharter, and GraphCompass address this problem, but no comprehensive head-to-head comparison exists.

**Clinical validation.** Benchmarks use computational ground truth (simulations, expert annotations, scRNA-seq-derived pseudo-spots) but rarely validate against clinical outcomes. A deconvolution method that predicts cell-type proportions well in simulation may not improve patient stratification in practice.

**Scalability at atlas scale.** As whole-organ and whole-organism spatial atlases become common (Allen Brain Cell Atlas, Stereo-seq mouse embryo), benchmarks need to test at scales of millions of cells. Many current benchmarks use datasets with 3,000--50,000 spots.

**Segmentation-free approaches.** Most benchmarks assume segmentation as a given, but segmentation-free methods (SpatialData bins, pixel-level analysis) may bypass the segmentation bottleneck entirely. These approaches are under-benchmarked.

## Further reading

- [Clustering Benchmarks](clustering-benchmarks.md)
- [Deconvolution Benchmarks](deconvolution-benchmarks.md)
- [Segmentation Benchmarks](segmentation-benchmarks.md)
- [SVG Benchmarks](svg-benchmarks.md)
- [Datasets](../datasets/datasets.md) for benchmark dataset details
- [The Pipeline Problem](../synthesis/pipeline-problem.md) for why assembling these tools remains difficult
