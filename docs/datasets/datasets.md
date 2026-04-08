# Datasets

A curated guide to spatial omics datasets commonly used for benchmarking, method development, and biological discovery. Organized by use case to help identify the right dataset for a given analysis need.

## Standard benchmark datasets

These datasets are the most frequently used in spatial omics benchmarking papers. They have well-characterized ground truth annotations and are widely available.

| Name | Technology | Tissue | Spots/Cells | Genes | Ground truth | Key paper | Download |
|---|---|---|---|---|---|---|---|
| DLPFC | Visium | Human dorsolateral prefrontal cortex | ~3,500/sample (12 samples) | ~33,000 | Manual layer annotations (L1--L6, WM) | [Maynard et al. 2021](https://doi.org/10.1038/s41593-020-00787-0) | [spatialLIBD](http://research.libd.org/spatialLIBD/) |
| Mouse brain MERFISH | MERFISH | Mouse whole brain | ~5 million cells | 500 | Allen Brain Cell Atlas cell types | [Allen Institute](https://alleninstitute.org/) | [Allen Brain Cell Atlas](https://portal.brain-map.org/atlases-and-data/bkp/abc-atlas) |
| Mouse embryo seqFISH | seqFISH | Mouse embryo (E8.5--E9.5) | ~20,000 cells | 351 | Expert cell-type annotations | [Lohoff et al. 2022](https://doi.org/10.1038/s41587-021-01006-2) | [ArrayExpress](https://www.ebi.ac.uk/arrayexpress/experiments/E-MTAB-11155/) |
| Slide-seqV2 cerebellum | Slide-seqV2 | Mouse cerebellum | ~40,000 beads | ~20,000 | Known cerebellar architecture | [Stickels et al. 2021](https://doi.org/10.1038/s41587-020-0739-1) | [Broad Single Cell Portal](https://singlecell.broadinstitute.org/) |
| MIBI-TOF breast | MIBI-TOF | Human breast cancer | ~200,000 cells | 36 proteins | Expert phenotyping | [Keren et al. 2018](https://doi.org/10.1016/j.cell.2018.08.039) | [Zenodo](https://zenodo.org/) |
| CosMx NSCLC | CosMx SMI | Human non-small cell lung cancer | ~800,000 cells | 960 | Pathologist annotations | [He et al. 2022](https://doi.org/10.1038/s41587-022-01483-z) | [NanoString](https://nanostring.com/products/cosmx-spatial-molecular-imager/ffpe-dataset/) |
| Stereo-seq mouse embryo | Stereo-seq | Mouse embryo (E9.5--E16.5) | ~100 million spots | ~25,000 | Developmental stage annotations | [Chen et al. 2022](https://doi.org/10.1016/j.cell.2022.04.003) | [STOmicsDB](https://db.cngb.org/stomics/) |
| Xenium breast cancer | Xenium | Human breast cancer | ~100,000 cells | 313 | Pathologist annotations | [Janesick et al. 2023](https://doi.org/10.1038/s41467-023-43458-x) | [10x Genomics](https://www.10xgenomics.com/datasets) |
| CODEX mouse spleen | CODEX | Mouse spleen | ~70,000 cells | 30 proteins | Known splenic architecture | [Goltsev et al. 2018](https://doi.org/10.1016/j.cell.2018.07.010) | [HuBMAP](https://hubmapconsortium.org/) |

!!! warning "The DLPFC caveat"
    The DLPFC dataset is by far the most common benchmark for spatial domain detection. Its layered cortical architecture makes it relatively easy to segment into domains. Performance on DLPFC may not generalize to tissues with irregular or overlapping spatial patterns. See [Benchmark Synthesis](../benchmarks/benchmark-synthesis.md) for discussion.

## Large-scale atlases

These atlas-scale datasets provide comprehensive spatial maps of organs or organisms. They are valuable for reference-based analyses, atlas-level questions, and training foundation models.

| Name | Technology | Scope | Scale | Access |
|---|---|---|---|---|
| Allen Brain Cell Atlas | MERFISH + scRNA-seq | Mouse whole brain | ~5 million cells, 500 genes (MERFISH) + full transcriptome (scRNA-seq) | [portal.brain-map.org](https://portal.brain-map.org/atlases-and-data/bkp/abc-atlas) |
| HuBMAP | Multiple (Visium, CODEX, MALDI, etc.) | Human organs (kidney, intestine, spleen, etc.) | Millions of cells across tissues | [hubmapconsortium.org](https://hubmapconsortium.org/) |
| Human Cell Atlas (spatial) | Multiple | Human organs | Varies by consortium | [humancellatlas.org](https://www.humancellatlas.org/) |
| CZ CELLxGENE Census | scRNA-seq + spatial | Human + mouse, many tissues | >50 million cells (Census), spatial collections growing | [cellxgene.cziscience.com](https://cellxgene.cziscience.com/) |
| Mouse Brain MERFISH (BICCN) | MERFISH | Mouse motor cortex | ~300,000 cells, 252 genes | [BICCN portal](https://biccn.org/) |

## Disease-specific datasets

These datasets are particularly relevant for translational research and clinical spatial omics.

| Name | Technology | Disease/Tissue | Key finding | Reference |
|---|---|---|---|---|
| Visium FFPE breast cancer | Visium | Breast cancer (FFPE) | Spatial heterogeneity of tumor subtypes within single sections | [10x Genomics](https://www.10xgenomics.com/datasets) |
| Spatial PDAC | Slide-seq + Visium | Pancreatic ductal adenocarcinoma | Spatially resolved tumor-stroma interactions | [Moncada et al. 2020](https://doi.org/10.1038/s41587-019-0392-8) |
| Glioblastoma spatial | Visium + scRNA-seq | Glioblastoma | Spatial organization of tumor cell states | [Ravi et al. 2022](https://doi.org/10.1158/2159-8290.CD-21-1086) |
| Alzheimer's spatial | Visium + MERFISH | Alzheimer's disease (human brain) | Spatial patterns of neurodegeneration | Multiple studies |
| CRC spatial | CODEX + scRNA-seq | Colorectal cancer | Immune microenvironment architecture | [Pelka et al. 2021](https://doi.org/10.1016/j.cell.2021.08.003) |
| COVID-19 lung | Visium + scRNA-seq | COVID-19 lung tissue | Spatial immune response in SARS-CoV-2 infection | [Melms et al. 2021](https://doi.org/10.1038/s41586-021-03570-8) |
| Melanoma spatial | Visium + scRNA-seq | Melanoma | Spatial organization of immune evasion | [Biermann et al. 2022](https://doi.org/10.1016/j.cell.2022.01.022) |

## Data repositories

| Repository | Content | Format | Access |
|---|---|---|---|
| [SODB](https://gene.ai.tencent.com/SpatialOmics/) | 30+ spatial datasets, standardized | AnnData/SpatialData | Free, web interface |
| [STOmicsDB](https://db.cngb.org/stomics/) | Stereo-seq and other spatial datasets | Varies | Free registration |
| [SpatialDB](https://www.spatialomics.org/SpatialDB/) | Curated spatial transcriptomics datasets | Varies | Free |
| [10x Genomics Datasets](https://www.10xgenomics.com/datasets) | Visium, Xenium, Visium HD demo datasets | Vendor format + H5AD | Free |
| [CZ CELLxGENE](https://cellxgene.cziscience.com/) | scRNA-seq + growing spatial collection | AnnData | Free |
| [Broad Single Cell Portal](https://singlecell.broadinstitute.org/) | scRNA-seq + spatial (Slide-seq) | Varies | Free registration |
| [GEO](https://www.ncbi.nlm.nih.gov/geo/) | Raw data for most published spatial studies | Raw counts + coordinates | Free |
| [Zenodo](https://zenodo.org/) | Processed datasets from publications | Varies | Free |
| [SpatialData (scverse)](https://spatialdata.scverse.org/) | Framework for spatial data + example datasets | SpatialData/Zarr | Free, Python API |

## Dataset selection guide

| Analysis need | Recommended dataset | Why |
|---|---|---|
| Benchmarking spatial clustering | DLPFC (Visium) | Most widely used, enables comparison with published results |
| Benchmarking deconvolution | DLPFC + matching scRNA-seq | Well-annotated layers provide pseudo-ground truth |
| Testing segmentation methods | CosMx NSCLC or Xenium breast | High-quality imaging + transcript coordinates |
| Multi-resolution analysis | Mouse brain MERFISH + Visium | Same tissue, different technologies |
| Testing at scale (>1M cells) | Stereo-seq mouse embryo | Largest publicly available spatial dataset |
| Tumor microenvironment | Xenium breast or CRC CODEX | Well-characterized immune landscapes |
| Reference atlas for mapping | Allen Brain Cell Atlas | Gold-standard brain reference |
| Protein-level spatial analysis | CODEX mouse spleen or MIBI-TOF breast | Well-characterized protein panels |
| Multi-modal (RNA + protein) | HuBMAP datasets | Multiple modalities, same tissue |
| Quick exploration / tutorials | 10x Genomics demo datasets | Small, well-documented, easy to download |

## Practical notes

!!! tip "Starting with a benchmark? Use the DLPFC."
    Despite its [limitations](../benchmarks/benchmark-synthesis.md), the DLPFC dataset is the easiest way to compare results with published methods. Use it for initial validation, then test on more challenging datasets.

!!! note "Data formats"
    Most datasets are available in or convertible to AnnData (H5AD) format, the standard for the scverse ecosystem (Scanpy, Squidpy, SpatialData). The SpatialData framework provides loaders for many common formats and wraps spatial coordinates, images, and expression data into a unified structure.

**Data size considerations.** Spatial datasets can be very large. Stereo-seq and MERFISH whole-brain datasets exceed 100 GB. Plan storage and memory requirements before downloading. Many repositories offer subsets or downsampled versions for initial exploration.

## Further reading

- [Benchmark Synthesis](../benchmarks/benchmark-synthesis.md) for how these datasets are used in benchmarks
- [Technologies Overview](../technologies/overview.md) for understanding which technology produced each dataset
- [The Pipeline Problem](../synthesis/pipeline-problem.md) for challenges in processing these datasets
