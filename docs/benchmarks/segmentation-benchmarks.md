# Cell Segmentation Benchmarks

**Pipeline question:** Which method best assigns pixels or transcripts to individual cells in spatial omics data?

## Overview

Cell segmentation is the critical preprocessing step for all imaging-based spatial technologies. Every downstream analysis -- cell typing, niche detection, cell-cell communication -- depends on correctly assigning molecules to cells. Two major benchmarks have systematically compared segmentation methods, revealing that the best approach depends on whether the primary input is images (DAPI, membrane stains) or transcript coordinates.

## Key benchmark studies

### Petukhov et al., Nature Biotechnology 2023 — 7+ methods

- **Paper:** [Cell segmentation in imaging-based spatial transcriptomics](https://doi.org/10.1038/s41587-023-01766-1)
- **Methods tested:** Baysor, Cellpose2, StarDist, Watershed, Voronoi, pciSeq, ClusterMap, and hybrid approaches
- **Datasets:** MERFISH (mouse brain, liver), osmFISH, seqFISH+, simulated data
- **Evaluation metrics:** Segmentation accuracy (F1), transcript assignment accuracy, downstream clustering quality

**Key findings:**

| Approach | Best method | F1 score range | Notes |
|---|---|---|---|
| Image-based | Cellpose2 | 0.75--0.85 | Requires DAPI or membrane stain |
| Transcript-based | Baysor | 0.70--0.80 | Works without any imaging |
| Hybrid | Baysor + Cellpose prior | 0.80--0.90 | Best overall performance |
| Classical | Watershed | 0.50--0.65 | Baseline; over-segments dense regions |
| Geometry-based | Voronoi | 0.45--0.60 | Baseline; assumes uniform cell size |

**Critical insight:** Using Baysor with a Cellpose-derived cell boundary prior consistently outperformed either method alone. The image-based prior constrains cell boundaries while the transcript density model handles ambiguous regions and cells missed by imaging.

### Greenwald et al., Nature Biotechnology 2022

- **Paper:** [Whole-cell segmentation of tissue images with human-level performance using large-scale data annotation and deep learning](https://doi.org/10.1038/s41587-021-01094-0)
- **Methods tested:** Mesmer (DeepCell), Cellpose, StarDist, Watershed, ilastik
- **Datasets:** TissueNet (>1 million annotated cells from multiplexed imaging), MIBI-TOF, CODEX, CyCIF, IMC, vectra
- **Evaluation metrics:** Whole-cell segmentation accuracy, nuclear segmentation accuracy, generalization across tissues

**Key findings:**

| Method | Nuclear seg. | Whole-cell seg. | Generalization | Notes |
|---|---|---|---|---|
| Mesmer/DeepCell | Excellent | Best | Best across tissues | Trained on TissueNet |
| Cellpose | Excellent | Good | Good | General-purpose, not tissue-specific |
| StarDist | Good | Moderate | Good for round cells | Struggles with irregular shapes |
| Watershed | Moderate | Poor | Poor | Requires extensive tuning |

## Consensus findings

### Cellpose2 wins image-based segmentation

Cellpose2 is the most widely validated deep learning model for nuclear and cell segmentation from DAPI or membrane stain images. Its generalist model works across tissue types without retraining, and fine-tuning on tissue-specific data further improves performance. For spatial transcriptomics platforms that provide high-quality nuclear staining (Xenium, MERSCOPE, CosMx), Cellpose2 is the recommended starting point.

!!! tip "Cellpose2 vs. Mesmer"
    Cellpose2 excels on DAPI-stained spatial transcriptomics data. Mesmer/DeepCell excels on multiplexed protein imaging (CODEX, MIBI-TOF, IMC) where whole-cell membrane stains are available. For nuclear-only segmentation, the two are comparable.

### Baysor wins transcript-based segmentation

Baysor operates directly on transcript coordinates without requiring any imaging input. It uses a Bayesian mixture model to assign transcripts to cells based on spatial density patterns. This makes it uniquely suited for technologies where imaging quality is poor or unavailable, and for handling transcripts that fall outside image-derived cell boundaries.

### Hybrid approaches are best overall

The strongest result from the Petukhov et al. benchmark is that combining image-based and transcript-based approaches outperforms either alone. The recommended workflow:

1. Run Cellpose2 on the nuclear/membrane stain to get initial cell boundaries
2. Use these boundaries as a prior for Baysor
3. Baysor refines boundaries, assigns extracellular transcripts, and identifies cells missed by imaging

This hybrid approach achieves the highest F1 scores and produces the cleanest downstream clustering results.

### Segmentation errors propagate downstream

!!! warning "The segmentation bottleneck"
    Segmentation quality has a larger effect on downstream results than the choice of clustering method, differential expression test, or cell-cell communication tool. Over-segmentation (splitting one cell into fragments) creates artificial cell types. Under-segmentation (merging adjacent cells) blurs cell-type boundaries. Investing time in segmentation quality control pays dividends throughout the analysis.

Common segmentation failure modes:

- **Dense tissue regions:** Cells packed tightly lead to under-segmentation
- **Elongated cells:** Neurons, fibroblasts, and other non-round cells are poorly captured by methods assuming circular shapes
- **Low-density regions:** Sparse transcript counts make Baysor unreliable; image-based methods are more robust here
- **Tissue edges and folds:** Artifacts at tissue boundaries produce spurious cell calls

## Technology-specific recommendations

| Technology | Imaging available | Recommended approach |
|---|---|---|
| Xenium | DAPI + morphology | Cellpose2 then Baysor refinement |
| MERSCOPE (MERFISH) | DAPI | Cellpose2 then Baysor refinement |
| CosMx | Morphology markers | Cellpose2 or vendor pipeline |
| seqFISH | DAPI | Cellpose2 then Baysor refinement |
| CODEX/PhenoCycler | Membrane + nuclear | Mesmer/DeepCell |
| MIBI-TOF | Membrane + nuclear | Mesmer/DeepCell |
| IMC | Membrane + nuclear | Mesmer/DeepCell or ilastik + CellProfiler |
| Stereo-seq (no imaging) | None | Baysor (transcript-only mode) |

## Practical considerations

**Computational cost.** Cellpose2 and Mesmer are GPU-accelerated and can segment a full tissue section in minutes. Baysor is CPU-based and can take 1--4 hours for large MERFISH datasets with millions of transcripts. The hybrid pipeline adds overhead but is typically manageable.

**Quality control.** Always visually inspect segmentation results overlaid on the original image or transcript map. Automated QC metrics (cells per area, transcripts per cell distribution, cell size distribution) should be checked for biologically reasonable values.

**Retraining.** For non-standard tissues or unusual staining protocols, fine-tuning Cellpose2 on a small set of manually annotated cells (50--200) can substantially improve performance. Mesmer's TissueNet training set is large enough that retraining is rarely necessary for multiplexed imaging.

## Further reading

- [Benchmark Synthesis](benchmark-synthesis.md) for cross-category findings
- [The Technology-Analysis Gap](../synthesis/technology-analysis-gap.md) for why segmentation remains a bottleneck
- [Datasets](../datasets/datasets.md) for benchmark dataset details
