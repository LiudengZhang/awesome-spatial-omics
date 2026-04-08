# The Technology-Analysis Gap

Spatial omics technologies are advancing faster than the analysis tools designed to process their data. Each generation of instruments produces higher resolution, more genes, and larger tissue areas -- but the computational methods, pipelines, and frameworks needed to fully exploit this data lag behind by one to two technology generations.

## Where the gap is widest

### Visium HD: 2-micron bins, 55-micron tools

10x Genomics released Visium HD in 2023, reducing the capture resolution from 55-micron spots to 2-micron bins -- a roughly 750-fold increase in spatial granularity. This was a transformative hardware advance. The analysis side was not ready.

Most spatial analysis tools were designed for the original Visium geometry: ~3,500 spots per tissue section, each containing 5--20 cells. Visium HD produces millions of bins per section, most containing partial cells or even single transcripts. The immediate consequences:

- **Clustering tools** (BayesSpace, SpaGCN) that model spot-level spatial graphs become computationally intractable at millions of nodes
- **Deconvolution** becomes conceptually unnecessary at single-cell resolution, but the bins are too small for direct cell typing -- an awkward intermediate scale
- **Memory requirements** exceed what most academic compute environments provide for standard AnnData workflows
- **Binning decisions** (aggregate 2-micron bins to 8-micron? 16-micron?) become a critical preprocessing choice with no established best practice

The practical result: most Visium HD analyses in 2024--2025 aggregate bins to pseudo-spots resembling the original Visium resolution, sacrificing the hardware's key advance to use existing tools.

### Xenium: subcellular resolution, cell-level pipelines

The Xenium platform provides subcellular transcript localization -- individual RNA molecules with nanometer-precision coordinates. This enables analyses that were previously impossible: intracellular transcript distribution, nuclear vs. cytoplasmic expression ratios, RNA localization patterns.

Current analysis pipelines largely ignore this subcellular information:

- Standard workflows aggregate transcripts to cell-level counts after segmentation, discarding spatial information within cells
- Tools for subcellular analysis (e.g., transcript polarity, subcellular domain detection) are experimental and not integrated into major frameworks
- The subcellular resolution is most valuable for studying RNA localization biology, but the spatial omics community is primarily focused on tissue-level questions

### Multi-modal spatial: measured together, analyzed separately

Technologies like DBiT-seq (RNA + protein), SPOTS (RNA + protein on Visium), and spatial ATAC+RNA measure multiple modalities from the same tissue section. Integration tools have not kept pace:

- Most multi-modal integration methods (MOFA+, Seurat WNN, MultiVI) were designed for dissociated single-cell data and do not model spatial relationships
- Spatial-specific multi-modal methods are rare: few tools jointly model gene expression and protein abundance with spatial context
- Spatial metabolomics (MALDI, DESI) produces data with fundamentally different characteristics (continuous spectra, lower spatial resolution) that no transcriptomics-oriented tool handles natively

### Stereo-seq: organism-scale, organ-scale tools

Stereo-seq achieves nanometer resolution across centimeter-scale tissue sections, producing datasets with billions of transcript coordinates per experiment. The Chen et al. (2022) mouse embryo atlas demonstrated the technology's power, but:

- No standard analysis framework handles billion-scale spatial data efficiently
- Visualization tools designed for thousands of cells cannot render millions
- Spatial statistics (autocorrelation, variograms) become computationally prohibitive at this scale
- The STOmics ecosystem provides tools, but they are not widely adopted outside BGI-affiliated groups

## The segmentation bottleneck

!!! warning "Segmentation quality limits everything downstream"
    For every imaging-based spatial technology, the accuracy of cell segmentation determines the ceiling for all subsequent analyses. A perfect clustering algorithm cannot recover from segmentation that merges two cells or splits one cell into fragments.

Segmentation is the single largest bottleneck in the technology-analysis gap:

- **Technologies produce more transcripts per cell**, but segmentation accuracy has not improved proportionally. More transcripts per cell means more transcripts near cell boundaries, where assignment is ambiguous.
- **Tissue diversity exceeds training data.** Cellpose and Mesmer are trained on specific tissue types. Performance degrades on tissues with unusual morphologies (e.g., elongated neurons, syncytia, adipocytes with large vacuoles).
- **3D segmentation** is largely unsolved. Technologies like Expansion Sequencing and sequential sectioning produce 3D data, but segmentation tools operate on 2D slices.
- **No universal quality metric** exists for segmentation. Users must visually inspect results, which does not scale to whole-organ datasets.

The gap between segmentation capability and segmentation need explains why transcript-based methods (Baysor) and segmentation-free approaches are gaining interest: they sidestep the bottleneck rather than solving it.

## Implications for the field

### Tool developers chase the previous generation

Most analysis tools are developed by academic groups who validate on available benchmark datasets. These datasets come from the previous generation of technology: a tool published in 2024 was likely developed on Visium data from 2021--2022. By the time a tool matures and gains users, the data it was designed for may already be superseded.

This creates a persistent lag: the most validated tools are optimized for technology that is one to two generations behind the current state of the art.

### Resolution outpaces biological questions

Higher resolution is not always biologically informative. Many biological questions -- tissue architecture, cell-type composition, spatial gene programs -- operate at the 50--100 micron scale. Subcellular resolution is powerful for RNA biology but unnecessary for most spatial domain detection or deconvolution tasks. The technology-analysis gap is partly a solution looking for a problem: the hardware has advanced beyond what current biological questions demand.

!!! note "When high resolution matters"
    Subcellular resolution becomes essential for studying RNA localization, nuclear-cytoplasmic partitioning, transcript polarity in polarized cells, and spatially resolved isoform usage. These are important biological questions that were previously inaccessible, but they require entirely new analytical frameworks rather than adaptations of existing tools.

### The framework gap

The data container and framework layer -- AnnData, SpatialData, Giotto -- must adapt to each new technology generation. SpatialData represents the most forward-looking effort, with its technology-agnostic data model supporting points, shapes, images, and tables. But SpatialData is still maturing, and most analysis tools have not been refactored to use it natively.

## What would close the gap

**Scalable-by-design tools.** New methods should be designed for million-cell datasets from the outset, not retrofitted after publication. GPU-accelerated implementations, out-of-core processing, and efficient spatial indexing should be baseline requirements.

**Technology-agnostic abstractions.** Tools that operate on abstract spatial data representations (point clouds, spatial graphs, raster images) rather than technology-specific formats will adapt more easily to new platforms.

**Subcellular analysis frameworks.** The field needs dedicated tools for subcellular spatial analysis -- not just cell-level aggregation with higher-resolution input. This includes transcript localization, intracellular domain detection, and subcellular spatial statistics.

**Continuous benchmarking.** Benchmarks should be updated as new technologies emerge, not published once on Visium data and cited indefinitely. Community-maintained benchmark platforms (similar to Open Problems in Single-Cell Analysis) could help.

## Further reading

- [The Pipeline Problem](pipeline-problem.md) for how tool fragmentation compounds the technology gap
- [Commercial Consolidation](commercial-consolidation.md) for how vendor changes affect technology availability
- [Technologies Overview](../technologies/overview.md) for current technology capabilities
- [Segmentation Benchmarks](../benchmarks/segmentation-benchmarks.md) for the state of cell segmentation
