# Spatial Multi-Omics

Spatial multi-omics technologies measure two or more molecular modalities (transcriptome, epigenome, proteome) from the same tissue section with spatial resolution. This is one of the fastest-moving areas in spatial biology, with most methods still in early-stage development and limited computational tool support.

!!! info "Emerging field"
    Most spatial multi-omic methods have been published in the last 2-3 years. Computational ecosystems are sparse, and benchmarks are largely absent. Expect rapid evolution in both technologies and analysis approaches. For most practical applications today, sequential profiling of separate sections (e.g., Visium + ATAC-seq on adjacent sections) remains more reliable than true multi-omic methods.

---

## Spatial ATAC-seq (Spatial Chromatin Accessibility)

### Spatial ATAC-seq

**How it works:** Combines the Tn5 transposase-based ATAC-seq protocol with spatial barcoding, enabling genome-wide chromatin accessibility profiling with spatial coordinates.

| Property | Value |
|----------|-------|
| Modalities | Chromatin accessibility |
| Resolution | 20 um (pixel-level) |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic** |
| Key paper | [Deng et al., bioRxiv 2022](https://doi.org/10.1101/2022.06.06.494972) |

**Limitations:** Lower data quality than standard scATAC-seq due to tissue-based processing. Spatial resolution is coarser than transcriptomic counterparts. Limited downstream analysis tools -- most users adapt scATAC-seq tools (ArchR, Signac) with manual spatial integration.

---

### Spatial CUT&Tag

**How it works:** Adapts CUT&Tag (Cleavage Under Targets and Tagmentation) for spatial profiling of histone modifications. Uses antibody-guided Tn5 transposase to tag specific chromatin modifications, combined with spatial barcoding.

| Property | Value |
|----------|-------|
| Modalities | Histone modifications (e.g., H3K27me3, H3K4me3, H3K27ac) |
| Resolution | 20 um (pixel-level) |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic** (Bhatt/Wu lab) |
| Key paper | [Deng et al., Science 2022](https://doi.org/10.1126/science.abg7216) |

!!! tip "Unique capability"
    Spatial CUT&Tag is currently the only method that provides spatially resolved histone modification data, enabling study of epigenetic regulation in tissue context. This is particularly valuable for developmental biology and cancer epigenetics.

**Limitations:** Each experiment profiles one histone mark at a time (though sequential experiments on adjacent sections can cover multiple marks). Spatial resolution is limited. Data analysis typically uses custom pipelines adapting scCUT&Tag workflows.

---

## Spatial Transcriptome + Barcode Methods

### Slide-tags

**How it works:** Combines spatial barcoding with single-nucleus RNA-seq. Nuclei are tagged with spatial barcodes in situ, then dissociated and processed through standard snRNA-seq. This assigns spatial coordinates to single-cell transcriptomic profiles.

| Property | Value |
|----------|-------|
| Modalities | Transcriptome (single-nucleus resolution with spatial coordinates) |
| Resolution | Single-cell (after demultiplexing) |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic** (Bhatt/Macosko lab) |
| Key paper | [Russell et al., Nature 2024](https://doi.org/10.1038/s41586-023-06837-4) |

**Strengths:** Achieves true single-cell resolution with whole-transcriptome coverage by combining spatial barcoding with high-quality snRNA-seq. **Limitations:** Requires dissociation (loses tissue context for imaging), spatial resolution depends on barcode density, fresh-frozen only.

---

### SPOTS (Spatial Protein and Transcriptome Sequencing)

**How it works:** Extends Visium-style spatial transcriptomics to simultaneously capture both mRNA and surface protein (via DNA-conjugated antibodies, similar to CITE-seq) from the same tissue spots.

| Property | Value |
|----------|-------|
| Modalities | Transcriptome + surface proteins |
| Resolution | 55 um (Visium spot level) |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic** |
| Key paper | [Ben-Chetrit et al., Nat Biotechnol 2023](https://doi.org/10.1038/s41587-022-01536-3) |

**Strengths:** Simple extension of existing Visium workflow, joint RNA + protein in the same spot. **Limitations:** Same 55-um resolution limitation as Visium, limited to surface proteins with available antibody-oligo conjugates, fresh-frozen only.

---

### SM-Omics (Spatial Multi-Omics)

**How it works:** Combines spatial transcriptomics with surface protein detection on the same tissue section, using a modified Visium-like workflow with antibody-derived tags.

| Property | Value |
|----------|-------|
| Modalities | Transcriptome + surface proteins |
| Resolution | ~100 um |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic** |
| Key paper | [Fang et al., bioRxiv 2022](https://doi.org/10.1101/2022.08.05.502989) |

**Limitations:** Lower resolution than Visium, limited adoption, overlapping scope with SPOTS.

---

## Microfluidic Multi-Omics

### DBiT-seq (Multi-omic mode)

**How it works:** The same microfluidic barcoding approach used for spatial transcriptomics (see [Sequencing-Based](sequencing-based.md)) can be applied to simultaneously measure transcriptome and chromatin accessibility, or transcriptome and protein, by modifying the capture chemistry.

| Property | Value |
|----------|-------|
| Modalities | Transcriptome + ATAC, or Transcriptome + Protein |
| Resolution | 10-25 um |
| Tissue type | Fresh-frozen, FFPE |
| Commercial status | **Academic** (Fan lab, Yale) |
| Key paper | [Liu et al., Cell 2020](https://doi.org/10.1016/j.cell.2020.09.026) |

**Strengths:** True multi-omic from the same section, FFPE compatible, flexible chemistry. **Limitations:** Microfluidic fabrication limits scalability, small tissue areas, limited computational tools for joint analysis.

---

### MISAR-seq

**How it works:** Microfluidics-based spatial multi-omic method combining transcriptome and chromatin accessibility measurements using a deterministic barcoding strategy similar to DBiT-seq.

| Property | Value |
|----------|-------|
| Modalities | Transcriptome + chromatin accessibility |
| Resolution | 50 um |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic** |
| Key paper | [Zhang et al., bioRxiv 2023](https://doi.org/10.1101/2023.01.07.522760) |

**Limitations:** Early-stage, limited validation data, coarser resolution than DBiT-seq.

---

## Commercial Multi-Omic Platforms

### Visium CytAssist

**How it works:** 10x Genomics CytAssist device enables Visium and Visium HD workflows on FFPE tissue sections, including tissue that has been previously stained with H&E or immunofluorescence. While not multi-omic in the strictest sense, CytAssist enables sequential imaging + spatial transcriptomics on the same section.

| Property | Value |
|----------|-------|
| Modalities | Transcriptome + histological imaging (H&E or IF) |
| Resolution | 55 um (Visium) or 2 um (Visium HD) |
| Tissue type | FFPE, fresh-frozen |
| Commercial status | **Active** (10x Genomics) |

**Strengths:** FFPE compatibility, leverages existing histology workflows, compatible with both Visium and Visium HD. **Limitations:** The protein information comes from standard IF (not high-plex), so the multi-omic aspect is limited compared to true joint profiling methods.

### CosMx SMI (Multi-modal mode)

CosMx supports simultaneous RNA and protein detection on the same tissue section. See [Imaging-Based Technologies](imaging-based.md) for details.

---

## Analysis considerations

!!! warning "Computational tools lag behind technology"
    The computational ecosystem for spatial multi-omics is substantially less mature than for spatial transcriptomics alone. Key gaps include:

    - **Joint embedding methods**: Few tools can jointly embed spatial transcriptome and epigenome data. Most users process each modality separately and integrate post-hoc using tools like MOFA+ or WNN (Seurat).
    - **Spatial epigenome analysis**: No spatial-aware tools exist specifically for spatial ATAC-seq or CUT&Tag data. Users adapt standard scATAC-seq tools (ArchR, Signac, SnapATAC2) and overlay spatial coordinates manually.
    - **Benchmarks**: No systematic benchmarks exist for spatial multi-omic integration methods.
    - **Data structures**: SpatialData and AnnData can accommodate multi-modal data but workflows are not yet standardized.

---

## Summary comparison

| Technology | Modalities | Resolution | FFPE? | Maturity |
|-----------|-----------|-----------|-------|---------|
| Spatial ATAC-seq | Chromatin accessibility | 20 um | No | Early |
| Spatial CUT&Tag | Histone modifications | 20 um | No | Early |
| Slide-tags | Transcriptome (spatial barcoded snRNA-seq) | Single-cell | No | Medium |
| SPOTS | Transcriptome + protein | 55 um | No | Early |
| SM-Omics | Transcriptome + protein | ~100 um | No | Early |
| DBiT-seq | Transcriptome + ATAC or protein | 10-25 um | Yes | Medium |
| MISAR-seq | Transcriptome + chromatin | 50 um | No | Early |
| Visium CytAssist | Transcriptome + IF imaging | 55 um / 2 um | Yes | High |
| CosMx SMI | RNA + protein | Subcellular | Yes | Medium |
