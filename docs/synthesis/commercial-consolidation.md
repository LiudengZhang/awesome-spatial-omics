# Commercial Consolidation in Spatial Omics

The spatial omics instrument landscape is undergoing rapid consolidation. Between 2023 and 2024, three major events reshaped the commercial field: Bruker acquired NanoString, 10x Genomics acquired Vizgen, and Ionpath shut down. The result is a market with fewer independent vendors, larger integrated platforms, and increasing concerns about vendor lock-in.

## The consolidation events

### 10x Genomics acquires Vizgen (2024)

10x Genomics, the dominant player in sequencing-based spatial transcriptomics (Visium, Xenium), acquired Vizgen in 2024. Vizgen developed the MERSCOPE platform, which commercialized MERFISH -- one of the two leading imaging-based spatial transcriptomics technologies.

**What this means:**

- 10x now controls both sequencing-based (Visium, Visium HD) and imaging-based (Xenium, MERSCOPE/MERFISH) spatial transcriptomics
- The two leading imaging-based platforms (Xenium and MERSCOPE) are now under one roof
- MERFISH, originally an academic technology developed by Xiaowei Zhuang's lab at Harvard, moves further into the proprietary sphere
- Existing MERSCOPE users face uncertainty about long-term platform support and reagent availability

### Bruker acquires NanoString (2023)

Bruker, a diversified instrumentation company, acquired NanoString Technologies in 2023. NanoString developed the GeoMx Digital Spatial Profiler and the CosMx Spatial Molecular Imager.

**What this means:**

- GeoMx (region-of-interest spatial profiling) and CosMx (single-cell spatial transcriptomics) now operate under Bruker's life sciences division
- NanoString's financial difficulties prior to acquisition had raised concerns about platform viability; Bruker's acquisition provides stability
- CosMx remains the primary competitor to Xenium in the commercial single-cell spatial transcriptomics market
- Integration into Bruker's broader instrumentation portfolio could accelerate multi-modal workflows (e.g., combining CosMx with mass spectrometry imaging)

### Ionpath shuts down (2024)

Ionpath, the company commercializing Multiplexed Ion Beam Imaging (MIBI-TOF) for spatial proteomics, ceased operations in 2024.

**What this means:**

- MIBI-TOF, a technology developed by Michael Angelo's lab at Stanford, loses its commercial platform
- Existing MIBI-TOF instruments become unsupported; reagent supply chains are disrupted
- Researchers with MIBI data face long-term data format and reproducibility challenges
- The spatial proteomics market consolidates around Imaging Mass Cytometry (Standard BioTools/Fluidigm), CODEX/PhenoCycler (Akoya Biosciences), and antibody-based approaches

## The current landscape

| Vendor | Platforms | Technology type | Market position |
|---|---|---|---|
| 10x Genomics | Visium, Visium HD, Xenium, MERSCOPE | Sequencing-based + imaging-based transcriptomics | Dominant, broadest portfolio |
| Bruker (NanoString) | CosMx, GeoMx | Imaging-based transcriptomics + ROI profiling | Strong second, growing |
| Akoya Biosciences | PhenoCycler (CODEX), PhenoImager | Spatial proteomics (cyclic immunofluorescence) | Leading spatial proteomics |
| Standard BioTools | Imaging Mass Cytometry (IMC) | Spatial proteomics (mass cytometry) | Established, niche |
| BGI/STOmics | Stereo-seq | Sequencing-based transcriptomics | Strong in Asia, growing globally |
| Resolve Biosciences | Molecular Cartography | Imaging-based transcriptomics | Smaller, European market |
| Curio Bioscience | Slide-seq (Curio Seeker) | Sequencing-based transcriptomics | Niche, bead-based |

## Implications for researchers

### Fewer choices, clearer defaults

Consolidation simplifies platform selection for new adopters. The choice for spatial transcriptomics is increasingly binary: 10x (Visium/Xenium) for the broadest ecosystem and most third-party tool support, or Bruker (CosMx) for an alternative with different technical strengths (larger gene panels, FFPE optimization).

### Vendor lock-in risks

As fewer vendors control more of the technology stack, researchers face increasing lock-in through:

