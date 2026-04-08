# Awesome Spatial Omics

Spatial omics has more tools than any analyst can evaluate. This site organizes them by the question being asked and the technology being used.

## The landscape at a glance

| Category | Count |
|----------|-------|
| Tools and methods cataloged | 240+ |
| Spatial technologies covered | 30+ |
| Analysis step categories | 13 |
| Benchmark comparisons synthesized | 4 |
| Deep-read paper analyses | 10+ |

Spatial transcriptomics, proteomics, and metabolomics generate data that is fundamentally different from dissociated single-cell experiments: every measurement comes with coordinates. This creates an entire class of analytical problems -- segmentation, niche detection, spatially variable gene testing, 3D alignment -- that standard scRNA-seq pipelines were never designed for. The field now has hundreds of tools, and choosing the right one depends on the technology, the biological question, and the data scale.

!!! tip "This is not just another awesome list"
    Most awesome lists are flat catalogs. This companion site adds what a flat list cannot: opinionated pipeline recommendations, technology-specific guidance, benchmark synthesis, and deep reads on key papers. The [GitHub README](https://github.com/LiudengZhang/awesome-spatial-omics) remains the canonical tool catalog; this site layers interpretation on top.

## How to use this site

**Starting a new spatial project?** Begin with the [Analysis Pipeline Decision Tree](pipeline/decision-tree.md). It maps technologies to recommended tool chains and maps biological questions to analysis categories.

**Choosing a technology?** The [Technologies Overview](technologies/overview.md) compares resolution, throughput, and commercial availability across all major platforms, with detailed pages for [sequencing-based](technologies/sequencing-based.md), [imaging-based](technologies/imaging-based.md), [spatial proteomics](technologies/spatial-proteomics.md), [spatial multi-omics](technologies/spatial-multiomics.md), and [spatial metabolomics](technologies/spatial-metabolomics.md) approaches.

**Evaluating a specific method?** The [Methods](methods/preprocessing-qc.md) section covers 13 analysis categories from pre-processing to 3D reconstruction, and the [Benchmarks](benchmarks/benchmark-synthesis.md) section synthesizes head-to-head comparisons.

**Going deeper on a key tool?** The [Deep Reads](deep-reads/index.md) section provides multi-page analyses of influential papers and tools, covering architecture, assumptions, limitations, and practical gotchas.

**Understanding field trends?** The [Synthesis](synthesis/pipeline-problem.md) section offers editorial analysis of structural challenges: the pipeline fragmentation problem, the technology-analysis gap, and commercial consolidation.

## Quick links

- [GitHub README and full tool catalog](https://github.com/LiudengZhang/awesome-spatial-omics)
- [Pipeline Decision Tree](pipeline/decision-tree.md)
- [Technology Overview](technologies/overview.md)
- [Benchmark Synthesis](benchmarks/benchmark-synthesis.md)
- [Deep Reads](deep-reads/index.md)
- [Glossary](glossary.md)
