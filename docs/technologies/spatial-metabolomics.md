# Spatial Metabolomics

Spatial metabolomics technologies map the distribution of metabolites, lipids, and small molecules directly in tissue sections using mass spectrometry imaging (MSI). Unlike spatial transcriptomics and proteomics, metabolomics is inherently **untargeted** -- no probes or antibodies are needed, and the detectable analytes are determined by the ionization method and mass analyzer rather than a pre-designed panel.

!!! info "Fundamentally different data"
    Spatial metabolomics data is structurally different from spatial transcriptomics. There are no cells, no genes, no count matrices. The output is a spatial map of mass-to-charge (m/z) peaks that must be annotated against metabolite databases. Most spatial transcriptomics analysis tools do not apply. Dedicated MSI analysis software (e.g., SCiLS, Cardinal, MSiReader) is required.

---

## MALDI-MSI (Matrix-Assisted Laser Desorption/Ionization)

**How it works:** A UV-absorbing matrix is applied to the tissue surface. A focused laser beam is rastered across the tissue, desorbing and ionizing analytes at each position. A mass spectrum is recorded at each pixel, generating a spatial map of metabolite distributions.

| Property | Value |
|----------|-------|
| Resolution | 5-50 um (depending on laser optics and matrix application) |
| Detectable analytes | Lipids, small metabolites, peptides, drugs, glycans |
| Tissue type | Fresh-frozen, FFPE (with deparaffinization) |
| Instrument | Bruker rapifleX, timsTOF fleX; Waters SYNAPT; others |
| Commercial status | **Active** (multiple vendors: Bruker, Waters, Shimadzu, JEOL) |
| Key paper | [Caprioli et al., Anal Chem 1997](https://doi.org/10.1021/ac971095b) |

!!! tip "Most established spatial metabolomics method"
    MALDI-MSI is the most widely used and mature spatial metabolomics approach, with decades of method development and a large user community. It is the default choice for most spatial metabolomics experiments.

**Strengths:** High spatial resolution achievable (down to ~5 um with oversampling), untargeted detection, compatible with subsequent H&E staining on the same section, large installed instrument base. **Limitations:** Matrix application is critical and can introduce artifacts, ion suppression effects limit detection sensitivity, metabolite identification requires MS/MS and database matching, quantification is challenging.

---

## DESI-MSI (Desorption Electrospray Ionization)

**How it works:** A charged solvent spray is directed at the tissue surface under ambient conditions. The spray desorbs and ionizes surface analytes, which are collected by a mass spectrometer inlet. No matrix application is needed.

| Property | Value |
|----------|-------|
| Resolution | 50-200 um |
| Detectable analytes | Lipids, small metabolites, drugs |
| Tissue type | Fresh-frozen (primarily), thin sections |
| Instrument | Waters (primary vendor for DESI sources) |
| Commercial status | **Active** (Waters, as part of DESI-XS) |
| Key paper | [Takats et al., Science 2004](https://doi.org/10.1126/science.1104404) |

**Strengths:** Ambient ionization (no sample preparation needed), can be applied to native tissue, no matrix artifacts, compatible with downstream histology. **Limitations:** Lower spatial resolution than MALDI (typically ~100-200 um), lower sensitivity for some analyte classes, smaller user community than MALDI.

---

## SpaceM

**How it works:** Integrates single-cell metabolomics with microscopy by coupling MALDI-MSI with fluorescence imaging of individual cells. Cells are first imaged (fluorescence for morphology/markers), then the same sample is subjected to MALDI-MSI at single-cell resolution. Computational registration links metabolite profiles to individual cells.

| Property | Value |
|----------|-------|
| Resolution | Single-cell (by combining MALDI with cell segmentation from fluorescence) |
| Detectable analytes | Lipids, metabolites |
| Tissue type | Cell monolayers, thin tissue sections |
| Commercial status | **Academic** (Alexandrov lab, EMBL) |
| Key paper | [Rappez et al., Nat Methods 2021](https://doi.org/10.1038/s41592-021-01198-0) |

!!! info "Bridging metabolomics and single-cell biology"
    SpaceM is notable as one of the few methods that connects spatial metabolomics to the single-cell resolution paradigm. By integrating cell segmentation from fluorescence images with MALDI-MSI data, it enables analysis of metabolic heterogeneity at the single-cell level. However, the sensitivity per cell is limited and the method works best on flat cell monolayers.

**Limitations:** Works best on cultured cells or flat tissue sections (not thick 3D tissues), requires careful alignment between microscopy and MSI modalities, limited metabolite coverage per cell.

---

## NanoSIMS (Nanoscale Secondary Ion Mass Spectrometry)

**How it works:** A focused primary ion beam (typically Cs+ or O-) sputters the tissue surface at nanometer-scale resolution, and secondary ions are analyzed by a magnetic sector mass spectrometer. Achieves the highest spatial resolution of any MSI technique.

| Property | Value |
|----------|-------|
| Resolution | 50-100 nm (nanoscale) |
| Detectable analytes | Elemental and isotopic composition (not intact metabolites) |
| Tissue type | Resin-embedded, thin sections |
| Instrument | CAMECA NanoSIMS 50/50L |
| Commercial status | **Active** (CAMECA/AMETEK) |

!!! warning "Different from molecular MSI"
    NanoSIMS measures elemental and isotopic composition, not intact metabolite molecules. It is used for isotope tracing (e.g., ^13^C, ^15^N labeling) to track metabolic flux at subcellular resolution, rather than untargeted metabolite identification. This makes it complementary to, rather than competitive with, MALDI and DESI.

**Strengths:** Nanoscale spatial resolution (the highest of any MSI method), quantitative isotope ratio measurements, enables subcellular metabolic flux studies. **Limitations:** Destructive, requires specialized sample preparation (resin embedding), limited to elemental/isotopic analysis (no intact metabolite detection), expensive instruments with limited availability, low throughput.

---

## AP-MALDI (Atmospheric Pressure MALDI)

**How it works:** MALDI performed at atmospheric pressure rather than in vacuum, enabling coupling to diverse mass analyzers (Orbitraps, Q-TOFs) and achieving high mass resolution and accuracy for metabolite identification.

| Property | Value |
|----------|-------|
| Resolution | 5-20 um |
| Detectable analytes | Lipids, metabolites, drugs |
| Tissue type | Fresh-frozen, FFPE |
| Instrument | AP-MALDI sources (TransMIT, MassTech) coupled to high-resolution MS |
| Commercial status | **Active** (niche vendors: TransMIT, MassTech) |

**Strengths:** High mass resolution and accuracy (when coupled to Orbitrap), better metabolite identification than vacuum MALDI-TOF, atmospheric pressure operation simplifies some workflows. **Limitations:** Niche market with limited vendor support, slower acquisition than vacuum MALDI, requires high-resolution mass analyzer.

---

## LAESI (Laser Ablation Electrospray Ionization)

**How it works:** A mid-infrared laser ablates tissue at ambient conditions (exploiting water absorption at ~2.94 um), and the ablated material is captured by an electrospray plume for ionization and mass analysis.

| Property | Value |
|----------|-------|
| Resolution | ~100-200 um |
| Detectable analytes | Metabolites, lipids, some proteins |
| Tissue type | Fresh, hydrated tissue (no sectioning required in some configurations) |
| Instrument | Protea Biosciences (original); now limited availability |
| Commercial status | **Limited** (Protea Biosciences ceased operations; academic implementations continue) |
| Key paper | [Nemes & Vertes, Anal Chem 2007](https://doi.org/10.1021/ac071106z) |

**Strengths:** Ambient ionization, can analyze fresh hydrated tissue without sectioning, no matrix needed. **Limitations:** Low spatial resolution, limited commercial support after Protea Biosciences closure, lower sensitivity than MALDI.

---

## Analysis tools for spatial metabolomics

| Tool | Description | Platform |
|------|------------|----------|
| **SCiLS Lab** | Commercial software for MSI data visualization, statistical analysis, and classification | Windows (Bruker) |
| **Cardinal** | R/Bioconductor package for statistical analysis of MSI data | R |
| **MSiReader** | Free MATLAB-based tool for MSI data visualization and analysis | MATLAB |
| **SpectralAnalysis** | Open-source MATLAB toolbox for MSI | MATLAB |
| **pyMSpec** | Python tools for mass spectrometry imaging | Python |
| **METASPACE** | Cloud platform for metabolite annotation of MSI data using molecular databases | Web |

!!! warning "Metabolite identification is the bottleneck"
    Unlike transcriptomics where genes are well-cataloged, metabolite identification from m/z peaks remains a major challenge. Database coverage is incomplete (especially for lipids and secondary metabolites), isomeric compounds share the same mass, and ion suppression effects mean absence of a peak does not mean absence of the metabolite. Always validate key findings with orthogonal methods (LC-MS/MS, standards).

---

## Summary comparison

| Technology | Resolution | Analytes | Sample prep | Matrix needed? | Status |
|-----------|-----------|---------|------------|---------------|--------|
| MALDI-MSI | 5-50 um | Lipids, metabolites, peptides | Matrix coating | Yes | Established |
| DESI-MSI | 50-200 um | Lipids, metabolites | None (ambient) | No | Active |
| SpaceM | Single-cell | Lipids, metabolites | Matrix + fluorescence | Yes (MALDI step) | Academic |
| NanoSIMS | 50-100 nm | Elements, isotopes | Resin embedding | No | Active (niche) |
| AP-MALDI | 5-20 um | Lipids, metabolites | Matrix coating | Yes | Active (niche) |
| LAESI | 100-200 um | Metabolites, lipids | None (ambient) | No | Limited |
