# Glossary

Key terms used throughout this site, listed alphabetically.

---

**AnnData**
A data structure (Python, part of the scverse ecosystem) for storing annotated matrices. The standard container for single-cell and spatial omics data in Python workflows. Stores expression matrices, cell/gene metadata, embeddings, and spatial coordinates in a single `.h5ad` file.

**Barcode**
A short nucleotide sequence used to tag molecules or locations. In spatial transcriptomics, barcodes are attached to specific spatial positions on a capture array (Visium, Slide-seq) to link transcripts to their tissue coordinates.

**Cell segmentation**
The process of assigning pixels or transcripts to individual cells in imaging-based spatial data. A critical preprocessing step that determines cell boundaries and directly affects all downstream analyses. See [Segmentation Benchmarks](benchmarks/segmentation-benchmarks.md).

**Cell2location**
A Bayesian deconvolution method that estimates cell-type abundance at each spatial location using a reference single-cell RNA-seq dataset. Consistently ranked as the most accurate deconvolution method in benchmarks. See [Deconvolution Benchmarks](benchmarks/deconvolution-benchmarks.md).

**CCC (Cell-Cell Communication)**
Analysis of signaling interactions between cells, typically inferred from ligand-receptor co-expression patterns. Spatial CCC methods (COMMOT, SpatialDM, MISTy) additionally require that interacting cells be spatially proximate.

**CODEX**
Co-Detection by Indexing. A cyclic immunofluorescence technology for spatial proteomics, now commercialized as PhenoCycler by Akoya Biosciences. Measures 40--100 proteins at single-cell resolution by iterative antibody staining and imaging.

**CosMx**
CosMx Spatial Molecular Imager. A single-molecule imaging platform developed by NanoString (now Bruker) for spatial transcriptomics at subcellular resolution. Measures up to 6,000 genes using fluorescent probe hybridization.

**DDA / DIA**
Data-Dependent Acquisition / Data-Independent Acquisition. Mass spectrometry acquisition strategies used in spatial proteomics (MALDI) and metabolomics. DIA provides more comprehensive coverage; DDA provides higher sensitivity for targeted analytes.

**Deconvolution**
The computational process of estimating cell-type proportions within a spatial spot or region that contains multiple cells. Necessary for technologies like Visium where each spot captures RNA from 5--20 cells. See [Deconvolution Benchmarks](benchmarks/deconvolution-benchmarks.md).

**DLPFC**
Dorsolateral prefrontal cortex. A brain region used as the most common benchmark dataset for spatial domain detection methods (Maynard et al. 2021). Contains well-defined cortical layers (L1--L6 plus white matter). See [Benchmark Synthesis](benchmarks/benchmark-synthesis.md) for caveats about over-reliance on this dataset.

**FISH**
Fluorescence In Situ Hybridization. A molecular biology technique that uses fluorescent probes to detect specific nucleic acid sequences in tissue. The basis for spatial transcriptomics technologies like MERFISH, seqFISH, and osmFISH.

**Foundation model**
A large-scale machine learning model pre-trained on massive datasets that can be fine-tuned for specific tasks. In spatial omics, foundation models (e.g., scGPT, Geneformer, scBERT) trained on millions of cells are being adapted for spatial tasks like cell-type annotation and imputation.

**GeoMx**
GeoMx Digital Spatial Profiler. A region-of-interest (ROI) spatial profiling platform developed by NanoString (now Bruker). Measures gene expression or protein levels in user-selected tissue regions rather than at single-cell resolution.

**GNN (Graph Neural Network)**
A class of deep learning models that operate on graph-structured data. In spatial omics, GNNs construct graphs where cells/spots are nodes and spatial proximity defines edges, enabling methods (GraphST, STAGATE) to learn spatially-aware representations.

**H&E**
Hematoxylin and Eosin. A standard histological stain that highlights tissue morphology. Many spatial transcriptomics platforms (Visium, Visium HD) capture H&E images alongside molecular data, enabling integration of morphological and molecular information.

