# Museum of Spatial Transcriptomics

**Verdict:** The definitive taxonomy of spatial transcriptomics methods -- start here before touching any tool.

**Citation:** Moses L, Pachter L. "Museum of Spatial Transcriptomics." *Nature Methods* 19, 534--546 (2022). [DOI: 10.1038/s41592-022-01409-2](https://doi.org/10.1038/s41592-022-01409-2)

## Problem Setup

By 2022, spatial transcriptomics had exploded into dozens of competing technologies with overlapping names, confusing acronyms, and no organizing framework. A researcher choosing between MERFISH, Slide-seq, Visium, and STARmap had no systematic way to compare their fundamental measurement principles. Moses and Pachter set out to create that organizing framework -- a taxonomy that classifies every spatial transcriptomics method by *how* it measures gene expression in space, not *when* it was published.

## Method

The paper organizes the entire field into two major branches based on measurement approach: **microdissection-based** methods (which physically isolate regions before sequencing) and **in situ** methods (which measure transcripts where they sit). Within in situ methods, a further split distinguishes **sequencing-based** approaches (like Slide-seq, which capture mRNA on barcoded beads) from **imaging-based** approaches (like MERFISH, which visualize individual transcripts via fluorescent probes).

Each method is evaluated along several axes: spatial resolution (subcellular to tissue-level), gene throughput (targeted panels vs. whole transcriptome), sensitivity (detection efficiency per transcript), and scalability (tissue area that can be profiled). The paper traces the historical development of each branch, connecting modern tools to their intellectual ancestors -- for example, linking Visium back to the original Spatial Transcriptomics method from 2016.

The taxonomy is supported by detailed figures showing the resolution-throughput tradeoff that defines the field: imaging-based methods achieve subcellular resolution but are limited to hundreds or thousands of genes, while sequencing-based methods capture the whole transcriptome but at coarser spatial resolution.

## Evaluation

This is a review paper, not a methods paper, so there are no benchmarks or quantitative evaluations. Its contribution is conceptual: providing the vocabulary and classification system the field now uses. The paper covers over 30 distinct technologies across both branches, with consistent comparison criteria applied to each. The historical timeline figures have become standard references in introductions to spatial omics papers.

## Honest Assessment

**Strengths:**

- Provides the definitive classification system that the entire field has adopted -- the microdissection/in situ and sequencing/imaging distinctions are now standard vocabulary.
- Historical depth is exceptional, tracing methods back to their roots in ISH (in situ hybridization) from the 1960s.
- The resolution-throughput tradeoff figures remain the clearest visual summary of the technology landscape.
- Technology-agnostic framing allows fair comparison without vendor bias.

**Limitations:**

- Already outdated for 2024--2025 technologies: Visium HD, Xenium, MERSCOPE, and CosMx have shifted the resolution-throughput frontier significantly since publication.
- Does not cover spatial proteomics (CODEX, MIBI) or multimodal spatial methods, limiting its scope to transcriptomics only.
- Minimal discussion of computational methods -- the analysis side of spatial omics receives only brief treatment.
- The taxonomy can feel overly detailed for practitioners who just need to pick a platform.

**Design Decision:** The key bet is that organizing by *measurement principle* rather than by application or chronology creates a more durable framework. This has proven correct -- even as new technologies appear, they slot into the existing branches, making the taxonomy a living reference rather than a static snapshot.
