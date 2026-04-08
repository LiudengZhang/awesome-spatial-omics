# Technology Overview

The spatial omics landscape spans dozens of technologies that differ along three fundamental axes: **resolution** (what spatial granularity is captured), **throughput** (how many genes/proteins are measured), and **tissue compatibility** (fresh-frozen vs. FFPE). Understanding these trade-offs is essential for technology selection.

---

## Resolution vs. throughput landscape

| Technology | Resolution | Genes/Proteins | Tissue type | Commercial? |
|-----------|-----------|---------------|-------------|-------------|
| **Visium** | 55 um (multi-cell) | Whole transcriptome | FF, FFPE (CytAssist) | Yes (10x) |
| **Visium HD** | 2 um (bins) | Whole transcriptome | FF, FFPE | Yes (10x) |
| **Slide-seq V2** | 10 um | Whole transcriptome | FF only | No (academic) |
| **Stereo-seq** | ~500 nm | Whole transcriptome | FF | Yes (BGI/STOmics) |
| **Open-ST** | Subcellular | Whole transcriptome | FF | No (open-source) |
| **Seq-Scope** | Subcellular | Whole transcriptome | FF | No (academic) |
| **Xenium** | Subcellular | 100-5,000 (panel) | FF, FFPE | Yes (10x) |
| **MERSCOPE** | Subcellular | 100-1,000 (panel) | FF, FFPE | Yes (Vizgen/10x) |
| **CosMx SMI** | Subcellular | 1,000-6,000 (panel) | FF, FFPE | Yes (Bruker) |
| **GeoMx DSP** | ROI-level | Whole transcriptome or ~100 proteins | FF, FFPE | Yes (Bruker) |
| **PhenoCycler** | Subcellular | ~100 proteins | FF, FFPE | Yes (Akoya) |
| **MIBI** | ~260 nm | ~40 proteins | FF, FFPE | Discontinued |
| **IMC/Hyperion** | 1 um | ~40 proteins | FF, FFPE | Yes (Standard BioTools) |
| **MALDI-MSI** | 5-50 um | Untargeted metabolites | FF, FFPE | Yes (Bruker, others) |

---

## The fundamental trade-off

!!! info "Sequencing-based vs. imaging-based"
    - **Sequencing-based methods** (Visium, Visium HD, Stereo-seq, Slide-seq) capture the **whole transcriptome** but at lower spatial resolution (multi-cell spots or small bins that require aggregation). Discovery-oriented: no need to pre-select a gene panel.
    - **Imaging-based methods** (Xenium, MERFISH, CosMx, seqFISH) achieve **subcellular resolution** with single-molecule sensitivity but require a **pre-designed gene panel** (typically 100-5,000 genes). Hypothesis-confirming or targeted discovery.
    - **Protein-based methods** (PhenoCycler, IMC, MIBI) measure **protein abundance** at subcellular resolution but are limited to **~40-100 markers** by antibody availability. Best for immune phenotyping and tissue architecture.

---

## Commercial consolidation (2023-2025)

The spatial omics vendor landscape has undergone rapid consolidation:

| Event | Year | Impact |
|-------|------|--------|
| 10x Genomics acquires Vizgen (MERSCOPE) | 2024 | 10x now controls both the leading sequencing-based (Visium/HD) and imaging-based (Xenium, MERSCOPE) platforms. MERSCOPE's future product roadmap is uncertain. |
| NanoString acquired by Bruker | 2024 | CosMx SMI and GeoMx DSP continue under the Bruker Spatial Biology brand. Financial stability improved but R&D direction may shift. |
| Ionpath shuts down | 2023 | MIBIscope (MIBI) is no longer commercially available. Existing instruments continue to operate but without vendor support. A significant loss for spatial proteomics. |
| Akoya Biosciences financial pressure | 2024-25 | PhenoCycler remains available but Akoya's long-term viability is uncertain. |

!!! warning "Vendor risk"
    Technology selection now requires evaluating commercial viability alongside scientific merit. Choosing a platform from a vendor under financial pressure creates risk for long-term projects. 10x Genomics and Bruker are currently the most stable vendors, though both face their own challenges.

---

## Technology selection guide

### If the goal is discovery (no prior gene list)

Use a **sequencing-based** method:

- **Standard tissue, established workflow needed**: Visium (most tutorials, most tools, largest community)
- **Higher resolution needed**: Visium HD (2 um bins, but tooling is still maturing)
- **Largest possible tissue area**: Stereo-seq (centimeter-scale, but data management is challenging)
- **Academic, budget-conscious**: Open-ST or Seq-Scope (open-source protocols, no instrument purchase)

### If the goal is targeted validation (known gene panel)

Use an **imaging-based** method:

- **Largest panel needed**: CosMx SMI (up to ~6,000 genes) or Xenium (up to ~5,000 genes)
- **Established MERFISH expertise**: MERSCOPE (mature platform, but monitor 10x integration plans)
- **FFPE clinical samples**: Xenium or CosMx (both handle FFPE well)
- **Subcellular transcript localization matters**: Any imaging-based method (all provide molecule coordinates)

### If the goal is protein phenotyping

Use a **protein-based** method:

- **High-plex protein on standard microscope**: PhenoCycler (formerly CODEX)
- **Highest spatial resolution for protein**: IMC/Hyperion (1 um, metal-tagged antibodies)
- **Combined RNA + protein**: CosMx SMI (supports both in the same experiment)

### If the goal is chromatin accessibility or multi-omic

See [Spatial Multi-Omics](spatial-multiomics.md) -- emerging technologies with limited but growing tool support.

### If the goal is metabolite mapping

See [Spatial Metabolomics](spatial-metabolomics.md) -- mass spectrometry imaging approaches with fundamentally different data structures.

---

## Cross-technology comparison: practical considerations

| Consideration | Sequencing-based | Imaging-based | Protein-based |
|--------------|-----------------|--------------|--------------|
| **Cost per sample** | $$ | $$$ | $$ |
| **Instrument cost** | Requires sequencer | Dedicated instrument ($500K-1M+) | Varies (microscope to dedicated) |
| **Hands-on time** | Low (standard library prep) | Medium-High (imaging runs hours-days) | Medium (iterative staining) |
| **FFPE compatibility** | Yes (CytAssist, some platforms) | Yes (Xenium, CosMx) | Yes (most) |
| **Data size per sample** | 1-50 GB | 50-500 GB (images) | 10-100 GB (images) |
| **Computational expertise needed** | Medium | High (segmentation-dependent) | Medium-High |
| **Deconvolution needed?** | Yes (except Visium HD with Bin2Cell) | No (single-cell resolution) | No |
| **Reference scRNA-seq needed?** | Usually (for deconvolution) | Helpful (for annotation) | No |

For detailed coverage of each technology category, see:

- [Sequencing-Based Technologies](sequencing-based.md)
- [Imaging-Based Technologies](imaging-based.md)
- [Spatial Proteomics](spatial-proteomics.md)
- [Spatial Multi-Omics](spatial-multiomics.md)
- [Spatial Metabolomics](spatial-metabolomics.md)
