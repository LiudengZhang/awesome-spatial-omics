# Spatial Proteomics

Spatial proteomics technologies measure protein abundance and localization in tissue sections using antibody-based detection. These methods provide fundamentally different information than spatial transcriptomics: protein levels often diverge from mRNA levels, post-translational modifications cannot be captured by RNA methods, and surface markers critical for immune phenotyping are best measured directly at the protein level.

The trade-off: antibody-based methods are limited to **~40-100 markers** per experiment, constrained by antibody availability, cross-reactivity, and multiplexing capacity.

---

## CODEX / PhenoCycler (Akoya Biosciences)

**How it works:** Iterative fluorescent antibody staining using DNA-conjugated antibodies. In each cycle, fluorescently-labeled DNA probes hybridize to the antibody-conjugated oligos, images are captured, probes are stripped, and new probes are applied. This enables high-plex imaging on standard fluorescence microscopes.

| Property | Value |
|----------|-------|
| Resolution | Subcellular (~200-400 nm) |
| Throughput | Up to ~100 protein markers |
| Tissue type | Fresh-frozen, FFPE |
| Instrument | Standard fluorescence microscope + PhenoCycler system |
| Commercial status | **Active** (Akoya Biosciences) |
| Key paper | [Goltsev et al., Cell 2018](https://doi.org/10.1016/j.cell.2018.07.010) (CODEX); [Black et al., Nat Biotechnol 2021](https://doi.org/10.1038/s41587-021-00940-5) |

!!! tip "Recommended analysis tools"
    - Segmentation: [Mesmer/DeepCell](../methods/cell-segmentation.md) (benchmark leader for protein imaging)
    - Phenotyping: FlowSOM, Leiden clustering on marker intensities
    - Neighborhood analysis: Squidpy, ATHENA
    - Spatial statistics: Squidpy (co-occurrence, interaction scores)

**Strengths:** Uses standard microscopes (lower instrument cost than competitors), high-plex protein imaging, iterative staining is gentle on tissue, FFPE compatible. **Limitations:** Imaging throughput limited by number of cycles, Akoya's financial position creates platform risk, antibody validation is time-consuming.

!!! warning "Akoya financial concerns"
    Akoya Biosciences has faced financial pressure since 2024. While PhenoCycler remains commercially available and technically sound, long-term platform support is uncertain. Factor vendor viability into purchasing decisions for new instruments.

---

## MIBI / MIBIscope (Ionpath) -- DISCONTINUED

**How it works:** Multiplexed ion beam imaging uses metal-tagged antibodies detected by secondary ion mass spectrometry (SIMS). A focused ion beam rastered across the tissue surface releases secondary ions from metal-conjugated antibodies, which are identified by time-of-flight mass spectrometry.

| Property | Value |
|----------|-------|
| Resolution | ~260 nm (subcellular) |
| Throughput | ~40 protein markers simultaneously |
| Tissue type | Fresh-frozen, FFPE |
| Commercial status | **Discontinued** (Ionpath shut down in 2023) |
| Key paper | [Angelo et al., Nat Med 2014](https://doi.org/10.1038/nm.3488); [Keren et al., Cell 2018](https://doi.org/10.1016/j.cell.2018.08.039) |

!!! danger "Ionpath shutdown"
    Ionpath ceased operations in 2023. MIBIscope instruments are no longer manufactured or commercially supported. Existing instruments continue to operate, and the analytical approaches remain relevant, but new instrument purchases are not possible. This is a cautionary example of vendor risk in the spatial omics field.

    Researchers with existing MIBI data should continue using established analysis workflows. The metal-tagged antibody approach lives on through IMC (see below).

**Strengths (historical):** Highest spatial resolution of any protein method, simultaneous detection (no iterative staining), no autofluorescence issues (mass-based detection). **Limitations:** Very slow imaging (~hours per mm^2), expensive instrument, now discontinued.

---

## IMC / Hyperion (Standard BioTools)

**How it works:** Imaging mass cytometry uses metal-tagged antibodies (same as CyTOF/mass cytometry) detected by laser ablation coupled to CyTOF mass spectrometry. A UV laser ablates tissue pixel-by-pixel, and the metal content of each ablated spot is quantified by time-of-flight mass spectrometry.

| Property | Value |
|----------|-------|
| Resolution | 1 um |
| Throughput | ~40 protein markers simultaneously |
| Tissue type | Fresh-frozen, FFPE |
| Instrument | Hyperion XTi (Standard BioTools) |
| Commercial status | **Active** (Standard BioTools, formerly Fluidigm) |
| Key paper | [Giesen et al., Nat Methods 2014](https://doi.org/10.1038/nmeth.2869) |

!!! tip "Recommended analysis tools"
    - Segmentation: Mesmer/DeepCell, ilastik (pixel classification)
    - Phenotyping: FlowSOM, Phenograph, manual gating
    - Analysis frameworks: steinbock, imcRtools (Bioconductor)
    - Spatial analysis: Squidpy, imcRtools

**Strengths:** Simultaneous multi-marker detection (no spectral overlap), FFPE compatible, well-established in immuno-oncology, growing antibody panel availability, active vendor support. **Limitations:** Slow imaging (~1 mm^2/hour), destructive (tissue is ablated), lower resolution than MIBI or fluorescence-based methods.

---

## t-CyCIF (Tissue-based Cyclic Immunofluorescence)

**How it works:** Iterative immunofluorescence using standard primary antibodies. After each imaging round, fluorophores are bleached or inactivated, and new antibodies are applied. Uses standard microscopy equipment without specialized conjugation chemistry.

| Property | Value |
|----------|-------|
| Resolution | Subcellular (~300 nm, diffraction-limited) |
| Throughput | ~20-60 protein markers |
| Tissue type | Fresh-frozen, FFPE |
| Instrument | Standard fluorescence or confocal microscope |
| Commercial status | **Academic protocol** (Sorger lab, Harvard) |
| Key paper | [Lin et al., eLife 2018](https://doi.org/10.7554/eLife.31657) |

**Strengths:** Uses standard antibodies (no conjugation needed), works on any fluorescence microscope, FFPE compatible, extensively validated in clinical research (HTAN project). **Limitations:** Fluorophore bleaching can cause signal loss in later rounds, limited to ~4 markers per round (fluorescence channels), tissue damage accumulates over cycles.

!!! info "Clinical validation"
    t-CyCIF has been extensively used in the Human Tumor Atlas Network (HTAN) project for characterizing tumor microenvironments across cancer types, providing one of the largest validated spatial proteomics datasets available.

---

## 4i (Iterative Indirect Immunofluorescence Imaging)

**How it works:** Iterative staining and imaging using indirect immunofluorescence (primary + secondary antibodies), with antibody elution between rounds. Enables high-plex protein imaging with standard antibodies.

| Property | Value |
|----------|-------|
| Resolution | Subcellular |
| Throughput | ~40+ protein markers |
| Tissue type | Cultured cells, tissue sections |
| Commercial status | **Academic protocol** (Pelkmans lab, University of Zurich) |
| Key paper | [Gut et al., Science 2018](https://doi.org/10.1126/science.aar7042) |

**Strengths:** Uses off-the-shelf primary antibodies, amplification from secondary antibodies improves signal, well-suited for cell biology studies. **Limitations:** Originally optimized for cultured cells (tissue applications require more optimization), antibody elution efficiency varies, long protocol.

---

## mIHC / mIF (Multiplex Immunohistochemistry / Immunofluorescence)

**How it works:** Multiplex chromogenic or fluorescent immunostaining using tyramide signal amplification (TSA) or similar chemistry. Multiple markers are stained sequentially on the same slide, with each round using a different fluorophore/chromogen.

| Property | Value |
|----------|-------|
| Resolution | Subcellular (fluorescence) to cellular (chromogenic) |
| Throughput | 5-12 protein markers (typical); up to ~30 with automation |
| Tissue type | FFPE (primary use case) |
| Instrument | Standard brightfield or fluorescence microscope; automated platforms (Akoya Vectra, Leica Bond) |
| Commercial status | **Active** (multiple vendors: Akoya, Leica, Roche) |

**Strengths:** Closest to standard clinical pathology workflows, FFPE optimized, strong clinical adoption, regulatory familiarity. **Limitations:** Low plex compared to other spatial proteomics methods, spectral unmixing challenges with more markers, marker order can affect results.

!!! info "Clinical relevance"
    mIHC/mIF is the most clinically adopted form of spatial proteomics, used in companion diagnostic development and immuno-oncology clinical trials. While the plex is lower than research-grade methods, its compatibility with clinical workflows makes it uniquely positioned for translational research.

---

## Summary comparison

| Technology | Resolution | Max markers | Simultaneous? | FFPE? | Status |
|-----------|-----------|------------|--------------|-------|--------|
| PhenoCycler (CODEX) | ~200-400 nm | ~100 | No (iterative) | Yes | Active (Akoya) |
| MIBI/MIBIscope | ~260 nm | ~40 | Yes | Yes | **Discontinued** |
| IMC/Hyperion | 1 um | ~40 | Yes | Yes | Active (Standard BioTools) |
| t-CyCIF | ~300 nm | ~60 | No (iterative) | Yes | Academic |
| 4i | Subcellular | ~40 | No (iterative) | Limited | Academic |
| mIHC/mIF | Cellular-subcellular | 5-30 | No (iterative) | Yes | Clinical standard |

---

## Choosing a spatial proteomics method

| Need | Recommendation |
|------|---------------|
| Highest plex | PhenoCycler (~100 markers) |
| Highest resolution | MIBI (discontinued) or PhenoCycler |
| Simultaneous detection (no iterative artifacts) | IMC/Hyperion |
| Standard microscope, no special instrument | t-CyCIF or mIHC/mIF |
| Clinical / translational FFPE | mIHC/mIF or IMC |
| Combined RNA + protein | CosMx SMI (see [Imaging-Based](imaging-based.md)) |
