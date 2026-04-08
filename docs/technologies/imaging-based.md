# Imaging-Based Spatial Transcriptomics

Imaging-based spatial methods detect individual mRNA molecules in situ using fluorescent probes, achieving subcellular resolution with single-molecule sensitivity. The trade-off: a pre-designed gene panel is required (typically 100-6,000 genes), making these methods targeted rather than discovery-oriented. Cell segmentation quality is critical because every downstream analysis depends on accurately assigning transcripts to cells.

---

## Academic Methods

### MERFISH

**How it works:** Multiplexed error-robust FISH uses combinatorial barcoding -- each gene is assigned a unique binary barcode across multiple rounds of hybridization and imaging. Error-correction codes (like Hamming codes) enable robust detection of hundreds to thousands of genes simultaneously.

| Property | Value |
|----------|-------|
| Resolution | Single-molecule (~100 nm localization) |
| Throughput | 100-10,000+ genes per experiment |
| Tissue type | Fresh-frozen, with FFPE adaptations |
| Commercial status | Commercialized as **MERSCOPE** (Vizgen, now 10x Genomics) |
| Key paper | [Chen et al., Science 2015](https://doi.org/10.1126/science.aaa6090) |

!!! tip "Recommended analysis tools"
    - Segmentation: [Baysor](../methods/cell-segmentation.md) (transcript-based, no image needed), Cellpose2 (image-based)
    - Spatial domains: [GraphST](../methods/spatial-domains.md), BANKSY
    - CCC: [COMMOT](../methods/cell-cell-communication.md) (optimal transport on coordinates)

**Limitations:** Imaging time scales with tissue area and panel size. Dense tissues (e.g., brain) can have crowding artifacts. Error-correction reduces effective throughput (more imaging rounds per gene than naive encoding).

---

### seqFISH / seqFISH+

**How it works:** Sequential FISH uses temporal barcoding -- genes are identified by the sequence of fluorescent colors observed across multiple hybridization rounds. seqFISH+ extended this to profile >10,000 genes per cell.

| Property | Value |
|----------|-------|
| Resolution | Single-molecule, subcellular |
| Throughput | seqFISH: ~100 genes; seqFISH+: 10,000+ genes |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic** (Cai lab, Caltech) |
| Key paper | [Eng et al., Nature 2019](https://doi.org/10.1038/s41586-019-1049-y) (seqFISH+) |

**Limitations:** Imaging time scales linearly with the number of pseudocolors and rounds, making 10,000-gene experiments take days per field of view. Limited tissue area per experiment. No commercial platform, so adoption requires significant microscopy expertise.

---

### STARmap / STARmap PLUS

**How it works:** In situ sequencing within a hydrogel matrix. DNA amplicons are embedded in a polyacrylamide gel that preserves tissue architecture, then sequenced by ligation in situ. STARmap PLUS extended this to thick tissue sections for 3D spatial mapping.

| Property | Value |
|----------|-------|
| Resolution | Subcellular |
| Throughput | ~1,000-3,000 genes |
| Tissue type | Fresh-frozen, thick sections (up to 150 um for PLUS) |
| Commercial status | **Academic** (Wang lab, MIT/Broad) |
| Key paper | [Wang et al., Science 2018](https://doi.org/10.1126/science.aat5691); [Shi et al., Nat Biotechnol 2022](https://doi.org/10.1038/s41587-022-01251-z) (PLUS) |

!!! info "3D capability"
    STARmap PLUS is one of the few methods capable of true 3D spatial transcriptomics within thick tissue sections, rather than serial 2D sections. This comes at the cost of protocol complexity and lower throughput per experiment.

---

### osmFISH

**How it works:** Single-molecule FISH with sequential rounds of hybridization, each round targeting a different gene. Conceptually simpler than combinatorial approaches but limited to small gene panels.

| Property | Value |
|----------|-------|
| Resolution | Single-molecule |
| Throughput | ~30-50 genes per experiment |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic** (Bhatt, Bhatt lab / Bhatt/Bhatt Linnarsson lab) |
| Key paper | [Codeluppi et al., Nat Methods 2018](https://doi.org/10.1038/s41592-018-0175-z) |

**Limitations:** Very small gene panels limit utility for discovery. Superseded by higher-throughput methods (MERFISH, seqFISH+) for most applications. Primarily of historical significance.

---

### In Situ Sequencing (ISS)

**How it works:** Padlock probes hybridize to target mRNAs in situ, are circularized and amplified by rolling circle amplification, then sequenced by ligation directly in the tissue section.

| Property | Value |
|----------|-------|
| Resolution | Single-molecule |
| Throughput | ~100-200 genes |
| Tissue type | Fresh-frozen, FFPE |
| Commercial status | Commercialized as **Resolve Molecular Cartography** |
| Key paper | [Ke et al., Nat Methods 2013](https://doi.org/10.1038/nmeth.2563) |

**Limitations:** Rolling circle amplification can introduce spatial displacement artifacts. Sensitivity is lower than MERFISH/seqFISH for the same targets. FFPE compatibility is an advantage over many competing methods.

---

### ExSeq (Expansion Sequencing)

**How it works:** Combines expansion microscopy (physically enlarging the tissue ~4x) with in situ sequencing, achieving effective nanoscale resolution of transcript localization.

| Property | Value |
|----------|-------|
| Resolution | ~70 nm (effective, after expansion) |
| Throughput | ~100-1,000 genes |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic** (Boyden lab, MIT) |
| Key paper | [Alon et al., Science 2021](https://doi.org/10.1126/science.aax2656) |

!!! info "Unique resolution"
    ExSeq provides the highest effective spatial resolution of any spatial transcriptomics method, enabling analysis of transcript localization within subcellular compartments (dendrites, synapses). The trade-off is extreme protocol complexity and very small tissue areas.

---

### EASI-FISH

**How it works:** Expansion-assisted iterative FISH designed for thick tissue sections (up to 300 um). Tissue is expanded, then probed with sequential FISH rounds.

| Property | Value |
|----------|-------|
| Resolution | Subcellular |
| Throughput | ~100-300 genes |
| Tissue type | Fresh-frozen, thick sections |
| Commercial status | **Academic** (HHMI Janelia) |
| Key paper | [Wang et al., Cell 2021](https://doi.org/10.1016/j.cell.2021.11.024) |

**Limitations:** Processing time is long (tissue expansion takes days). Limited gene panels. Primarily used in neuroscience applications where 3D thick-tissue imaging is essential.

---

### HybISS

**How it works:** Hybridization-based in situ sequencing combining padlock probes with sequential hybridization readout, offering improved sensitivity over standard ISS.

| Property | Value |
|----------|-------|
| Resolution | Single-molecule |
| Throughput | ~100-500 genes |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic** (Nilsson lab) |
| Key paper | [Gyllborg et al., Nucleic Acids Res 2020](https://doi.org/10.1093/nar/gkaa792) |

---

## Commercial Platforms

### Xenium (10x Genomics)

**How it works:** Padlock probe-based in situ detection with rolling circle amplification and fluorescent decoding. Provides molecule-level coordinates, DAPI staining, and optional H&E post-staining for tissue morphology.

| Property | Value |
|----------|-------|
| Resolution | Subcellular (molecule-level) |
| Throughput | Pre-designed panels (100-5,000 genes); custom panels available |
| Tissue type | Fresh-frozen, FFPE |
| Tissue area | Up to ~1 cm^2 |
| Commercial status | **Active** (10x Genomics, launched 2023) |

!!! tip "Recommended analysis tools"
    - Data loading: SpatialData, Squidpy (native Xenium readers)
    - Segmentation: Cellpose2 (re-segmentation), Baysor, or use 10x default boundaries
    - Cell typing: STELLAR, Tangram (reference-based transfer)
    - CCC: COMMOT, CellChat v2

**Strengths:** Rapidly growing ecosystem, good FFPE performance, expanding panel sizes, integration with Visium/Visium HD workflows. **Limitations:** Panel-based (must pre-select genes), instrument cost ($300K+), imaging throughput limits per-run capacity.

---

### MERSCOPE (Vizgen / 10x Genomics)

**How it works:** Commercial implementation of MERFISH with automated sample processing, imaging, and primary analysis.

| Property | Value |
|----------|-------|
| Resolution | Subcellular (molecule-level) |
| Throughput | 100-1,000 genes (standard panels) |
| Tissue type | Fresh-frozen, FFPE |
| Commercial status | **Active but uncertain** (Vizgen acquired by 10x Genomics in 2024) |

!!! warning "Commercial uncertainty"
    10x Genomics' acquisition of Vizgen raises questions about MERSCOPE's long-term future alongside Xenium. Existing MERSCOPE data and workflows remain valid, but new instrument purchases should factor in platform longevity risk. 10x has indicated continued support but the product roadmap is unclear.

**Strengths:** Mature MERFISH technology, automated workflow, established analysis tools. **Limitations:** Smaller panels than Xenium, uncertain commercial future under 10x ownership.

---

### CosMx SMI (Bruker / NanoString)

**How it works:** Spatial molecular imaging using fluorescently labeled probes with single-molecule detection. Uniquely supports simultaneous RNA and protein measurement on the same tissue section.

| Property | Value |
|----------|-------|
| Resolution | Subcellular (molecule-level) |
| Throughput | RNA: 1,000-6,000 genes; Protein: ~100 markers |
| Tissue type | Fresh-frozen, FFPE |
| Commercial status | **Active** (Bruker Spatial Biology, following NanoString acquisition) |

!!! tip "Recommended analysis tools"
    - Cell typing: InSituType (NanoString's probabilistic method, optimized for CosMx)
    - Spatial domains: SpaGCN, BANKSY
    - Multi-modal: RNA and protein should typically be analyzed separately, then integrated

**Strengths:** Largest gene panels of any imaging platform, RNA + protein co-detection, FFPE performance. **Limitations:** Slower imaging throughput than Xenium, NanoString-specific ecosystem creates some vendor lock-in in analysis tools.

---

### GeoMx DSP (Bruker / NanoString)

**How it works:** Region-of-interest (ROI) profiling -- the user selects areas of interest on tissue (guided by immunofluorescence), UV-cleaves photo-cleavable oligo tags from those regions, and sequences or counts the released probes.

| Property | Value |
|----------|-------|
| Resolution | ROI-level (not single-cell) |
| Throughput | Whole transcriptome or ~100 proteins per ROI |
| Tissue type | Fresh-frozen, **FFPE** (primary use case) |
| Commercial status | **Active** (Bruker Spatial Biology) |

!!! warning "Not single-cell spatial"
    GeoMx is region-level profiling, not single-cell or single-molecule spatial transcriptomics. Do not apply single-cell spatial analysis methods (segmentation, spatial domains, CCC) to GeoMx data. Analysis is closer to bulk RNA-seq with spatial annotation. Use GeomxTools (NanoString R package) for preprocessing and standard differential expression tools (DESeq2, limma) for analysis.

**Strengths:** Excellent FFPE performance (the best platform for archival clinical samples), flexible ROI selection, whole-transcriptome capability. **Limitations:** No single-cell resolution, low throughput (tens of ROIs per slide), hypothesis-driven rather than discovery.

---

### Resolve Molecular Cartography

**How it works:** Commercialization of combinatorial FISH with automated probe design and imaging. Based on padlock probe / ISS technology.

| Property | Value |
|----------|-------|
| Resolution | Single-molecule |
| Throughput | Up to ~100 genes |
| Tissue type | Fresh-frozen, FFPE |
| Commercial status | **Active** (Resolve Biosciences) |

**Strengths:** Automated probe design, FFPE compatibility. **Limitations:** Smaller gene panels than competitors, limited market share, smaller computational ecosystem.

---

## Summary comparison

| Platform | Resolution | Max genes | FFPE? | Multi-modal? | Status |
|----------|-----------|----------|-------|-------------|--------|
| MERFISH (academic) | Subcellular | 10,000+ | Limited | No | Academic |
| seqFISH+ | Subcellular | 10,000+ | No | No | Academic |
| STARmap PLUS | Subcellular | ~3,000 | No | No | Academic |
| Xenium | Subcellular | ~5,000 | Yes | No | Commercial (10x) |
| MERSCOPE | Subcellular | ~1,000 | Yes | No | Commercial (uncertain) |
| CosMx SMI | Subcellular | ~6,000 | Yes | RNA + protein | Commercial (Bruker) |
| GeoMx DSP | ROI-level | WTA | Yes | RNA or protein | Commercial (Bruker) |
| Resolve | Subcellular | ~100 | Yes | No | Commercial |
