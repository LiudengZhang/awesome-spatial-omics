# Nicheformer

**Verdict:** 49M parameters beats 444M -- spatial context matters more than model scale for single-cell foundation models.

**Citation:** Schaar AC, Gruber M, Gao L, et al. "Nicheformer: a foundation model for single-cell and spatial omics." *Nature Methods* (2025). [DOI: 10.1038/s41592-025-02609-2](https://doi.org/10.1038/s41592-025-02609-2)

## Problem Setup

Foundation models for single-cell genomics aim to learn general-purpose representations from large-scale atlases that transfer to downstream tasks (cell type annotation, perturbation prediction, spatial domain identification). Existing models like scGPT, Geneformer, and scFoundation scale to hundreds of millions of parameters but train exclusively on dissociated scRNA-seq data, discarding spatial context. Nicheformer asks whether incorporating spatial neighborhood information during pretraining produces better representations, even with a smaller model.

## Method

Nicheformer is a transformer-based foundation model pretrained on approximately 110 million cells from both dissociated scRNA-seq and spatial transcriptomics datasets. The architecture uses 49 million parameters -- roughly one-ninth the size of TranscriptFormer (444M). The model operates on gene expression tokens and uses masked gene prediction as its pretraining objective, similar to BERT's masked language modeling.

The distinguishing feature is spatial context integration through a "niche" mechanism. For cells that come from spatial datasets, the model aggregates expression information from each cell's spatial neighborhood (its niche) and incorporates this as additional context during pretraining. The niche is defined as the k nearest spatial neighbors, and their aggregated expression profile is provided to the transformer alongside the focal cell's own expression. This teaches the model that gene expression is not cell-autonomous -- a cell's state depends on its neighbors.

For cells from dissociated scRNA-seq (which lack spatial coordinates), the spatial context is simply absent, and the model learns from expression alone. This dual-mode design allows Nicheformer to leverage both spatial and non-spatial datasets during pretraining, maximizing training data while learning spatial awareness.

After pretraining, the model produces cell embeddings that can be fine-tuned or used directly for downstream tasks: cell type classification, spatial domain identification, batch integration, and perturbation response prediction.

## Evaluation

On cell type annotation benchmarks across multiple tissues, Nicheformer (49M) matched or outperformed TranscriptFormer (444M), scGPT (51M), and Geneformer (10M). For spatial domain identification on the DLPFC benchmark, Nicheformer's spatial context provided a clear advantage, achieving smoother and more accurate domain boundaries than models trained without spatial awareness.

The most striking result is the parameter efficiency: Nicheformer achieves comparable performance to TranscriptFormer with 9x fewer parameters, suggesting that the spatial inductive bias is more valuable than additional model capacity. On perturbation prediction tasks, performance was competitive but not dominant, indicating that spatial pretraining helps most for spatially relevant downstream tasks.

## Honest Assessment

**Strengths:**

- Demonstrates that spatial context during pretraining is more valuable than model scale, challenging the "bigger is better" assumption in single-cell foundation models.
- Parameter-efficient design (49M) makes fine-tuning accessible on standard academic GPUs, unlike 400M+ parameter models.
- Dual-mode pretraining on both spatial and dissociated data maximizes data utilization without requiring all training data to be spatial.
- Strong performance across diverse downstream tasks suggests the learned representations capture genuine biological structure.

**Limitations:**

- The niche definition (k nearest neighbors) is implicit and fixed during pretraining -- different tissues have different neighborhood scales, and the model cannot adapt its niche size dynamically.
- Spatial awareness is acquired during pretraining but the mechanism for how it transfers to non-spatial downstream tasks is not fully characterized.
- The evaluation relies heavily on the DLPFC benchmark for spatial tasks (see [Benchmark Synthesis](benchmark-synthesis.md)), and generalization to diverse tissue architectures is not extensively tested.
- As a foundation model, it requires substantial pretraining compute (though less than larger alternatives), and the benefit over simpler task-specific models varies by application.

**Design Decision:** The central bet is that *spatial inductive bias beats scale* -- that teaching a model about cellular neighborhoods during pretraining is more efficient than simply adding parameters. The results support this bet convincingly for spatially relevant tasks. This has implications beyond spatial omics: it suggests that domain-specific structural priors can substitute for brute-force scaling in biological foundation models.