**IMC (Imaging Mass Cytometry)**
A spatial proteomics technology that uses metal-tagged antibodies and mass spectrometry to image 40+ proteins simultaneously at ~1 micron resolution. Commercialized by Standard BioTools (formerly Fluidigm).

**In situ**
Latin for "in its place." Refers to measurements made within the original tissue context, preserving spatial relationships. Spatial omics technologies measure molecules in situ rather than after tissue dissociation.

**ISS (In Situ Sequencing)**
A spatial transcriptomics approach that sequences RNA molecules directly within tissue using padlock probes and rolling circle amplification. Provides single-molecule resolution but with lower gene throughput than MERFISH.

**Ligand-receptor**
A pair of molecules (one secreted or surface-bound, one on the receiving cell) that mediate cell-cell signaling. Ligand-receptor databases (CellPhoneDB, CellChat) form the basis of most cell-cell communication analysis methods.

**MALDI**
Matrix-Assisted Laser Desorption/Ionization. A mass spectrometry technique used for spatial metabolomics and proteomics. Provides untargeted molecular profiling at 5--50 micron resolution without requiring antibodies or probes.

**MERFISH**
Multiplexed Error-Robust Fluorescence In Situ Hybridization. A single-molecule imaging technology that measures hundreds to thousands of genes at subcellular resolution. Commercialized as the MERSCOPE platform by Vizgen (now 10x Genomics).

**Niche**
A local cellular neighborhood in tissue, defined by the composition and spatial arrangement of cell types. Niche detection identifies recurring spatial patterns of cell-type co-localization that may reflect functional tissue units.

**Optimal transport**
A mathematical framework for finding the most efficient mapping between two distributions. Used in spatial omics for mapping single-cell reference data to spatial locations (Tangram, novoSpaRc) and modeling intercellular communication (COMMOT).

**PhenoCycler**
The commercial name for the CODEX technology, sold by Akoya Biosciences. A cyclic immunofluorescence platform for high-plex spatial proteomics.

**Resolution**
The spatial granularity of a measurement. In spatial omics, resolution ranges from ~55 microns (original Visium spots, containing 5--20 cells) to ~100 nanometers (MERFISH, Xenium, where individual transcripts are localized). Higher resolution provides more spatial detail but generates larger datasets.

**scRNA-seq**
Single-cell RNA sequencing. The dissociated single-cell transcriptomics approach that measures gene expression in individual cells without preserving spatial information. Frequently used as a reference for spatial deconvolution and cell-type annotation.

**Spatial domain**
A contiguous region of tissue with a coherent gene expression program. Spatial domain detection (also called spatial clustering) identifies these regions by combining transcriptomic similarity with spatial contiguity. See [Clustering Benchmarks](benchmarks/clustering-benchmarks.md).

**Spot**
A discrete capture location on a spatial array. In Visium, spots are 55-micron circles arranged in a hexagonal grid, each capturing RNA from 5--20 cells. In Visium HD, bins are 2 microns. The term "spot" is sometimes used loosely for any spatial measurement unit.

**SVG (Spatially Variable Gene)**
A gene whose expression pattern shows spatial structure beyond random expectation. SVGs mark tissue domains, gradients, or localized cell states. Detected using spatial autocorrelation methods (nnSVG, SPARK-X, SpatialDE). See [SVG Benchmarks](benchmarks/svg-benchmarks.md).

**Visium**
A sequencing-based spatial transcriptomics platform by 10x Genomics. Captures mRNA at ~3,500 spots (55-micron diameter) across a tissue section with genome-wide coverage. The most widely used spatial transcriptomics technology and the most common benchmark platform.

**Visium HD**
The high-resolution successor to Visium, capturing at 2-micron bins. Provides near single-cell resolution with genome-wide coverage but generates much larger datasets. See [The Technology-Analysis Gap](synthesis/technology-analysis-gap.md) for analysis challenges.

**Xenium**
An in situ imaging platform by 10x Genomics that measures targeted gene panels (up to ~5,000 genes) at subcellular resolution. Provides single-cell and subcellular transcript localization with high sensitivity. Competes with CosMx and MERSCOPE in the imaging-based spatial transcriptomics market.
