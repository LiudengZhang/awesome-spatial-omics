# The Pipeline Problem

Spatial omics has more than 200 published analysis tools. No standard pipeline exists for assembling them. Every lab that adopts a spatial technology must build its own analysis workflow from scratch, navigating a fragmented landscape of incompatible tools, undocumented assumptions, and technology-specific quirks. This is the pipeline problem.

## The scale of fragmentation

Consider the analysis steps required for a typical Visium experiment:

1. Raw data processing (Space Ranger or open-source alternative)
2. Quality control and filtering
3. Normalization
4. Dimensionality reduction
5. Spatial domain detection
6. Deconvolution (if spot-level)
7. Differential expression
8. Spatially variable gene detection
9. Cell-cell communication inference
10. Visualization

For each step, there are 5--30 published tools. Most were developed independently, with different input/output formats, different programming languages (R vs. Python), different spatial data representations, and different assumptions about upstream preprocessing. The total number of possible pipeline configurations is combinatorial.

Now consider that this pipeline is specific to Visium. MERFISH requires a different starting point (segmentation instead of deconvolution). Stereo-seq requires binning decisions. CODEX requires protein-specific normalization. Each technology demands a partially different pipeline.

## Why no standard pipeline has emerged

### Technologies differ fundamentally

Unlike scRNA-seq, where 10x Chromium dominates and a Scanpy/Seurat workflow covers most use cases, spatial omics spans fundamentally different measurement modalities:

- **Sequencing-based** (Visium, Slide-seq, Stereo-seq): genome-wide coverage, spot-level resolution, count matrices as output
- **Imaging-based** (MERFISH, Xenium, CosMx, seqFISH): targeted gene panels, single-cell/subcellular resolution, transcript coordinates as output
- **Protein-based** (CODEX, MIBI-TOF, IMC): protein panels, single-cell resolution, pixel intensities as output

A tool designed for Visium count matrices cannot directly process MERFISH transcript coordinates. A segmentation method critical for MERFISH is irrelevant for Visium. This heterogeneity prevents any single pipeline from being universal.

### The Scanpy/Seurat analogy breaks down

In scRNA-seq, Scanpy and Seurat each provide an end-to-end pipeline: loading, QC, normalization, clustering, differential expression, visualization. They became standards because one framework could handle the entire workflow for the dominant technology.

Spatial omics frameworks (Squidpy, Giotto, Seurat v5 spatial) provide partial coverage but cannot replicate this success because:

- No single framework implements the best method for each step
- Specialized tools (Cell2location, Baysor, nnSVG) are standalone packages, not framework plugins
- Framework-native methods (e.g., Squidpy's spatial autocorrelation) are adequate but rarely best-in-class
- Data structures differ: AnnData vs. SpatialExperiment vs. Giotto objects

### Best-in-class tools do not interoperate

The best deconvolution method (Cell2location) outputs proportions in its own format. The best clustering method (GraphST) expects a specific AnnData structure. The best SVG detection tool (nnSVG) is R-based while most upstream tools are Python-based. Connecting these tools requires custom glue code for every pair, and this glue code is rarely published.

## The consequences

### Reproducibility suffers

When every lab builds a custom pipeline, minor differences in preprocessing, parameter choices, and tool versions produce different results from the same data. Published analyses are difficult to reproduce because the pipeline is described in methods sections as a sequence of tool names without the critical details: which parameters, which normalization, which gene filtering cutoffs.

### New users face a steep learning curve

A graduate student starting a spatial transcriptomics project must simultaneously learn the technology, the biology, and the analysis tools. Without a standard pipeline to follow, they must evaluate dozens of tools, read benchmark papers, and make choices that require expertise they do not yet have.

### Methods papers optimize for benchmarks, not pipelines

Most spatial omics methods papers demonstrate their tool on one analysis step in isolation. They compare against other tools on benchmark datasets but do not show how the tool integrates into a complete analysis workflow. A method that wins a benchmark may be impractical to use because of installation difficulties, format incompatibilities, or undocumented preprocessing requirements.

## What would help

### Decision frameworks, not tool lists

The field needs structured guidance for choosing tools based on technology, data characteristics, and biological question. A decision tree is more useful than a ranked list. This is what the [Pipeline Decision Tree](../pipeline/decision-tree.md) on this site attempts to provide.

### Interoperability standards

The scverse ecosystem (AnnData, MuData, SpatialData) represents the most promising effort toward interoperability. By standardizing the data container, tools that read and write AnnData or SpatialData can be composed without custom glue code. But adoption is incomplete: many tools still use custom formats or require conversion steps.

### Tested pipeline recipes

Rather than building from scratch, labs need tested combinations: "for Visium data with H&E imaging, use this specific sequence of tools with these parameters." Pipeline repositories like nf-core/spatialvi and published Snakemake/Nextflow workflows begin to address this, but coverage is limited and maintenance is inconsistent.

### Honest documentation of limitations

Every tool has limitations that its documentation underplays. A deconvolution method that works beautifully on mouse brain may fail on tumor tissue. A clustering method benchmarked on DLPFC may produce meaningless results on lung. The field needs more honest reporting of where tools break, not just where they succeed.

## The state of frameworks

| Framework | Language | Strengths | Gaps |
|---|---|---|---|
| Squidpy | Python | Spatial statistics, graph analysis, image features | No deconvolution, limited DL methods |
| Giotto | R | Comprehensive, many analysis modules | Complex installation, less active development |
| Seurat v5 | R | Large user base, integrated spatial | R-only, limited spatial-specific methods |
| SpatialData | Python | Data interoperability, multi-technology | Analysis tools still maturing |
| Scanpy + extensions | Python | Mature ecosystem, extensible | Spatial support is add-on, not native |

No framework currently provides a complete, best-in-class pipeline for any spatial technology. Each covers some steps well and delegates others to external tools, leaving the integration burden on users.

## Further reading

- [Pipeline Decision Tree](../pipeline/decision-tree.md) for practical pipeline guidance
- [The Technology-Analysis Gap](technology-analysis-gap.md) for how technology advances outpace tool development
- [Commercial Consolidation](commercial-consolidation.md) for how vendor changes affect the pipeline landscape
- [Benchmark Synthesis](../benchmarks/benchmark-synthesis.md) for which tools perform best at each step
