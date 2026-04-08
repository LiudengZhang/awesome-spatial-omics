# SpatialData

**Verdict:** The FAIR data framework the field needed -- the scverse answer to spatial omics data fragmentation.

**Citation:** Marconato L, Palla G, Yaber KA, et al. "SpatialData: an open and universal data framework for spatial omics." *Nature Methods* 21, 2196--2209 (2024). [DOI: 10.1038/s41592-024-02212-x](https://doi.org/10.1038/s41592-024-02212-x)

## Problem Setup

Spatial omics data is inherently multimodal: a single experiment can produce high-resolution images, segmentation masks, point coordinates (transcript locations), geometric annotations (region shapes), and quantified expression tables. Each technology (Visium, MERFISH, Xenium, CosMx, CODEX) outputs these elements in different formats. Before SpatialData, analysts cobbled together ad hoc data structures -- storing images as TIFFs, coordinates as CSVs, and expression as AnnData objects -- with no standardized way to keep these elements aligned in a common coordinate system.

## Method

SpatialData defines a unified in-memory and on-disk data model with five element types: **images** (raster data, stored as multiscale spatial images), **labels** (segmentation masks, integer-valued raster data), **points** (transcript or molecule coordinates), **shapes** (geometric annotations like polygons and circles), and **tables** (quantified expression matrices as AnnData objects). All elements are registered to a common coordinate system through spatial transformations, enabling multi-element queries like "give me the expression table for cells within this tissue region."

On disk, SpatialData uses the OME-NGFF (Zarr-based) format for images and labels, and Parquet for points and shapes, following FAIR (Findable, Accessible, Interoperable, Reusable) data principles. The Zarr backend enables lazy loading and chunked access, making it feasible to work with multi-gigabyte imaging datasets without loading everything into memory.

The framework integrates with the scverse ecosystem: tables are AnnData objects compatible with [Squidpy](squidpy.md), scanpy, and other scverse tools. A Napari plugin (spatialdata-io) provides interactive visualization of all element types in a shared coordinate space. Technology-specific readers convert raw outputs from Visium, Xenium, MERFISH, CosMx, and other platforms into SpatialData objects.

## Evaluation

SpatialData was evaluated on its ability to represent data from seven different spatial omics technologies in a single framework. The paper demonstrates end-to-end workflows on Visium, Xenium, MERFISH, and CosMx datasets, showing that the same analysis code can operate across technologies once data is converted to SpatialData format. Performance benchmarks show that Zarr-backed lazy loading reduces memory usage by 5--10x compared to loading full-resolution images.

Community adoption has been strong: SpatialData is the default data structure for Squidpy 2.0 and is integrated into the scverse ecosystem roadmap.

## Honest Assessment

**Strengths:**

- Solves the right problem at the right time: as spatial omics technologies proliferate, a universal data framework prevents the field from fragmenting into technology-specific silos.
- The five-element data model (images, labels, points, shapes, tables) is general enough to represent any current spatial omics modality.
- FAIR-compliant on-disk format (OME-NGFF/Zarr + Parquet) enables efficient storage, sharing, and lazy loading of large datasets.
- Deep scverse integration means existing scanpy/Squidpy workflows can operate on SpatialData objects with minimal modification.

**Limitations:**

- Significant learning curve: the coordinate system, spatial transformations, and multi-element queries introduce new concepts that are unfamiliar to analysts coming from AnnData-only workflows.
- The ecosystem is still maturing -- not all spatial analysis tools accept SpatialData as input natively, requiring conversion steps that can introduce friction.
- Technology-specific readers need ongoing maintenance as vendors update their output formats, creating a maintenance burden for the development team.
- Performance on very large datasets (e.g., whole-slide Xenium with millions of transcripts) can still be challenging despite lazy loading.

**Design Decision:** The key bet is that a *unified data model* across technologies is more valuable than optimized per-technology data structures. This mirrors the AnnData bet for scRNA-seq, which proved transformative: once the community agreed on a data format, interoperable tools followed. SpatialData is making the same bet for spatial omics, and early adoption patterns suggest it will succeed.
