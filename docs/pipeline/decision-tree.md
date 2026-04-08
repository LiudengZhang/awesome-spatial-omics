# Analysis Pipeline Decision Tree

This is the most practically useful page on the site. It answers two questions: **"I have data from technology X -- what tools should I use?"** and **"I want to answer question Y -- what category of tool do I need?"**

!!! warning "Opinionated recommendations"
    These pipelines reflect a synthesis of benchmarks, community adoption, and practical experience as of early 2026. The field moves fast. Where a clear winner exists, it is named; where the choice is context-dependent, alternatives are listed with trade-offs.

---

## Part 1: What technology do you have?

Use this table to jump to the technology-specific pipeline that matches your data.

| Technology | Type | Resolution | Recommended pipeline |
|-----------|------|-----------|---------------------|
| **Visium** | Sequencing | 55 um (multi-cell) | [Visium pipeline](#visium-pipeline) |
| **Visium HD** | Sequencing | 2 um (bins) | [Visium HD pipeline](#visium-hd-pipeline) |
| **Xenium** | Imaging | Subcellular | [Xenium pipeline](#xenium-pipeline) |
| **MERFISH / MERSCOPE** | Imaging | Subcellular | [MERFISH pipeline](#merfish--merscope-pipeline) |
| **CosMx SMI** | Imaging | Subcellular | [CosMx pipeline](#cosmx-pipeline) |
| **CODEX / PhenoCycler** | Protein | Subcellular | [CODEX pipeline](#codex--phenocycler-pipeline) |
| **Stereo-seq** | Sequencing | ~500 nm (bins) | [Stereo-seq pipeline](#stereo-seq-pipeline) |
| **Slide-seq V2** | Sequencing | 10 um | [Slide-seq pipeline](#slide-seq-v2-pipeline) |
| **GeoMx DSP** | Sequencing/Protein | ROI-level | [GeoMx pipeline](#geomx-dsp-pipeline) |

---

### Visium pipeline

The most mature ecosystem. Spot-level resolution (55 um) means each spot captures 1-10 cells, making deconvolution essential for cell-type inference.

```
Raw data
  |
  v
SpaceRanger (10x) ---- alignment + counting
  |
  v
SpotClean / SpatialDWLS ---- ambient RNA correction
  |
  v
Scanpy/Squidpy ---- standard QC, filtering, normalization, HVG selection
  |
  v
BayesSpace / STAGATE / SpaGCN ---- spatial domain identification
  |
  v
Cell2location / RCTD / Tangram ---- cell type deconvolution (requires scRNA-seq reference)
  |
  v
nnSVG / SPARK-X ---- spatially variable gene detection
  |
  v
CellChat v2 / LIANA+ ---- cell-cell communication inference
  |
  v
Squidpy / stLearn ---- neighborhood enrichment, spatial statistics
```

!!! tip "Key decisions"
    - **Deconvolution method**: Cell2location is the benchmark leader for Visium but requires a good scRNA-seq reference. RCTD is faster and more robust to reference mismatches. Tangram works well when the reference is from the same tissue.
    - **Spatial domains**: BayesSpace is Visium-native and leverages the grid structure. STAGATE uses graph attention networks and generalizes better. SpaGCN is simpler but less accurate in benchmarks.
    - **SVG detection**: nnSVG is the current benchmark leader. SPARK-X is faster for large datasets. Avoid older methods like SpatialDE v1 (slow, inflated false positives).

---

### Visium HD pipeline

Visium HD generates 2-um bins that must be aggregated before most analysis. The ecosystem is still maturing -- expect gaps and rough edges.

```
Raw data (2-um bins)
  |
  v
SpaceRanger (10x) ---- alignment + counting at 2-um and 8-um bins
  |
  v
Bin2Cell / ENACT ---- bin-to-cell aggregation using H&E image
  |
  v
Standard QC (Scanpy/Squidpy) ---- filtering, normalization
  |
  v
BANKSY / STAGATE ---- spatial domain identification (scalable methods required)
  |
  v
Cell2location / RCTD ---- deconvolution (if using 8-um bins without Bin2Cell)
  |
  v
nnSVG ---- spatially variable genes (subsample if needed for speed)
  |
  v
CellChat v2 / COMMOT ---- cell-cell communication
```

!!! info "Visium HD is still emerging"
    - Bin2Cell (Kleshchevnikov et al., 2024) and ENACT are the leading bin-to-cell methods but neither is fully benchmarked yet.
    - Many tools designed for Visium work on Visium HD 8-um bins but may need parameter tuning.
    - Memory and compute requirements are 10-100x larger than standard Visium. Dask-backed AnnData or SpatialData is often necessary.
    - The [Visium HD deep read](../deep-reads/visium-hd.md) covers practical considerations in detail.

---

### Xenium pipeline

Xenium provides molecule-level coordinates with pre-designed or custom panels. Segmentation is the critical first step.

```
Xenium Explorer output (transcripts + DAPI + cell boundaries)
  |
  v
Cellpose2 / Baysor ---- cell segmentation (or use 10x default boundaries as starting point)
  |
  v
SpatialData / Squidpy ---- data loading + QC
  |
  v
BANKSY / GraphST ---- spatial domain identification
  |
  v
STELLAR / Tangram ---- cell type annotation (transfer from scRNA-seq reference)
  |
  v
CellChat v2 / COMMOT ---- cell-cell communication
  |
  v
Squidpy ---- neighborhood enrichment, co-occurrence, spatial statistics
```

!!! tip "Key decisions"
    - **Segmentation**: The 10x-provided cell boundaries are reasonable for many analyses. Re-segmentation with Cellpose2 (using DAPI + membrane stains) or Baysor (transcript-based, no image needed) can improve results in dense tissues.
    - **Cell typing**: With targeted panels (100-5000 genes), direct annotation via marker-based approaches may outperform reference-based transfer. STELLAR is designed for spatial data and handles novel cell types.
    - **CCC**: COMMOT is specifically designed for imaging-based spatial data and uses optimal transport. CellChat v2 now supports spatial coordinates directly.

---

### MERFISH / MERSCOPE pipeline

MERFISH/MERSCOPE data is structurally similar to Xenium but typically uses the Vizgen pipeline for initial processing.

```
MERSCOPE output (transcripts + cell boundaries)
  |
  v
Baysor / Cellpose2 ---- re-segmentation (Vizgen defaults are often suboptimal)
  |
  v
Squidpy / SpatialData ---- data loading + QC
  |
  v
GraphST / BANKSY ---- spatial domain identification
  |
  v
COMMOT ---- cell-cell communication (optimal transport on coordinates)
  |
  v
nnSVG / SPARK-X ---- spatially variable genes
```

!!! warning "MERSCOPE commercial status"
    10x Genomics acquired Vizgen in 2024. The MERSCOPE platform continues to operate but long-term product roadmap is uncertain. Existing data and workflows remain valid.

---

### CosMx pipeline

CosMx provides single-molecule imaging with both RNA and protein readouts. NanoString (now part of Bruker) provides initial cell segmentation.

```
CosMx output (transcripts + cell boundaries)
  |
  v
InSituType ---- probabilistic cell typing (NanoString's method, well-suited to CosMx data)
  |
  v
SpaGCN / BANKSY ---- spatial domain identification
  |
  v
CellChat v2 ---- cell-cell communication
  |
  v
Squidpy ---- spatial statistics and neighborhood analysis
```

!!! info "CosMx considerations"
    - InSituType is optimized for CosMx data and often outperforms general-purpose methods on this platform.
    - CosMx supports simultaneous RNA + protein measurement, but most computational tools do not yet handle multi-modal CosMx data natively. Process RNA and protein separately, then integrate.
    - NanoString was acquired by Bruker in 2024; platform support continues under the Bruker Spatial Biology brand.

---

### CODEX / PhenoCycler pipeline

Protein-based spatial data requires a fundamentally different analytical approach -- no gene expression matrices, no deconvolution. Analysis centers on cell phenotyping from marker intensities and neighborhood structure.

```
PhenoCycler images (multichannel fluorescence)
  |
  v
Mesmer / DeepCell ---- cell segmentation from nuclear + membrane markers
  |
  v
Intensity extraction ---- per-cell marker expression matrix
  |
  v
FlowSOM / Leiden clustering ---- cell phenotyping from marker profiles
  |
  v
Neighborhood analysis (Squidpy / ATHENA) ---- cellular neighborhood identification
  |
  v
Spatial statistics ---- co-occurrence, interaction scores, community detection
```

!!! tip "Key decisions"
    - **Segmentation**: Mesmer (DeepCell) is the benchmark leader for protein imaging data. It handles variable marker quality better than classical watershed approaches.
    - **Phenotyping**: Manual gating is still common but does not scale. FlowSOM with expert-guided metaclustering offers a good balance. Avoid over-clustering.
    - **Neighborhood analysis**: This is where the unique value of spatial proteomics lies. Squidpy provides neighborhood enrichment tests; ATHENA offers more sophisticated niche identification.

---

### Stereo-seq pipeline

Stereo-seq generates the largest spatial datasets (subcellular resolution across centimeter-scale tissue), requiring scalable tools throughout.

```
Raw data (~500 nm bins)
  |
  v
SAW (BGI pipeline) ---- alignment + counting
  |
  v
Bin aggregation (cell-bin or fixed-size bins) ---- typically 50-100 um bins for tissue-level, or cell-bin via segmentation
  |
  v
STAGATE / BANKSY ---- spatial domain identification (must handle millions of bins)
  |
  v
Cell2location / RCTD ---- deconvolution (if using larger bins)
  |
  v
nnSVG ---- spatially variable genes (subsampling likely necessary)
```

!!! warning "Scale challenges"
    Stereo-seq datasets can exceed 100 million bins. Most standard tools will fail without subsampling or chunked processing. STAGATE and BANKSY are among the few spatial domain methods that scale to this size. Use SpatialData or Dask-backed workflows.

---

### Slide-seq V2 pipeline

Slide-seq V2 achieves 10-um resolution on fresh-frozen tissue with bead-based capture. The pipeline is similar to Visium but with higher resolution and noisier per-bead data.

```
Puck data (bead x gene matrix + coordinates)
  |
  v
Standard QC (Scanpy) ---- filter low-quality beads, normalize
  |
  v
RCTD / Cell2location ---- deconvolution (RCTD was originally developed for Slide-seq)
  |
  v
BayesSpace / STAGATE ---- spatial domain identification
  |
  v
nnSVG / SPARK-X ---- spatially variable genes
```

!!! info "Slide-seq context"
    RCTD was originally developed and validated on Slide-seq data, making it a natural first choice for deconvolution on this platform. Slide-seq is primarily an academic technology; commercial adoption is limited.

---

### GeoMx DSP pipeline

GeoMx provides region-of-interest (ROI) level data, not single-cell. Analysis resembles bulk RNA-seq with spatial annotation rather than spatial transcriptomics proper.

```
GeoMx output (ROI x gene/protein matrix)
  |
  v
GeomxTools (NanoString R package) ---- QC, normalization
  |
  v
Standard differential expression (DESeq2 / limma) ---- ROI-level comparisons
  |
  v
Spatial context is metadata ---- annotate ROIs with tissue region, distance to feature, etc.
```

!!! info "GeoMx limitations"
    GeoMx is not single-cell and not truly spatially resolved at the cellular level. It is best suited for clinical FFPE samples where other technologies fail, or for hypothesis-driven ROI comparisons. Do not apply single-cell spatial methods to GeoMx data.

---

## Part 2: What question are you asking?

Use this table to identify the right category of tool for a biological question, then follow the link to the detailed methods page.

| Question | Tool category | Top picks | Methods page |
|----------|--------------|-----------|-------------|
| What cell types are present and where? | Deconvolution / Cell typing | Cell2location, RCTD, Tangram, InSituType | [Deconvolution](../methods/deconvolution.md) |
| Which genes vary spatially? | SVG detection | nnSVG, SPARK-X, SpatialDE2 | [Spatially Variable Genes](../methods/spatially-variable-genes.md) |
| What are the tissue domains / regions? | Spatial domain identification | BANKSY, GraphST, STAGATE, BayesSpace | [Spatial Domains](../methods/spatial-domains.md) |
| How do cells communicate? | Cell-cell communication | COMMOT, CellChat v2, LIANA+ | [Cell-Cell Communication](../methods/cell-cell-communication.md) |
| What are the cellular neighborhoods? | Niche analysis | Squidpy, ATHENA, Nicheformer | [Spatial Domains](../methods/spatial-domains.md) |
| Are genes differentially expressed between regions? | Spatial DE | GLISS, DestDE, SpatialDWLS | [Spatial DE](../methods/spatial-de.md) |
| Where are transcripts within cells? | Subcellular analysis | Bento, Ficture, subcellular segmentation | [Subcellular Analysis](../methods/subcellular-analysis.md) |
| How do cell states change across space? | Spatial trajectories | stLearn, SpaceFlow, CellRank | [Spatial Trajectories](../methods/spatial-trajectories.md) |
| How do I segment cells from images? | Cell segmentation | Cellpose2, Baysor, Mesmer/DeepCell | [Cell Segmentation](../methods/cell-segmentation.md) |
| How do I integrate spatial with scRNA-seq? | Multi-modal integration | Tangram, gimVI, SpatialData | [Multi-modal Integration](../methods/multimodal-integration.md) |
| Can I use a foundation model? | Foundation models | scGPT, Geneformer, Nicheformer | [Foundation Models](../methods/foundation-models.md) |
| How do I align serial sections in 3D? | 3D reconstruction | PASTE, STalign, GPSA | [3D Reconstruction](../methods/3d-reconstruction.md) |

---

## Part 3: Common pitfalls

!!! danger "Mistakes that waste months"
    1. **Skipping deconvolution on Visium data.** Each 55-um spot contains multiple cells. Treating spots as cells leads to chimeric expression profiles and misleading downstream results.
    2. **Using SpatialDE v1 for SVG detection.** It is slow, has inflated false positive rates, and has been superseded by nnSVG and SPARK-X in every benchmark.
    3. **Ignoring segmentation quality on imaging data.** Garbage segmentation propagates through every downstream step. Always visually inspect segmentation results on multiple tissue regions before proceeding.
    4. **Applying scRNA-seq CCC methods to spatial data without coordinates.** CellChat v1 and CellPhoneDB do not use spatial coordinates. Use COMMOT, CellChat v2, or LIANA+ with spatial mode for coordinate-aware communication inference.
    5. **Running standard Visium tools on Visium HD without aggregation.** Visium HD 2-um bins are not cells. Aggregate to 8-um bins or use Bin2Cell before applying standard pipelines.
    6. **Neglecting ambient RNA correction.** Spatial platforms have significant ambient RNA contamination, especially Visium. SpotClean or similar correction should precede downstream analysis.

---

## Part 4: Framework and infrastructure choices

Regardless of technology, spatial data benefits from standardized data structures and workflow frameworks.

| Need | Recommended tool | Notes |
|------|-----------------|-------|
| Unified data structure | SpatialData | Handles images, points, shapes, and tables in a single object. Actively developed by the scverse consortium. |
| Spatial statistics toolkit | Squidpy | Neighborhood enrichment, co-occurrence, spatial autocorrelation, and more. Integrates with AnnData/SpatialData. |
| Visualization | Napari + napari-spatialdata | Interactive visualization of spatial data layers. Essential for QC and segmentation validation. |
| Scalable processing | Dask-backed AnnData | Required for Visium HD and Stereo-seq scale data. |
| Pipeline orchestration | Nextflow / Snakemake | For reproducible, end-to-end spatial analysis pipelines. |
