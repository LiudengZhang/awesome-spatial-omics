# Tangram

**Verdict:** The bridge between scRNA-seq and spatial data -- versatile integration, but quality depends entirely on the reference.

**Citation:** Biancalani T, Scalia G, Buffoni L, et al. "Deep learning and alignment of spatially resolved single-cell transcriptomes with Tangram." *Nature Methods* 18, 1352--1362 (2021). [DOI: 10.1038/s41592-021-01264-7](https://doi.org/10.1038/s41592-021-01264-7)

## Problem Setup

Spatial transcriptomics platforms face a gene coverage tradeoff: sequencing-based methods (Visium) capture the whole transcriptome at multi-cell resolution, while imaging-based methods (MERFISH, STARmap) achieve single-cell resolution but for only hundreds of genes. Meanwhile, dissociated scRNA-seq provides full-transcriptome single-cell profiles but loses spatial information. Tangram bridges this gap by learning a mapping between scRNA-seq cells and spatial locations, enabling prediction of unmeasured genes at each spatial location and assignment of spatial coordinates to dissociated cells.

## Method

Tangram formulates the integration problem as an alignment task: given a set of scRNA-seq cells and a set of spatial locations, find the optimal mapping that assigns each cell to a spatial location such that the predicted gene expression at each location matches the observed spatial expression. The mapping is represented as a soft assignment matrix (each cell can contribute to multiple locations with different weights) and is optimized via gradient descent.

The loss function measures the cosine similarity between the predicted expression (sum of mapped scRNA-seq profiles at each location) and the observed spatial expression, computed over a set of shared "training" genes. Once the mapping is learned on training genes, it can predict expression for any gene measured in the scRNA-seq but not in the spatial data -- effectively imputing the full transcriptome at spatial resolution.

The optimization uses PyTorch and runs on GPU, converging in minutes for typical dataset sizes. Tangram can operate in two modes: "cells" mode maps individual cells to locations (best for imaging-based data with single-cell resolution), and "clusters" mode maps cell-type centroids (more appropriate for multi-cell-resolution data like Visium). A constraint option ensures that the total number of mapped cells matches the expected cell count per location.

## Evaluation

On MERFISH data from the mouse primary motor cortex, Tangram achieved a median correlation of 0.85 between predicted and held-out gene expression across spatial locations, outperforming gimVI and SpaGE on the same benchmark. The method correctly predicted spatial expression patterns for genes not included in the MERFISH panel, validated against matched ISH data from the Allen Brain Atlas.

On Visium data, Tangram successfully mapped scRNA-seq cell types to expected anatomical locations, though the coarser resolution makes cell-level validation more challenging. Gene imputation performance was lower on Visium than MERFISH, reflecting the difficulty of mapping to multi-cell spots.

## Honest Assessment

**Strengths:**

- Versatile design handles both imaging-based and sequencing-based spatial data through the cells/clusters mode distinction.
- Gene imputation capability effectively upgrades targeted spatial panels to near-whole-transcriptome coverage, which is especially valuable for MERFISH and Xenium experiments.
- Fast optimization via PyTorch with GPU support, converging in minutes for typical datasets.
- From the Broad Institute / Genentech team, well-maintained with active development and integration with [Squidpy](squidpy.md).

**Limitations:**

- Quality is entirely dependent on the scRNA-seq reference: if cell types present in the tissue are missing from the reference, they cannot be mapped, and the model silently assigns incorrect cells to those locations.
- Assumes the scRNA-seq reference is representative of the spatial tissue -- batch effects, dissociation artifacts, or tissue-specific expression changes can distort the mapping.
- Imputed gene expression is a prediction, not a measurement -- downstream analyses should account for imputation uncertainty, but Tangram does not provide confidence intervals.
- The cosine similarity loss treats all training genes equally, which may bias the mapping toward highly variable genes at the expense of subtle spatial patterns.

**Design Decision:** The key bet is that aligning scRNA-seq to spatial data via a learned mapping is more useful than analyzing each modality separately. This is correct when the reference is good -- the imputed whole-transcriptome spatial data enables analyses impossible with either modality alone. But the field needs better tools to diagnose when the reference is inadequate, because silent failures (plausible but wrong mappings) are more dangerous than explicit errors.
