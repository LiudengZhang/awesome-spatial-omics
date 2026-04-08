# Subcellular Spatial Analysis

**Pipeline question:** Where within individual cells are transcripts localized, and what does subcellular organization reveal about gene regulation and cell function?

## Overview

Subcellular spatial analysis examines the distribution of transcripts within cells — nuclear vs. cytoplasmic localization, polarized mRNA distributions, and transcript clustering near organelles or the cell membrane. This emerging field requires imaging-based spatial transcriptomics data with subcellular resolution (e.g., MERFISH, Xenium, seqFISH) and represents a frontier where spatial omics provides information inaccessible to any dissociation-based approach.

## Key Methods

### Bento
- **Paper:** [Nature Communications, 2024](https://doi.org/10.1038/s41467-024-47853-0)
- **Code:** [github.com/ckmah/bento-tools](https://github.com/ckmah/bento-tools)
- **Key innovation:** Comprehensive framework for subcellular transcript analysis — quantifies spatial patterns within cells, tests for non-random localization, and identifies genes with polarized or compartmentalized distributions.
- **Strengths:**
    - Full subcellular analysis toolkit (pattern detection, statistical testing, visualization)
    - Identifies genes with nuclear, cytoplasmic, or polarized localization
    - Well-documented Python package with scverse integration
- **Limitations:**
    - Requires high-quality cell and nuclear segmentation
    - Computationally intensive for very large datasets
- **Technology compatibility:** MERFISH, Xenium, seqFISH, any subcellular-resolution platform

### SPRAWL
- **Paper:** [Genome Biology, 2023](https://doi.org/10.1186/s13059-023-02876-y)
- **Code:** [github.com/amonell/SPRAWL](https://github.com/amonell/SPRAWL)
- **Key innovation:** Quantifies subcellular RNA localization patterns and identifies genes with significantly non-random spatial distributions within cells.
- **Strengths:**
    - Statistical framework for testing subcellular localization significance
    - Simple and interpretable output metrics
- **Limitations:**
    - Requires nuclear and cell boundary annotations
    - Less comprehensive toolkit than Bento
- **Technology compatibility:** MERFISH, Xenium, seqFISH

### FISHFactor
- **Paper:** [Bioinformatics, 2023](https://doi.org/10.1093/bioinformatics/btad183)
- **Code:** [github.com/bioFAM/FISHFactor](https://github.com/bioFAM/FISHFactor)
- **Key innovation:** Non-negative spatial factorization model that decomposes subcellular transcript patterns into interpretable spatial gene expression programs.
- **Strengths:**
    - Discovers co-localized gene programs within cells
    - Non-negative factorization is biologically interpretable
    - Identifies subcellular spatial gene modules
- **Limitations:**
    - Requires dense subcellular transcript data
    - Factor interpretation requires biological expertise
- **Technology compatibility:** MERFISH, seqFISH, osmFISH

### InSTAnT
- **Paper:** [Nature Communications, 2024](https://doi.org/10.1038/s41467-024-51188-7)
- **Code:** [github.com/LieberInstitute/InSTAnT](https://github.com/LieberInstitute/InSTAnT)
- **Key innovation:** Tests for significant transcript co-localization within cells, identifying gene pairs that are physically proximate at subcellular scales.
- **Strengths:**
    - Identifies physically interacting or co-regulated transcript pairs
    - Statistical framework with proper null models
- **Limitations:**
    - Pairwise testing scales quadratically with gene panel size
    - Physical co-localization does not necessarily imply functional interaction
- **Technology compatibility:** MERFISH, Xenium

### troutpy
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.12.17.629046)
- **Code:** [github.com/LucasBrunworthy/troutpy](https://github.com/LucasBrunworthy/troutpy)
- **Key innovation:** Python toolkit for subcellular transcript analysis with a focus on nuclear/cytoplasmic ratio quantification and transcript trafficking patterns.
- **Strengths:**
    - Focused on nuclear-cytoplasmic partitioning — a well-studied biological process
    - Lightweight and easy to integrate into existing pipelines
- **Limitations:**
    - Narrower scope than Bento
    - Preprint stage
- **Technology compatibility:** MERFISH, Xenium, seqFISH

### SPLISOSM
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.08.26.609810)
- **Code:** [github.com/lmweber/SPLISOSM](https://github.com/lmweber/SPLISOSM)
- **Key innovation:** Analyzes isoform-level spatial expression patterns at subcellular resolution, connecting alternative splicing to transcript localization.
- **Strengths:**
    - Uniquely addresses isoform-level spatial patterns
    - Connects splicing biology to spatial localization
- **Limitations:**
    - Requires isoform-resolution spatial data (currently rare)
    - Preprint with very limited validation
- **Technology compatibility:** Platforms with isoform-resolution detection

## Benchmark Summary

Subcellular spatial analysis is an emerging field with no formal benchmarks. **Bento** is the most comprehensive toolkit and serves as the de facto standard for subcellular transcript analysis. The field is limited by the availability of data with true subcellular resolution and by the quality of nuclear and cell segmentation — subcellular analyses inherit and amplify any segmentation errors from upstream steps.

!!! warning "Segmentation quality is critical"
    Subcellular analysis results are only as good as the cell and nuclear segmentation. A transcript classified as "cytoplasmic" may simply be misassigned to the wrong cell. Always validate segmentation quality before interpreting subcellular localization patterns.

!!! tip "Start with Bento"
    Bento provides the most complete and well-documented framework for subcellular analysis. Use it as the starting point, and add SPRAWL or InSTAnT for specific statistical tests.

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| Subcellular-resolution FISH | Comprehensive subcellular analysis | Bento | Most complete toolkit for subcellular patterns |
| Subcellular-resolution FISH | Test localization significance | SPRAWL | Focused statistical testing framework |
| Dense subcellular transcripts | Discover co-localized gene programs | FISHFactor | Non-negative factorization of spatial patterns |
| Subcellular-resolution FISH | Find co-localizing transcript pairs | InSTAnT | Pairwise co-localization testing |
| Any subcellular data | Nuclear/cytoplasmic ratios | troutpy or Bento | Quantify compartmentalized localization |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| Bento | - | - | Yes | Yes | - | - | - |
| SPRAWL | - | - | Yes | Yes | - | - | - |
| FISHFactor | - | - | - | Yes | - | - | - |
| InSTAnT | - | - | Yes | Yes | - | - | - |
| troutpy | - | - | Yes | Yes | - | - | - |
| SPLISOSM | - | - | - | - | - | - | - |
