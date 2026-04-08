# Cellpose

**Verdict:** The generalist segmentation algorithm that just works -- retrain on your tissue if the defaults fall short.

**Citation:** Stringer C, Wang T, Michaelos M, Pachitariu M. "Cellpose: a generalist algorithm for cellular segmentation." *Nature Methods* 18, 100--106 (2021). [DOI: 10.1038/s41592-020-01018-x](https://doi.org/10.1038/s41592-020-01018-x)

## Problem Setup

Cell segmentation -- drawing boundaries around individual cells in microscopy images -- is the critical first step for any imaging-based spatial omics experiment. Before Cellpose, most segmentation tools were either hand-tuned for specific cell types (e.g., round cells only) or required large annotated training sets for each new tissue. A generalist algorithm that works across cell types, imaging modalities, and tissue contexts without per-experiment retraining was the missing piece.

## Method

Cellpose introduces a gradient flow representation for cell segmentation. Instead of directly predicting cell boundaries (which are thin and hard to detect), the network predicts a smooth vector field that flows from every pixel toward the center of the cell it belongs to. Pixels that converge to the same center are grouped into one cell. This representation handles irregular cell shapes naturally -- the flow field is well-defined even for highly elongated or branching cells.

The architecture is a U-Net variant trained on a diverse dataset of fluorescence, brightfield, and phase-contrast images spanning neurons, bacteria, and epithelial cells. Two channels are expected as input: one for cell body (cytoplasm) and one for nuclei, though the model can work with either alone. The key design choice is the gradient flow representation itself -- by predicting a continuous field rather than discrete boundaries, the model avoids the fragmentation errors that plague watershed-based and contour-based methods.

Cellpose 2.0 added a human-in-the-loop retraining capability via a GUI: users correct a few segmentation errors, and the model fine-tunes on those corrections, typically requiring only 5--10 annotated images to adapt to a new cell type.

## Evaluation

On the paper's benchmark of 608 images spanning multiple cell types and imaging modalities, Cellpose outperformed StarDist, Mask R-CNN, and traditional watershed methods. Average precision at IoU 0.5 reached 0.86 on held-out test images, compared to 0.76 for StarDist on the same data. The model generalized to cell types not seen during training (e.g., plant cells), though performance dropped for extremely dense tissues where cells are tightly packed. Independent benchmarks in the spatial omics community have confirmed Cellpose as the default choice for image-based segmentation tasks.

## Honest Assessment

**Strengths:**

- Genuine generalist: works across fluorescence, brightfield, and H&E without architecture changes, making it the default starting point for any segmentation task.
- The gradient flow representation elegantly handles irregular cell shapes that defeat watershed and contour-based methods.
- Human-in-the-loop retraining via Cellpose 2.0's GUI makes adaptation to new tissues practical with minimal annotation effort (5--10 images).
- Large and active community, well-maintained codebase, and integration with downstream tools like [Squidpy](squidpy.md) and [SpatialData](spatialdata.md).

**Limitations:**

- Requires nuclei or membrane staining as input -- cannot segment cells from transcript coordinates alone, limiting applicability to sequencing-only platforms like Visium or Slide-seq.
- Performance degrades in extremely dense tissues (e.g., lymph node germinal centers) where cell boundaries are ambiguous even to human annotators.
- The U-Net backbone, while effective, does not leverage the 3D context available in volumetric imaging data without separate 3D models.
- Assumes cells are the unit of segmentation -- subcellular compartment segmentation (e.g., dendrites, axons) requires different approaches.

**Design Decision:** The central bet is that a *representation change* (gradient flows instead of boundary detection) matters more than model scale or training data size. This has proven remarkably durable -- Cellpose remains competitive against newer, larger models precisely because the flow representation captures the right inductive bias for cell shapes.