- **Proprietary data formats.** Each vendor uses custom file formats that require vendor software for initial processing. While open-source converters exist, they lag behind vendor format updates.
- **Proprietary analysis software.** 10x's Loupe Browser, NanoString's AtoMx, and Akoya's InForm/HALO provide vendor-curated analysis workflows. These are convenient but create dependency.
- **Reagent ecosystems.** Gene panels, probe sets, and antibody conjugates are vendor-specific and often proprietary. Switching platforms means redesigning the entire experimental panel.
- **Instrument-software coupling.** Raw data processing (e.g., Space Ranger for Visium, Xenium Ranger) is tightly coupled to vendor instruments. Open-source alternatives (STARsolo, Baysor) exist but are not always feature-complete.

!!! warning "The vendor lock-in trap"
    Choosing a spatial platform is a multi-year commitment. The instrument cost ($200k--$800k), reagent contracts, and workflow development create high switching costs. Platform discontinuation (as with Ionpath/MIBI) can strand years of investment.

### Data format fragmentation

Each consolidation event risks creating larger but still incompatible data ecosystems:

- 10x formats: Space Ranger H5, Xenium cell feature matrix, MERSCOPE output
- Bruker/NanoString formats: CosMx flat files, GeoMx DSP exports
- Akoya formats: CODEX/PhenoCycler QPTIFF

The scverse ecosystem (AnnData, SpatialData) provides the most promising path toward format-agnostic analysis, with readers for most major formats. But conversion is imperfect, and vendor-specific metadata is often lost in translation.

## Open-source as counterbalance

Academic open-source tools serve as a critical counterbalance to commercial consolidation. Key projects:

| Project | Role | Significance |
|---|---|---|
| SpatialData (scverse) | Universal data container | Technology-agnostic data representation reduces vendor lock-in |
| Squidpy | Spatial analysis framework | Vendor-independent analysis workflows |
| Baysor | Cell segmentation | Open alternative to vendor segmentation pipelines |
| STARsolo | Raw data processing | Open alternative to Space Ranger |
| napari | Visualization | Open alternative to vendor visualization tools |
| Giotto | Analysis framework (R) | Comprehensive open-source alternative |

These tools cannot replace vendor instruments, but they reduce dependency on vendor software for downstream analysis. Investing in open-source spatial analysis tools is both a scientific and a strategic decision.

## BGI/STOmics: the non-Western alternative

BGI Genomics, through its STOmics division, offers Stereo-seq -- a sequencing-based spatial technology with nanometer resolution across large tissue areas. Stereo-seq has several distinctive characteristics:

- Achieves the highest spatial resolution of any sequencing-based method
- Has produced the largest published spatial datasets (mouse embryo atlas with billions of reads)
- Is widely used in China and increasingly in Europe, but has limited adoption in North America
- Uses its own data ecosystem (STOmicsDB, SAW pipeline) with growing but incomplete interoperability with Western tools
- Faces geopolitical headwinds that may limit adoption in some research environments

As 10x and Bruker consolidate the Western market, BGI/STOmics represents an independent alternative that may grow in importance globally.

## What to watch

**Will 10x unify Xenium and MERSCOPE?** The two platforms have overlapping capabilities. 10x may maintain both, merge them, or phase out MERSCOPE in favor of Xenium. This decision will significantly affect existing MERSCOPE users.

**Will Bruker invest in CosMx?** NanoString's acquisition by Bruker provides financial stability, but Bruker's commitment to continued CosMx development and market competition with 10x will determine whether a viable second option persists.

**Will open-source alternatives mature?** Projects like SpatialData, Squidpy, and napari are the best insurance against vendor lock-in. Their continued development depends on academic funding and community contributions -- both of which are fragile.

**Will new entrants emerge?** The spatial omics market is large and growing. New technologies (expansion microscopy, in situ sequencing variants, novel imaging modalities) could produce new commercial entrants, countering consolidation.

## Further reading

- [Technologies Overview](../technologies/overview.md) for technical comparison of platforms
- [The Pipeline Problem](pipeline-problem.md) for how tool fragmentation interacts with vendor fragmentation
- [The Technology-Analysis Gap](technology-analysis-gap.md) for how technology advances outpace analysis tools
- [Datasets](../datasets/datasets.md) for where to access spatial data from different platforms
