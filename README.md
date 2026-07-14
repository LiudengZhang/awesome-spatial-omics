# Awesome Spatial Omics [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Spatial omics has more tools than any analyst can evaluate. This list organizes them by the question you're asking and the technology you're using.

Spatial transcriptomics, proteomics, and metabolomics generate data that is fundamentally different from dissociated single-cell experiments: every measurement comes with coordinates. This creates an entire class of analytical problems -- segmentation, niche detection, spatial variable gene testing, 3D alignment -- that standard scRNA-seq pipelines were never designed for. The field now has hundreds of tools, and choosing the right one depends on the technology, the biological question, and the data scale. This list provides an opinionated, structured catalog to help analysts navigate that landscape.

## Contents

- [Surveys and Reviews](#surveys-and-reviews)
- [Technologies and Platforms](#technologies-and-platforms)
- [Pre-processing and QC](#pre-processing-and-qc)
- [Cell Segmentation](#cell-segmentation)
- [Spatially Variable Genes](#spatially-variable-genes)
- [Spatial Domains and Tissue Architecture](#spatial-domains-and-tissue-architecture)
- [Deconvolution and Cell Type Mapping](#deconvolution-and-cell-type-mapping)
- [Cell-Cell Communication](#cell-cell-communication)
- [Spatial Niches and Microenvironments](#spatial-niches-and-microenvironments)
- [Cell Annotation and Mapping](#cell-annotation-and-mapping)
- [Spatial Differential Expression](#spatial-differential-expression)
- [Subcellular Analysis](#subcellular-analysis)
- [Spatial Trajectories](#spatial-trajectories)
- [Multi-modal Integration](#multi-modal-integration)
- [Foundation Models](#foundation-models)
- [3D Reconstruction and Alignment](#3d-reconstruction-and-alignment)
- [Visualization and Interactive Tools](#visualization-and-interactive-tools)
- [Frameworks and Infrastructure](#frameworks-and-infrastructure)
- [Benchmarks](#benchmarks)
- [Datasets and Databases](#datasets-and-databases)
- [Companion Site](#companion-site)
- [Contributing](#contributing)

## Surveys and Reviews

Comprehensive reviews and perspectives covering spatial omics technologies, methods, and applications.

- [Museum of Spatial Transcriptomics](https://doi.org/10.1038/s41592-022-01409-2) - The definitive historical and technical survey of spatial transcriptomics methods, organized chronologically from 1969 to present (Nature Methods, 2022).
- [Spatially resolved transcriptomics in neuroscience](https://doi.org/10.1038/s41576-021-00370-8) - Thorough review of spatial transcriptomics technologies with a focus on neuroscience applications and practical considerations (Nature Reviews Genetics, 2021).
- [Method of the Year: Spatially Resolved Transcriptomics](https://doi.org/10.1038/s41592-020-01033-y) - Nature Methods editorial recognizing spatial transcriptomics as the 2020 method of the year, providing a concise overview of the field's trajectory (Nature Methods, 2021).
- [Spatial components of molecular tissue biology](https://doi.org/10.1038/s41587-021-01182-1) - Framework for integrating single-cell and spatial data in the context of tissue biology, with practical guidance on computational workflows (Nature Biotechnology, 2022).
- [Computational challenges and opportunities in spatially resolved transcriptomic data analysis](https://doi.org/10.1186/s13059-022-02653-7) - Systematic categorization of computational methods for spatial transcriptomics organized by analytical task (Genome Biology, 2022).
- [Exploring tissue architecture using spatial transcriptomics](https://doi.org/10.1038/s41586-021-03634-9) - High-level perspective on how spatial transcriptomics reveals tissue organization principles across organs (Nature, 2021).
- [Methods and applications for single-cell and spatial multi-omics](https://doi.org/10.1038/s41576-023-00580-2) - Review covering emerging multi-omic spatial technologies that measure transcriptomes alongside epigenomes or proteomes (Nature Reviews Genetics, 2023).
- [Spatial landscapes of cancers: insights and opportunities](https://doi.org/10.1038/s41571-024-00886-8) - Focused review on spatial omics applications in cancer, covering tumor microenvironment mapping and clinical implications (Nature Reviews Clinical Oncology, 2024).
- [The dawn of spatial omics](https://doi.org/10.1126/science.abq4964) - Broad perspective on the convergence of spatial transcriptomics, proteomics, and metabolomics into a unified spatial omics field (Science, 2023).
- [Single-cell best practices: spatial transcriptomics](https://www.sc-best-practices.org/spatial/index.html) - Practical tutorial-style guide covering spatial data analysis workflows from raw data to biological interpretation, part of the sc-best-practices book (Online, 2023).

## Technologies and Platforms

The spatial omics landscape spans sequencing-based methods (which capture full transcriptomes but at variable resolution), imaging-based methods (which detect pre-selected genes at subcellular resolution), and protein/metabolite platforms.

### Sequencing-Based

- [Visium](https://www.10xgenomics.com/products/spatial-gene-expression) - The most widely adopted spatial transcriptomics platform, capturing full transcriptomes at 55-micron spot resolution with well-established computational ecosystem (10x Genomics, 2020).
- [Visium HD](https://www.10xgenomics.com/products/visium-hd) - Successor to Visium with 2-micron resolution bins, approaching single-cell resolution but generating substantially larger datasets that stress standard pipelines (10x Genomics, 2024).
- [Slide-seq/V2](https://github.com/MacoskoLab/Slide-seq) - Bead-based spatial transcriptomics achieving 10-micron resolution on fresh-frozen tissue, with V2 improving capture efficiency roughly tenfold (Science, 2019/2021).
- [Stereo-seq](https://doi.org/10.1016/j.cell.2022.04.003) - DNA nanoball-based technology offering subcellular resolution (~500 nm) across centimeter-scale tissues, generating the largest spatial datasets currently available (Cell, 2022).
- [Open-ST](https://github.com/rajewsky-lab/openst) - Open-source spatial transcriptomics protocol achieving subcellular resolution with straightforward library preparation, designed to be accessible to any lab (Cell, 2024).
- [Seq-Scope](https://doi.org/10.1016/j.cell.2021.05.010) - Repurposes Illumina sequencing flow cells for spatial transcriptomics at subcellular resolution, offering a cost-effective alternative to commercial platforms (Cell, 2021).
- [DBiT-seq](https://doi.org/10.1016/j.cell.2020.09.026) - Deterministic barcoding in tissue using microfluidic channels, enabling spatial multi-omics on FFPE tissue at 10-micron pixel size (Cell, 2020).
- [HDST](https://doi.org/10.1038/s41592-019-0548-y) - High-definition spatial transcriptomics using 2-micron beads, an early high-resolution method that demonstrated feasibility but had limited capture efficiency (Nature Methods, 2019).
- [Pixel-seq](https://doi.org/10.1016/j.cell.2022.01.015) - Polony-based spatial barcoding achieving subcellular resolution with improved transcript capture compared to earlier bead-based approaches (Cell, 2022).

### Imaging-Based

- [MERFISH](https://github.com/ZhuangLab/MERlin) - Multiplexed error-robust FISH detecting hundreds to thousands of genes at single-molecule resolution, the most mature imaging-based spatial method (Science, 2015).
- [seqFISH/seqFISH+](https://doi.org/10.1038/s41586-019-1049-y) - Sequential barcoding FISH capable of profiling over 10,000 genes at subcellular resolution, though imaging time scales linearly with gene panel size (Nature, 2019).
- [STARmap PLUS](https://doi.org/10.1038/s41587-022-01251-z) - Hydrogel-tissue chemistry approach for intact-tissue spatial transcriptomics, enabling 3D spatial mapping of thousands of genes in thick tissue sections (Nature Biotechnology, 2022).
- [osmFISH](https://doi.org/10.1038/s41592-018-0175-z) - Single-molecule FISH with sequential rounds of hybridization, offering straightforward probe design but limited to modest gene panels (Nature Methods, 2018).
- [In Situ Sequencing (ISS)](https://doi.org/10.1038/nmeth.2563) - Padlock probe-based in situ sequencing that reads out barcodes directly in tissue, one of the earliest methods for spatially resolved gene expression (Nature Methods, 2013).
- [ExSeq](https://doi.org/10.1126/science.aax2656) - Expansion sequencing combining tissue expansion with in situ sequencing for nanoscale-resolution transcriptomics, technically demanding but uniquely high resolution (Science, 2021).
- [EASI-FISH](https://doi.org/10.1016/j.cell.2021.11.024) - Expansion-assisted iterative FISH designed for thick tissue sections, enabling intact 3D spatial transcriptomics at the cost of longer processing times (Cell, 2021).

### Commercial Platforms

- [Xenium](https://www.10xgenomics.com/platforms/xenium) - 10x Genomics' imaging-based platform detecting up to ~5,000 genes at subcellular resolution, with growing adoption and ecosystem integration via SpatialData (10x Genomics, 2023).
- [MERSCOPE](https://vizgen.com/products/merscope/) - Vizgen's commercialization of MERFISH providing turnkey imaging-based spatial transcriptomics for hundreds of genes with automated image acquisition (Vizgen, 2021).
- [CosMx SMI](https://nanostring.com/products/cosmx-spatial-molecular-imager/) - NanoString's spatial molecular imager achieving single-molecule sensitivity with both RNA and protein detection on FFPE tissue (NanoString, 2022).
- [GeoMx DSP](https://nanostring.com/products/geomx-digital-spatial-profiler/) - Region-of-interest profiling platform that sacrifices single-cell resolution for robust performance on FFPE clinical samples, measuring RNA or protein from user-selected areas (NanoString, 2020).
- [PhenoCycler](https://www.akoyabio.com/phenocycler/) - Akoya Biosciences platform (formerly CODEX) enabling high-plex protein imaging through iterative antibody staining on standard microscopes (Akoya Biosciences, 2022).
- [Resolve Molecular Cartography](https://resolvebiosciences.com/) - Combinatorial FISH platform detecting up to 100 genes at single-molecule resolution with automated probe design (Resolve Biosciences, 2021).
- [STOmics](https://www.stomics.tech/) - BGI Genomics' Stereo-seq-based commercial platform offering subcellular-resolution spatial transcriptomics at competitive per-sample cost (BGI, 2022).
- [Curio Seeker](https://curiobioscience.com/curio-seeker/) - Commercial Slide-seq implementation providing 10-micron resolution spatial transcriptomics with simplified sample preparation (Curio Bioscience, 2022).

### Spatial Proteomics

- [CODEX](https://doi.org/10.1016/j.cell.2018.07.010) - Co-detection by indexing using DNA-barcoded antibodies for multiplexed protein imaging of 40+ markers, now commercialized as PhenoCycler (Cell, 2018).
- [MIBI](https://doi.org/10.1038/s41591-019-0436-8) - Multiplexed ion beam imaging using metal-tagged antibodies with mass spectrometry readout, achieving 500 nm resolution for 40+ protein targets simultaneously (Nature Medicine, 2019).
- [Imaging Mass Cytometry (IMC/Hyperion)](https://doi.org/10.1038/s41592-018-0072-5) - Laser ablation coupled to mass cytometry for 40+ marker protein imaging, slower throughput than fluorescence-based methods but minimal spectral overlap (Nature Methods, 2018).
- [t-CyCIF](https://doi.org/10.7554/eLife.31657) - Tissue-based cyclic immunofluorescence enabling 20-60 marker protein imaging on standard microscopes using iterative antibody staining and bleaching (eLife, 2018).
- [4i](https://doi.org/10.1126/science.aar7042) - Iterative indirect immunofluorescence imaging for multiplexed protein profiling with antibody elution between rounds, achieving high-plex imaging without specialized hardware (Science, 2018).
- [mIHC/mIF](https://doi.org/10.1038/s41374-020-0445-y) - Multiplex immunohistochemistry and immunofluorescence using tyramide signal amplification, the most clinically established spatial protein method with 7-10 marker panels (Laboratory Investigation, 2020).

### Spatial Multi-Omics

- [Spatial ATAC-seq](https://doi.org/10.1038/s41586-023-06795-1) - Spatial chromatin accessibility profiling combining ATAC-seq with spatial barcoding, enabling epigenomic mapping at near-single-cell resolution in tissue (Nature, 2023).
- [Spatial CUT&Tag](https://doi.org/10.1126/science.abg7216) - Spatial epigenomic profiling of histone modifications using CUT&Tag chemistry with spatial barcoding, revealing tissue-level chromatin state patterns (Science, 2022).
- [Slide-tags](https://doi.org/10.1038/s41586-024-07697-2) - Nuclear tagging approach that assigns spatial barcodes to nuclei for joint spatial multi-omic profiling including RNA, ATAC, and protein (Nature, 2024).
- [SPOTS](https://doi.org/10.1038/s41467-023-36066-2) - Spatial proteomics and transcriptomics sequencing combining Visium with protein detection for joint spatial measurement of RNA and surface proteins (Nature Communications, 2023).
- [SM-Omics](https://doi.org/10.1038/s41467-023-43256-5) - Spatial multi-omics platform co-profiling transcriptome and surface proteins on the same tissue section with Visium-compatible workflow (Nature Communications, 2023).
- [DBiT-seq (multi-omic)](https://doi.org/10.1016/j.cell.2020.09.026) - Multi-omic extension of DBiT-seq co-profiling chromatin accessibility and gene expression or protein and RNA in the same tissue section (Cell, 2020).

### Spatial Metabolomics

- [MALDI-MSI](https://doi.org/10.1038/s41592-019-0344-8) - Matrix-assisted laser desorption/ionization mass spectrometry imaging, the most established spatial metabolomics technique with 5-50 micron resolution and broad metabolite coverage (Nature Methods, 2019).
- [DESI-MSI](https://doi.org/10.1126/science.1180449) - Desorption electrospray ionization mass spectrometry imaging enabling ambient-condition metabolite mapping without matrix application, widely used for clinical tissue analysis (Science, 2009).
- [SpaceM](https://github.com/metaspace2020/SpaceM) - Integrates MALDI-MSI with microscopy to link single-cell metabolomic profiles to morphological phenotypes, bridging metabolomics and imaging (Nature Methods, 2022).
- [NanoSIMS](https://doi.org/10.1038/s41586-020-2196-0) - Secondary ion mass spectrometry at nanometer resolution for elemental and isotopic imaging, enabling metabolic tracing at subcellular scale but with very low throughput (Nature, 2020).

## Pre-processing and QC

Quality control and normalization tools addressing spatial-specific artifacts such as tissue damage, ambient RNA contamination, and spatially varying capture efficiency.

- [SpotClean](https://github.com/zijianni/SpotClean) - Statistical model for decontaminating spatial transcriptomics data by estimating and removing ambient RNA diffusion artifacts (NAR Genomics and Bioinformatics, 2022).
- [SpotSweeper](https://github.com/MicTott/SpotSweeper) - Spatially-aware quality control that identifies low-quality spots using local neighborhood statistics rather than global thresholds (Bioinformatics, 2024).
- [SpaNorm](https://github.com/phipsonlab/SpaNorm) - Spatial normalization method that accounts for position-dependent variation in library size and gene detection across the tissue (Genome Biology, 2024).
- [Sprod](https://github.com/az7jh2/sprod) - Denoising method that leverages spatial and image-derived similarity to impute dropout events in spot-based spatial transcriptomics (Nature Methods, 2022).
- [DenoIST](https://github.com/MossWang/DenoIST) - Deep learning denoiser for imaging-based spatial transcriptomics that corrects transcript detection errors using spatial context (Nature Communications, 2024).
- [sopa](https://github.com/gustaveroussy/sopa) - Scalable pipeline for spatial omics that handles pre-processing through segmentation with technology-agnostic design and SpatialData integration (Nature Communications, 2024).
- [cellAdmix](https://github.com/Cellverse/cellAdmix) - Ambient RNA estimation and removal for spatial data that models contamination as a mixture of cell-type-specific expression profiles (Nature Biotechnology, 2024).
- [MisTIC](https://github.com/Lagergren-Lab/MisTIC) - Quality metric for spatial transcriptomics that quantifies the mismatch between spatial and expression-based neighborhoods (Bioinformatics, 2023).
- [ResolVI](https://github.com/scverse/resolvi) - Variational inference framework for denoising imaging-based spatial transcriptomics, designed to handle the specific noise characteristics of MERFISH and Xenium data (bioRxiv, 2024).
- [SPLIT](https://github.com/Uauy-Lab/SPLIT) - Spatially-aware normalization for spot-level spatial transcriptomics that corrects for local technical variation while preserving biological signal (Bioinformatics, 2023).
- [TISSUE](https://github.com/Elipiriti/TISSUE) - Tissue quality assessment tool that evaluates spatial transcriptomics data integrity using tissue morphology and expression concordance (Nature Methods, 2024).

## Cell Segmentation

Accurate cell boundary delineation is critical for imaging-based spatial transcriptomics. Methods range from deep learning on morphology images to transcript-aware probabilistic models.

- [Cellpose 2](https://github.com/MouseLand/cellpose) - Generalist deep learning segmentation model with human-in-the-loop fine-tuning, strong out-of-the-box performance on diverse tissue types (Nature Methods, 2022).
- [StarDist](https://github.com/stardist/stardist) - Star-convex polygon-based segmentation optimized for densely packed nuclei, faster than Cellpose but less flexible on elongated or irregular cell shapes (MICCAI, 2018).
- [Baysor](https://github.com/kharchenkolab/Baysor) - Transcript-aware Bayesian segmentation that does not require pre-existing nuclear stains, particularly useful when morphology images are unavailable or poor quality (Nature Biotechnology, 2022).
- [BIDCell](https://github.com/SydneyBioX/BIDCell) - Deep learning segmentation integrating both cell morphology and transcript density, aiming to outperform morphology-only or transcript-only approaches (Nature Communications, 2024).
- [Mesmer/DeepCell](https://github.com/vanvalenlab/deepcell-tf) - Whole-cell segmentation using nuclear and membrane markers trained on the TissueNet dataset of over one million annotated cells (Nature Biotechnology, 2022).
- [CellSAM](https://github.com/vanvalenlab/cellSAM) - Adaptation of Segment Anything Model (SAM) for cell segmentation, leveraging foundation model capabilities with cell-specific fine-tuning (bioRxiv, 2024).
- [Proseg](https://github.com/dcjones/proseg) - Probabilistic transcript assignment to cells that jointly performs segmentation and ambient RNA correction, designed for high-density imaging data (Nature Methods, 2024).
- [ComSeg](https://github.com/fish-quant/ComSeg) - Community detection-based segmentation using transcript co-expression graphs, effective for tissues where cells are not easily separated by morphology alone (bioRxiv, 2024).
- [Segger](https://github.com/EliHei2/segger_dev) - Graph neural network-based segmentation for spatial transcriptomics that operates directly on transcript point clouds (bioRxiv, 2024).
- [Bering](https://github.com/jia-wu-lab/Bering) - Graph neural network segmentation for FISH-based spatial transcriptomics that incorporates both spatial coordinates and gene identity (Nature Communications, 2024).
- [ClusterMap](https://github.com/wanglab-broad/ClusterMap) - Unsupervised spatial clustering of transcripts into cells without requiring staining images, using density-based spatial grouping (Nature Communications, 2021).

### Segmentation-Free Methods

- [SSAM](https://github.com/eilslabs/ssam) - Segmentation-free spatial analysis using kernel density estimation on transcript locations, bypassing segmentation errors entirely at the cost of single-cell resolution (Nature Communications, 2021).
- [Points2Regions](https://github.com/wahlby-lab/Points2Regions) - Segmentation-free regional analysis that aggregates transcripts into coherent tissue domains using spatial point pattern statistics (bioRxiv, 2023).

### Visium HD-Specific Segmentation

- [Bin2Cell](https://doi.org/10.1038/s41586-024-08416-7) - Aggregates Visium HD 2-micron bins into cell-level profiles using nuclear stain images, the recommended starting point for Visium HD analysis (Nature, 2024).
- [ENACT](https://github.com/Beibarys-Daulet/ENACT) - Deep learning framework for Visium HD that jointly performs cell segmentation and bin-to-cell assignment using tissue morphology (bioRxiv, 2024).
- [STHD](https://github.com/gerstung-lab/STHD) - Statistical method for cell type deconvolution directly from Visium HD bins without prior segmentation, treating each bin as a mixture (bioRxiv, 2024).

## Spatially Variable Genes

Methods for identifying genes whose expression varies across tissue space in patterns that cannot be explained by random noise.

- [SpatialDE 2](https://github.com/PMBio/SpatialDE) - Gaussian process-based spatially variable gene detection with automatic expression histology for grouping co-varying genes, the original and most widely cited SVG method (Nature Methods, 2018/2021).
- [SPARK-X](https://github.com/xzhoulab/SPARK) - Nonparametric SVG test using covariance testing that scales to large datasets orders of magnitude faster than Gaussian process methods (Genome Biology, 2021).
- [nnSVG](https://github.com/lmweber/nnSVG) - Nearest-neighbor Gaussian process SVG method that scales linearly with spot count, enabling SVG testing on large Visium and Slide-seq datasets (Nature Communications, 2023).
- [SOMDE](https://github.com/WhyLIM/SOMDE) - Self-organizing map-based SVG identification that reduces computational cost by clustering spatial locations before testing (Bioinformatics, 2021).
- [Hotspot](https://github.com/YosefLab/Hotspot) - Identifies informative genes and gene modules using local autocorrelation statistics, useful for both spatial and graph-based neighbor definitions (Cell Systems, 2021).
- [SpatialCorr](https://github.com/mbernste/SpatialCorr) - Tests for spatially varying gene-gene correlations rather than just expression levels, detecting coordination patterns invisible to standard SVG tests (bioRxiv, 2022).
- [PROST](https://github.com/tangkang/PROST) - Quantifies spatial expression patterns using a tensor decomposition framework that captures both smooth gradients and localized domains (Nature Communications, 2024).
- [trendsceek](https://github.com/edsgard/trendsceek) - Mark-based spatial point process method for SVG detection, one of the earliest approaches but slower than modern alternatives on large datasets (Nature Methods, 2018).

## Spatial Domains and Tissue Architecture

Methods for identifying spatially coherent tissue regions with distinct molecular profiles, analogous to clustering but incorporating spatial coordinates.

- [BANKSY](https://github.com/prabhakarlab/Banksy_py) - Augments cell features with neighborhood expression statistics before clustering, achieving strong domain detection with a simple and scalable design (Nature Genetics, 2024).
- [SpaGCN](https://github.com/jianhuupenn/SpaGCN) - Graph convolutional network integrating expression, spatial location, and histology for spatial domain identification (Nature Methods, 2021).
- [STAGATE](https://github.com/zhanglabtools/STAGATE) - Graph attention autoencoder for spatial clustering that adaptively learns neighborhood relationships, among the top-performing methods in benchmark studies (Nature Communications, 2022).
- [BayesSpace](https://github.com/edward130603/BayesSpace) - Bayesian clustering that explicitly models spatial neighborhood structure through a Potts prior, offering principled resolution enhancement for Visium data (Nature Biotechnology, 2021).
- [GraphST](https://github.com/JinmiaoChenLab/GraphST) - Self-supervised graph contrastive learning for spatial transcriptomics clustering and integration, scales to large datasets (Nature Communications, 2023).
- [BASS](https://github.com/zhengli09/BASS) - Multi-scale Bayesian model that simultaneously identifies cell types and tissue domains, jointly modeling single-cell and spatial information (Nature Methods, 2022).
- [SpaceFlow](https://github.com/hongleir/SpaceFlow) - Deep graph network using spatially regularized latent representations with pseudo-spatial-temporal dynamics for domain identification (Nature Communications, 2022).
- [SEDR](https://github.com/JinmiaoChenLab/SEDR) - Deep autoencoder with variational graph autoencoder for learning spatial embedding of transcriptomics data (Genome Research, 2022).
- [GASTON](https://github.com/raphael-group/GASTON) - Identifies spatial domains as regions of distinct gene expression programs using a topic model with spatial smoothness constraints (Nature Methods, 2024).
- [conST](https://github.com/ys-zong/conST) - Contrastive learning framework that integrates gene expression, spatial location, and morphology for spatial clustering and domain annotation (Nature Communications, 2023).
- [smoothclust](https://github.com/lmweber/smoothclust) - Simple spatial smoothing before standard clustering, demonstrating that complex graph neural network architectures are not always necessary for domain detection (bioRxiv, 2024).
- [Vesalius](https://github.com/WonLab-CS/Vesalius) - Image analysis-inspired approach that treats spatial transcriptomics as image data, using image processing techniques for territory identification (Nature Communications, 2024).

## Deconvolution and Cell Type Mapping

Methods for estimating cell type composition within spatial spots or for mapping dissociated single-cell data back to spatial coordinates.

- [cell2location](https://github.com/BayraktarLab/cell2location) - Bayesian model that maps single-cell reference signatures to spatial locations with uncertainty quantification, among the most accurate in benchmark comparisons (Nature Biotechnology, 2022).
- [RCTD/spacexr](https://github.com/dmcable/spacexr) - Robust cell type decomposition using supervised regression with platform effect correction, also provides C-SIDE for spatial differential expression (Nature Biotechnology, 2022).
- [CARD](https://github.com/YingMa0107/CARD) - Reference-based deconvolution using conditional autoregressive modeling that borrows information from spatial neighbors (Nature Biotechnology, 2022).
- [Tangram](https://github.com/broadinstitute/Tangram) - Alignment-based mapping of single-cell data to spatial data using optimal transport, flexible across technologies and gene panel sizes (Nature Methods, 2021).
- [STdeconvolve](https://github.com/JEFworks-Lab/STdeconvolve) - Reference-free deconvolution using latent Dirichlet allocation, useful when a matching single-cell reference is unavailable (Nature Communications, 2022).
- [SPOTlight](https://github.com/MarcElosworthy/SPOTlight) - NMF-based deconvolution that is fast and lightweight but less accurate than probabilistic methods on complex tissues (Nucleic Acids Research, 2021).
- [CytoSPACE](https://github.com/digitalcytometry/cytospace) - Single-cell resolution mapping that assigns individual cells to spatial locations, going beyond proportion estimation (Nature Biotechnology, 2023).
- [DestVI](https://github.com/scverse/scvi-tools) - Deep generative model for multi-resolution deconvolution built within the scvi-tools framework, jointly learning cell type proportions and cell state variation (Nature Biotechnology, 2022).
- [spacedeconv](https://github.com/omnideconv/spacedeconv) - Unified interface wrapping multiple deconvolution methods with consistent input/output, useful for method comparison but adds a dependency layer (Bioinformatics, 2024).
- [bulk2space](https://github.com/ZJUFanLab/bulk2space) - Maps bulk RNA-seq to spatial coordinates using a deep learning autoencoder, useful for legacy datasets without spatial resolution (Nature Communications, 2023).
- [TACCO](https://github.com/simonwm/tacco) - Transfer of annotations to cells and their combinations in other spaces, a flexible framework for spatial mapping and composition analysis (Nature Biotechnology, 2023).
- [InSituType](https://github.com/Nanostring-Biostats/InSituType) - Probabilistic cell typing designed for NanoString CosMx data that jointly clusters and annotates cells using likelihood-based assignment (Nature Biotechnology, 2022).

## Cell-Cell Communication

Methods for inferring intercellular signaling from spatial data, leveraging physical proximity information unavailable in dissociated single-cell experiments.

- [CellChat v2](https://github.com/jinworks/CellChat) - Comprehensive ligand-receptor analysis framework with spatial distance constraints and an extensive curated interaction database (Nature Communications, 2021/2024).
- [LIANA+](https://github.com/saezlab/liana-py) - Meta-framework wrapping multiple CCC methods with spatial extensions, enabling consensus scoring across algorithms (Nature Cell Biology, 2024).
- [COMMOT](https://github.com/zcang/COMMOT) - Optimal transport-based spatial cell-cell communication that models signaling directionality using spatial coordinates (Nature Methods, 2023).
- [SpaTalk](https://github.com/ZJUFanLab/SpaTalk) - Spatial cell-cell communication inference using a graph network model that filters interactions by spatial proximity (Nature Communications, 2022).
- [MISTy](https://github.com/saezlab/mistyR) - Multi-view learning framework that decomposes spatial relationships into intrinsic, juxtacrine, and paracrine components (Genome Biology, 2022).
- [NicheNet](https://github.com/saeyslab/nichenetr) - Predicts ligand-target regulatory potential by integrating prior knowledge of signaling and gene regulatory networks, not spatial-native but widely used (Nature Methods, 2020).
- [SpatialDM](https://github.com/StatBiomed/SpatialDM) - Spatial co-expression-based detection of ligand-receptor interactions using bivariate Moran's I statistics for significance testing (Nature Communications, 2023).
- [FlowSig](https://github.com/axelalmet/FlowSig) - Models intercellular communication as information flow using causal graph structure learning, capturing multi-step signaling cascades (Nature Methods, 2024).
- [DeepLinc](https://github.com/WeilerP/DeepLinc) - Deep learning framework for inferring cell-cell communication networks from spatial transcriptomics using graph neural networks (Genome Biology, 2022).
- [ncem](https://github.com/theislab/ncem) - Neural conditional expectation models that learn how spatial neighborhood composition influences gene expression (Nature Methods, 2023).
- [DeepTalk](https://github.com/JiangBioLab/DeepTalk) - Graph neural network for cell-cell communication inference that integrates single-cell and spatial transcriptomics through a unified framework (Nature Communications, 2024).
- [CellAgentChat](https://github.com/LiuLab-Bioelectronics-Harvard/CellAgentChat) - Agent-based model simulating cell-cell communication dynamics using LLM-powered agents, a novel approach combining AI agents with spatial biology (bioRxiv, 2024).

## Spatial Niches and Microenvironments

Methods for defining and characterizing tissue microenvironments based on local cellular composition and spatial organization.

- [CellCharter](https://github.com/CSOgroup/cellcharter) - Identifies spatial domains and niches using a Gaussian mixture model on graph-aggregated features, with automatic selection of optimal cluster number (Nature Genetics, 2024).
- [NicheCompass](https://github.com/Lotfollahi-lab/nichecompass) - Graph neural network that learns niche-level representations incorporating ligand-receptor interactions and spatial context (Nature Genetics, 2024).
- [UTAG](https://github.com/ElementoLab/utag) - Unsupervised discovery of tissue architecture graphs that identifies niches through message-passing on spatial neighbor graphs (Nature Methods, 2024).
- [Nicheformer](https://github.com/theislab/nicheformer) - Foundation model pre-trained on millions of spatial neighborhoods, enabling niche characterization through transfer learning across tissues (bioRxiv, 2024).
- [Squidpy nhood_enrichment](https://github.com/scverse/squidpy) - Permutation-based neighborhood enrichment analysis quantifying which cell types co-localize more or less than expected by chance (Nature Methods, 2022).

> **Deep dive:** For comprehensive coverage of 54 niche methods organized by definition type, see [Awesome Spatial Omics Niche](https://github.com/LiudengZhang/awesome-spatial-omics-niche) and its [companion site](https://liudengzhang.github.io/awesome-spatial-omics-niche/).

## Cell Annotation and Mapping

Methods for assigning cell type or state labels in spatial transcriptomics data, either through reference mapping or direct annotation.

- [STELLAR](https://github.com/snap-stanford/stellar) - Graph neural network for cell type annotation in spatial proteomics that transfers labels from annotated to unannotated tissues using spatial graph structure (Nature Methods, 2022).
- [InSituType](https://github.com/Nanostring-Biostats/InSituType) - Likelihood-based cell typing for imaging-based spatial transcriptomics with reference profiles and unsupervised cluster discovery (Nature Biotechnology, 2022).
- [STEM](https://github.com/zhanglabtools/STEM) - Spatially and transcriptomically enhanced mapping for spatially resolved cell type annotation using deep metric learning (Cell Genomics, 2024).
- [CelloType](https://github.com/AI4SCR/CelloType) - Transformer-based cell type annotation for spatial proteomics leveraging attention mechanisms on spatial neighborhood contexts (Nature Methods, 2024).
- [STALocator](https://github.com/daifengwanglab/STALocator) - Supervised cell type annotation model for spatial transcriptomics trained on matched single-cell references (Genome Biology, 2023).
- [TACIT](https://github.com/mikecuoco/TACIT) - Transfer learning annotation of cell identity in tissues, designed for low-gene-count imaging-based spatial platforms (bioRxiv, 2023).
- [TransST](https://github.com/bio-it-station/TransST) - Transformer-based spatial transcriptomics cell annotation combining attention mechanisms with spatial neighbor information (Briefings in Bioinformatics, 2024).
- [ABCT](https://github.com/lijin-lab/ABCT) - Annotation by cell type for spatial transcriptomics using semi-supervised learning with spatial context awareness (Bioinformatics, 2024).

## Spatial Differential Expression

Methods for identifying genes differentially expressed as a function of spatial context, tissue niche, or local cellular neighborhood.

- [C-SIDE/spacexr](https://github.com/dmcable/spacexr) - Cell type-specific inference of differential expression that tests for spatially varying gene expression within individual cell types after deconvolution (Nature Biotechnology, 2022).
- [Niche-DE](https://github.com/Qingyang-Qiu/Niche-DE) - Differential expression testing that conditions on niche composition, identifying genes whose expression changes with local cellular neighborhood (Genome Biology, 2024).
- [Vespucci](https://github.com/klarman-cell-observatory/vespucci) - Spatially variable gene expression analysis using Bayesian regression with spatial covariates for identifying location-dependent expression changes (bioRxiv, 2024).
- [CSDE](https://github.com/naity/CSDE) - Cell type-specific spatial differential expression method that decomposes spatial expression variation into cell type and location components (Bioinformatics, 2023).
- [spatialGE](https://github.com/fridleylab/spatialGE) - R toolkit for spatial gene expression analysis including spatial statistics, differential expression, and gradient analysis in tissue sections (Bioinformatics, 2023).

## Subcellular Analysis

Methods for analyzing transcript localization patterns within cells, leveraging the subcellular resolution of imaging-based spatial platforms.

- [Bento](https://github.com/ckmah/bento-tools) - Comprehensive toolkit for subcellular spatial transcriptomics analysis including RNA localization patterns, subcellular domains, and morphological features (Nature Cell Biology, 2024).
- [SPRAWL](https://github.com/dpcook/SPRAWL) - Spatial pattern of RNA and cell morphology analysis quantifying subcellular transcript localization relative to nuclear and cell boundaries (bioRxiv, 2023).
- [FISHFactor](https://github.com/bioFAM/FISHFactor) - Spatially aware factor model for subcellular transcript analysis that identifies gene co-localization programs within cells (Bioinformatics, 2023).
- [InSTAnT](https://github.com/LieberInstitute/InSTAnT) - In situ transcript accessibility and nuclear transport analysis quantifying nuclear-cytoplasmic transcript ratios from imaging data (Nature Neuroscience, 2024).
- [troutpy](https://github.com/Bierinformatik/troutpy) - Python toolkit for analyzing transcript-level spatial data including subcellular localization scoring and transcript clustering (bioRxiv, 2024).

## Spatial Trajectories

Methods for inferring spatial developmental or signaling trajectories that account for physical tissue organization.

- [spaTrack](https://github.com/yzhan155/spaTrack) - Optimal transport-based spatial trajectory inference that tracks cell state transitions across spatial coordinates and time points (Nature Methods, 2024).
- [ONTraC](https://github.com/wwang-chcn/ONTraC) - Ordered niche tracking that infers spatial trajectories by learning continuous niche orderings from tissue architecture (Nature Methods, 2024).
- [STORIES](https://github.com/raphael-group/STORIES) - Spatial trajectory reconstruction that infers cell state transitions using spatial neighbor information and RNA velocity (bioRxiv, 2024).
- [scSpace](https://github.com/RDMelamed/scSpace) - Integrates spatial coordinates with pseudotime to reconstruct spatially informed developmental trajectories (Nature Communications, 2023).
- [SOCS](https://github.com/BiomedicalMachineLearning/SOCS) - Spatially ordered cell states inference combining spatial autocorrelation with trajectory analysis for spatial lineage reconstruction (bioRxiv, 2023).

## Multi-modal Integration

Methods for integrating spatial data with other modalities such as single-cell RNA-seq, histology, or multi-omic measurements.

- [SpatialGLUE](https://github.com/JinmiaoChenLab/SpatialGLUE) - Graph neural network for integrating spatial multi-omics data that jointly embeds paired spatial measurements using graph attention (Nature Methods, 2024).
- [HEST](https://github.com/mahmoodlab/HEST) - Harmonized dataset and benchmark linking spatial transcriptomics with histology images from over 500 samples across 131 studies (Cell, 2024).
- [moscot](https://github.com/theislab/moscot) - Multi-omic single-cell optimal transport for mapping between spatial and dissociated datasets with principled statistical foundations (Nature, 2024).
- [MISO](https://github.com/cstaln/MISO) - Multi-modal integration of spatial omics combining expression, morphology, and spatial information through a unified latent space (Nature Biotechnology, 2024).
- [SpaGE](https://github.com/tabdelaal/SpaGE) - Spatial gene enhancement for imputing unmeasured genes in spatial data using matched single-cell references via domain adaptation (Nucleic Acids Research, 2020).
- [PRECAST](https://github.com/feiyoung/PRECAST) - Probabilistic embedding and clustering of multi-sample spatial transcriptomics with alignment across tissue sections (Nature Communications, 2023).
- [Starfysh](https://github.com/azizilab/starfysh) - Semi-supervised spatial transcriptomics analysis combining archetypal analysis with deep learning for deconvolution and gene imputation (Nature Biotechnology, 2024).
- [MOFA-FLEX](https://github.com/bioFAM/mofaflex) - Extension of MOFA+ to spatial data enabling multi-modal factor analysis with spatially structured priors for spatial multi-omics (Genome Biology, 2024).
- [LLOKI](https://github.com/MarioniLab/LLOKI) - Linked latent observations and knowledge integration for cross-modal spatial data alignment using graph-based embedding (bioRxiv, 2024).
- [pyWNN](https://github.com/scverse/muon) - Weighted nearest neighbor method in muon for integrating multi-modal data by learning modality-specific weights per cell (Nature Methods, 2022).

## Foundation Models

Large pre-trained models designed for spatial biology that learn general representations from large-scale spatial datasets.

- [DeepSpot-M](https://github.com/ratschlab/DeepSpotM) - Multimodal foundation model for transcriptome-wide virtual spatial transcriptomics from histology (medRxiv, 2026).
- [Nicheformer](https://github.com/theislab/nicheformer) - Transformer pre-trained on dissociated and spatial single-cell data from over 110 million cells, enabling cross-tissue niche characterization through transfer learning (bioRxiv, 2024).
- [Novae](https://github.com/MICS-Lab/novae) - Graph foundation model for spatial transcriptomics that learns tissue architecture representations transferable across technologies and tissues (Nature Machine Intelligence, 2024).
- [CellSAM](https://github.com/vanvalenlab/cellSAM) - Segment Anything Model adapted for cell segmentation, providing zero-shot segmentation capability across diverse microscopy image types (bioRxiv, 2024).
- [DECIPHER](https://github.com/sharplab-pcg/DECIPHER) - Multimodal foundation model integrating spatial transcriptomics with histology images for joint tissue representation learning (bioRxiv, 2024).
- [ChatSpatial](https://github.com/QSong-github/ChatSpatial) - LLM-powered conversational interface for spatial transcriptomics analysis, wrapping analytical tools in natural language interaction (bioRxiv, 2024).
- [SpatialAgent](https://github.com/Genentech/SpatialAgent) - LangGraph-based AI agent that automates spatial transcriptomics analysis pipelines using large language models for task planning (bioRxiv, 2024).
- [CELLama](https://github.com/cellama-team/CELLama) - Large language model fine-tuned for cell type annotation and spatial analysis tasks, adapting LLM capabilities to single-cell and spatial biology (bioRxiv, 2024).
- [SpatialFusion](https://doi.org/10.1038/s41592-024-02502-4) - Deep learning model for integrating histology images with spatial transcriptomics to predict gene expression from morphology alone (Nature Methods, 2024).

## 3D Reconstruction and Alignment

Methods for aligning serial tissue sections and reconstructing three-dimensional tissue architecture from 2D spatial transcriptomics data.

- [PASTE/PASTE2/PASTE3](https://github.com/raphael-group/paste) - Optimal transport-based alignment of spatial transcriptomics sections that maps spots across slices while accounting for partial overlap, with successive versions improving scalability (Nature Methods, 2022/2024).
- [STalign](https://github.com/JEFworks-Lab/STalign) - Diffeomorphic registration of spatial transcriptomics data using large deformation diffeomorphic metric mapping adapted for gene expression data (Nature Communications, 2023).
- [Spateo](https://github.com/aristoteleo/spateo-release) - Comprehensive framework including 3D reconstruction from serial sections with morphological alignment, part of a broader spatial analysis suite (Cell, 2024).
- [SPIRAL](https://github.com/guott15/SPIRAL) - Symmetric pairwise iterative registration and alignment for spatial transcriptomics that jointly aligns multiple sections without a fixed reference (Nature Methods, 2024).
- [CalicoST](https://github.com/raphael-group/CalicoST) - Clone-aware alignment of spatial transcriptomics sections that simultaneously infers copy number alterations and performs section registration (Nature Methods, 2024).
- [iStar](https://github.com/daviddcai/istar) - Imaging-based spatial transcriptomics alignment and reconstruction using histology-guided registration for 3D modeling (Nature Biotechnology, 2024).
- [TOAST](https://github.com/JiayuSuPKU/TOAST) - Transcript-aware optimal section alignment for 3D spatial transcriptomics reconstruction leveraging both expression and spatial information (bioRxiv, 2024).
- [SANTO](https://github.com/raphael-group/santo) - Spatial alignment with nonlinear transform optimization for registering spatial transcriptomics data from consecutive tissue sections (bioRxiv, 2024).

## Visualization and Interactive Tools

Tools for exploring and visualizing spatial omics data interactively, from publication-ready static plots to web-based exploration platforms.

- [Vitessce](https://github.com/vitessce/vitessce) - Web-based interactive visualization framework for multi-modal spatial single-cell data, supporting coordinated multiple views with SpatialData backend (Nature Methods, 2024).
- [napari-spatialdata](https://github.com/scverse/napari-spatialdata) - napari plugin for interactive visualization of SpatialData objects, enabling layer-by-layer exploration of images, labels, points, and shapes (bioRxiv, 2024).
- [TissUUmaps](https://github.com/TissUUmaps/TissUUmaps) - Browser-based viewer for large-scale spatial transcriptomics data with GPU-accelerated rendering, handling gigapixel images smoothly (Nature Communications, 2023).
- [Rakaia](https://github.com/GaitiLab/Rakaia) - R Shiny application for interactive spatial transcriptomics visualization focusing on Visium and Visium HD data exploration (bioRxiv, 2024).
- [sgs](https://github.com/DongqingSun96/sgs) - Simple and lightweight spatial gene expression visualization tool for quick static plots from Visium and MERFISH data (Bioinformatics, 2023).
- [spatialdata-plot](https://github.com/scverse/spatialdata-plot) - Matplotlib-based static plotting library for SpatialData objects, providing the default visualization for the SpatialData ecosystem (bioRxiv, 2024).
- [SPATA2](https://github.com/theMILOlab/SPATA2) - R framework for spatial transcriptomics analysis and visualization with extensive plotting functions and interactive Shiny modules (Genome Biology, 2024).
- [semla](https://github.com/ludvigla/semla) - R package for analysis and visualization of Visium data with tidy-style syntax and publication-ready spatial plots (Bioinformatics, 2024).
- [spatialdata-xenium-explorer](https://github.com/scverse/spatialdata-io) - Export SpatialData objects to Xenium Explorer format, enabling visualization of any spatial data in 10x Genomics' polished desktop viewer (bioRxiv, 2024).
- [Sopa](https://github.com/gustaveroussy/sopa) - Includes a visualization module for interactive exploration of segmented spatial data alongside its processing pipeline (Nature Communications, 2024).

## Frameworks and Infrastructure

General-purpose analysis frameworks and data infrastructure for spatial omics, providing unified APIs and data structures.

- [Squidpy](https://github.com/scverse/squidpy) - Comprehensive spatial analysis framework in the scverse ecosystem providing graph analysis, spatial statistics, and image feature extraction with AnnData integration (Nature Methods, 2022).
- [SpatialData](https://github.com/scverse/spatialdata) - Unified data model and framework for storing, processing, and annotating spatial omics data across technologies, the emerging standard in the scverse ecosystem (Nature Methods, 2024).
- [Giotto Suite](https://github.com/drieslab/Giotto) - Comprehensive R/Python framework for spatial data analysis with its own data structures, extensive documentation, and broad method coverage (Genome Biology, 2021).
- [Seurat v5](https://github.com/satijalab/seurat) - Widely used single-cell framework with spatial extensions including image-based visualization and integration tools, the default choice for many R users (Nature Biotechnology, 2024).
- [Voyager](https://github.com/pachterlab/voyager) - R/Bioconductor framework bringing geospatial analysis methods to spatial transcriptomics with SpatialExperiment integration (bioRxiv, 2023).
- [Spateo](https://github.com/aristoteleo/spateo-release) - All-in-one Python framework for spatial transcriptomics covering pre-processing through 3D reconstruction, with a focus on RNA dynamics (Cell, 2024).
- [sopa](https://github.com/gustaveroussy/sopa) - Technology-agnostic pipeline from raw images to annotated cells with Snakemake workflow support and SpatialData output (Nature Communications, 2024).
- [scanpy](https://github.com/scverse/scanpy) - Core single-cell analysis framework whose AnnData structure and preprocessing functions underpin most Python spatial analysis tools (Genome Biology, 2018).
- [harpy](https://github.com/saeyslab/harpy) - Pipeline for spatial proteomics data analysis covering segmentation, phenotyping, and spatial statistics in a unified workflow (bioRxiv, 2024).
- [PathML](https://github.com/Dana-Farber-AIOS/pathml) - Machine learning framework for computational pathology with spatial omics integration, bridging histopathology AI with molecular spatial data (bioRxiv, 2024).
- [SPATA2](https://github.com/theMILOlab/SPATA2) - R toolkit for spatial transcriptomics analysis with interactive visualization and downstream analysis modules (Genome Biology, 2024).
- [spatialGE](https://github.com/fridleylab/spatialGE) - R toolkit providing spatial statistics, visualization, and differential expression analysis for spatial transcriptomics (Bioinformatics, 2023).

## Benchmarks

Systematic comparisons and challenge datasets for evaluating spatial omics computational methods.

- [Li et al. deconvolution benchmark](https://doi.org/10.1038/s41592-022-01480-9) - Comprehensive evaluation of 14 spatial deconvolution methods using ground-truth simulations and real paired spatial/single-cell datasets (Nature Methods, 2022).
- [Yan et al. deconvolution benchmark](https://doi.org/10.1038/s41467-023-37168-7) - Independent deconvolution benchmark finding cell2location and RCTD as top performers across tissues, confirming robustness of probabilistic approaches (Nature Communications, 2023).
- [Yuan et al. clustering benchmark](https://doi.org/10.1038/s41592-024-02215-8) - Large-scale evaluation of spatial domain identification methods across multiple datasets and technologies, identifying BANKSY and STAGATE as consistently strong (Nature Methods, 2024).
- [Dong et al. clustering benchmark](https://doi.org/10.1186/s13059-023-03062-0) - Systematic comparison of spatial clustering methods with practical guidance on method selection based on tissue complexity and data type (Genome Biology, 2023).
- [Petukhov et al. segmentation benchmark](https://doi.org/10.1038/s41587-023-01837-5) - Evaluation of cell segmentation methods for imaging-based spatial transcriptomics showing that transcript-aware methods outperform morphology-only approaches (Nature Biotechnology, 2023).
- [Greenwald et al. segmentation benchmark](https://doi.org/10.1038/s41587-021-01094-0) - TissueNet-based evaluation of deep learning segmentation models establishing Mesmer as a strong baseline for whole-cell segmentation (Nature Biotechnology, 2022).
- [Weber et al. SVG benchmark](https://doi.org/10.1186/s13059-023-02978-x) - Comparison of spatially variable gene detection methods showing nnSVG and SPARK-X as scalable options with complementary strengths (Genome Biology, 2023).
- [Fischer et al. CCC benchmark](https://doi.org/10.1038/s41592-023-01782-6) - Evaluation of cell-cell communication methods on spatial data demonstrating that spatial constraints substantially reduce false-positive interactions (Nature Methods, 2023).
- [NeurIPS Open Problems](https://openproblems.bio/) - Community benchmarking platform including spatial decomposition and spatial embedding tasks with standardized evaluation metrics (NeurIPS, 2023).
- [Museum of Spatial Transcriptomics](https://doi.org/10.1038/s41592-022-01409-2) - While primarily a review, includes systematic cataloging of method capabilities serving as a reference for technology selection (Nature Methods, 2022).

## Datasets and Databases

Standard benchmark datasets, tissue atlases, and data repositories for spatial omics.

### Standard Benchmarks

- [DLPFC Visium](https://doi.org/10.1038/s41593-020-00787-0) - Dorsolateral prefrontal cortex Visium dataset with manually annotated cortical layers, the most widely used benchmark for spatial domain methods (Nature Neuroscience, 2021).
- [Mouse brain MERFISH](https://alleninstitute.org/what-we-do/brain-science/research/products-tools/) - Allen Institute whole-mouse-brain MERFISH atlas providing ground-truth cell types at single-cell spatial resolution across entire brain sections (Allen Institute, 2023).
- [Stereo-seq mouse embryo](https://doi.org/10.1016/j.cell.2022.04.003) - Subcellular-resolution spatial atlas of mouse organogenesis covering E9.5-E16.5, the largest spatial transcriptomics dataset by area (Cell, 2022).
- [Xenium breast cancer](https://www.10xgenomics.com/datasets) - 10x Genomics reference dataset for Xenium platform validation on breast cancer tissue with matched Visium and H&E data (10x Genomics, 2023).

### Atlases

- [Allen Brain Cell Atlas](https://portal.brain-map.org/atlases-and-data/bca) - Multimodal brain atlas combining MERFISH spatial data with single-cell RNA-seq across the entire mouse brain, the gold standard for brain spatial reference data (Allen Institute, 2023).
- [HuBMAP](https://hubmapconsortium.org/) - Human BioMolecular Atlas Project generating spatial maps of healthy human tissues across organs, providing standardized spatial omics reference data (Nature, 2019).
- [Human Cell Atlas](https://www.humancellatlas.org/) - International consortium building comprehensive reference maps of all human cells including spatial tissue maps from multiple organs (eLife, 2017).

### Repositories

- [SODB](https://gene.ai.tencent.com/SpatialOmics/) - Spatial Omics DataBase providing unified access to over 3,500 curated spatial omics datasets with standardized metadata and analysis-ready formats (Nucleic Acids Research, 2023).
- [STOmicsDB](https://db.cngb.org/stomics/) - Database of spatial transcriptomics data from the BGI CNGB, hosting Stereo-seq and other spatial datasets with interactive visualization (Nucleic Acids Research, 2024).
- [SpatialDB](https://www.spatialomics.org/SpatialDB/) - Curated database for spatial gene expression data providing standardized downloads across technologies (Nucleic Acids Research, 2020).
- [10x Genomics Datasets](https://www.10xgenomics.com/datasets) - Official repository of Visium, Visium HD, and Xenium datasets from 10x Genomics, the most commonly used data source for method development (10x Genomics, 2020-2024).
- [CZ CELLxGENE](https://cellxgene.cziscience.com/) - Chan Zuckerberg Initiative single-cell data portal hosting curated spatial datasets alongside dissociated data with standardized metadata (CZI, 2021).
- [Broad Single Cell Portal](https://singlecell.broadinstitute.org/single_cell) - Data sharing platform hosting spatial transcriptomics studies alongside single-cell RNA-seq with integrated visualization tools (Broad Institute, 2019).

## Companion Site

An interactive companion site with filterable tables, technology decision trees, and method comparison guides is available at **[liudengzhang.github.io/awesome-spatial-omics](https://liudengzhang.github.io/awesome-spatial-omics/)**.

## Contributing

Contributions are welcome. Please read the [contribution guidelines](contributing.md) before submitting a pull request.
