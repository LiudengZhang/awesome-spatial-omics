# Cell2location

**Verdict:** Bayesian deconvolution done right -- best accuracy and uncertainty quantification, worth the computational cost.

**Citation:** Kleshchevnikov V, Shmatko A, Dann E, et al. "Cell2location maps fine-grained cell types in spatial transcriptomics." *Nature Biotechnology* 40, 661--671 (2022). [DOI: 10.1038/s41587-021-01139-4](https://doi.org/10.1038/s41587-021-01139-4)

## Problem Setup

Sequencing-based spatial transcriptomics platforms like Visium capture gene expression at spots that typically contain 1--10 cells. The measured expression at each spot is a mixture of contributions from multiple cell types. Deconvolution -- estimating how many cells of each type are present at each spot -- is essential for interpreting spatial data at cell-type resolution. The challenge is doing this accurately, especially for rare cell types, while providing uncertainty estimates that indicate where the model is confident and where it is guessing.

## Method

Cell2location uses a Bayesian hierarchical model with two stages. In the first stage, cell-type-specific gene expression signatures are learned from a matched scRNA-seq reference dataset using a negative binomial regression model. This step accounts for technical variation between the reference and spatial datasets (e.g., different sequencing depths, platform effects).

In the second stage, these learned signatures are used to decompose each spatial spot's expression profile into a weighted combination of cell types. The model explicitly estimates the *absolute number* of cells of each type per spot (not just proportions), accounting for total mRNA content variation across spots. The Bayesian framework provides posterior distributions over cell abundances, yielding uncertainty estimates for every prediction.

Inference is performed using variational inference (via Pyro), which is faster than MCMC sampling while still providing approximate posterior distributions. The model includes informative priors on cell abundance that regularize estimates for rare cell types, preventing the overfitting that plagues simpler regression-based approaches.

## Evaluation

On synthetic spatial data with known ground truth, Cell2location achieved the highest correlation between estimated and true cell-type abundances compared to RCTD, SPOTlight, stereoscope, and other deconvolution methods. On real mouse brain Visium data, the method correctly localized interneuron subtypes to known anatomical layers, with uncertainty estimates that were well-calibrated -- high confidence in regions with abundant cell types and appropriate uncertainty in transition zones.

In the comprehensive Li et al. (2023) benchmark, Cell2location consistently ranked first or second across metrics, with the strongest performance on rare cell type detection. The main cost is runtime: fitting the full model takes 30--60 minutes on GPU for a typical Visium dataset, compared to seconds for RCTD.

## Honest Assessment

**Strengths:**

- Best-in-class accuracy for spatial deconvolution across multiple independent benchmarks, particularly strong for rare cell types that other methods miss.
- Provides calibrated uncertainty estimates through the Bayesian framework, enabling principled downstream decisions about which regions to trust.
- Estimates absolute cell counts per spot rather than just proportions, preserving information about cell density variation across tissue.
- The two-stage design separates reference learning from spatial decomposition, making it modular and robust to reference quality variation.

**Limitations:**

- Requires a high-quality, matched scRNA-seq reference dataset -- performance degrades significantly if the reference lacks cell types present in the tissue or has batch effects relative to the spatial data.
- Computationally expensive: 30--60 minutes per dataset on GPU, which becomes impractical for large-scale studies or [Visium HD](visium-hd.md) data without aggregation.
- The variational inference approximation, while faster than MCMC, can underestimate posterior uncertainty in some cases.
- Assumes that cell-type expression signatures are constant across spatial locations, ignoring spatially varying gene programs within a cell type.

**Design Decision:** The key bet is that a full Bayesian generative model with informative priors is worth the computational cost over simpler regression or NMF approaches. The benchmark results decisively validate this -- the accuracy gap is large enough that Cell2location should be the default choice unless dataset size makes it prohibitive. The field would benefit from scalable approximations that preserve the Bayesian advantages at [Visium HD](visium-hd.md) scale.
