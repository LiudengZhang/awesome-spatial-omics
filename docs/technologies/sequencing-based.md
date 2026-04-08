# Sequencing-Based Spatial Transcriptomics

Sequencing-based spatial methods capture the full transcriptome without requiring a pre-selected gene panel. They achieve this by spatially barcoding mRNA molecules on a tissue section, then reading out gene identity and location via next-generation sequencing. The trade-off: most methods capture at multi-cell or bin-level resolution, making computational deconvolution or bin-to-cell aggregation a necessary step.

---

## Visium

**How it works:** Tissue is placed on a slide printed with ~5,000 barcoded spots (55 um diameter, 100 um center-to-center), each capturing mRNA from the overlying cells via poly-dT oligos.

| Property | Value |
|----------|-------|
| Resolution | 55 um (~1-10 cells per spot) |
| Throughput | Whole transcriptome (~20,000 genes) |
| Tissue area | 6.5 mm x 6.5 mm capture area |
| Tissue type | Fresh-frozen; FFPE via CytAssist |
| Commercial status | **Active** (10x Genomics). Being superseded by Visium HD but still widely used. |
| Key paper | [Stahl et al., Science 2016](https://doi.org/10.1126/science.aaf2403); [10x Visium protocol](https://www.10xgenomics.com/products/spatial-gene-expression) |

!!! tip "Recommended analysis tools"
    - Pre-processing: SpaceRanger, SpotClean (ambient RNA correction)
    - Spatial domains: [BayesSpace](../methods/spatial-domains.md), STAGATE
    - Deconvolution: [Cell2location](../methods/deconvolution.md), RCTD, Tangram
    - SVG detection: [nnSVG](../methods/spatially-variable-genes.md), SPARK-X

**Limitations:** 55-um spots mix multiple cells, requiring deconvolution. Limited capture area restricts whole-organ studies. Sensitivity (~3,000-5,000 genes per spot) is lower than scRNA-seq.

---

## Visium HD

**How it works:** Same capture chemistry as Visium but printed at 2-um resolution (continuous lawn of barcoded oligos), with counts aggregated into 2-um or 8-um bins computationally.

| Property | Value |
|----------|-------|
| Resolution | 2 um bins (approaching single-cell) |
| Throughput | Whole transcriptome |
| Tissue area | 6.5 mm x 6.5 mm |
| Tissue type | Fresh-frozen, FFPE |
| Commercial status | **Active** (10x Genomics, launched 2024) |
| Key paper | [10x Genomics Visium HD](https://www.10xgenomics.com/products/visium-hd); [Oliveira et al., Nature 2024](https://doi.org/10.1038/s41586-024-08145-x) |

!!! tip "Recommended analysis tools"
    - Bin-to-cell: Bin2Cell, ENACT
    - Spatial domains: BANKSY, STAGATE (must scale to millions of observations)
    - Deconvolution: Cell2location, RCTD (on 8-um bins if not using Bin2Cell)
    - See the [Visium HD deep read](../deep-reads/visium-hd.md) for detailed guidance

**Limitations:** Raw 2-um bins are not cells -- aggregation is essential. Datasets are 10-100x larger than Visium, stressing memory and compute. Tooling ecosystem is still maturing. Per-bin sensitivity is low; aggregation improves it at the cost of resolution.

---

## Slide-seq / V2

**How it works:** A "puck" coated with DNA-barcoded beads (10 um diameter) is placed on a tissue section. mRNA transfers to beads, is captured, and sequenced. Bead positions are decoded by in situ sequencing.

| Property | Value |
|----------|-------|
| Resolution | 10 um (~1-2 cells per bead) |
| Throughput | Whole transcriptome |
| Tissue area | 3 mm diameter puck |
| Tissue type | Fresh-frozen only |
| Commercial status | **Academic protocol** (not commercially available) |
| Key paper | [Rodriques et al., Science 2019](https://doi.org/10.1126/science.aaw1219); [Stickels et al., Nat Biotechnol 2021](https://doi.org/10.1038/s41587-020-0739-1) (V2) |

!!! tip "Recommended analysis tools"
    - Deconvolution: RCTD (originally developed for Slide-seq), Cell2location
    - Spatial domains: BayesSpace, STAGATE

**Limitations:** Fresh-frozen only. Lower sensitivity than Visium per bead (V2 improved ~10x over V1). Small capture area limits applications. Requires custom bead synthesis and decoding.

---

## Stereo-seq

**How it works:** DNA nanoballs (DNBs) are patterned on a chip at ~500 nm spacing, creating a dense grid of spatial barcodes. Tissue is placed on the chip and captured mRNA is sequenced.

| Property | Value |
|----------|-------|
| Resolution | ~500 nm (subcellular) |
| Throughput | Whole transcriptome |
| Tissue area | Up to several centimeters |
| Tissue type | Fresh-frozen |
| Commercial status | **Active** (BGI/STOmics) |
| Key paper | [Chen et al., Cell 2022](https://doi.org/10.1016/j.cell.2022.04.003) |

!!! tip "Recommended analysis tools"
    - Initial processing: SAW (BGI pipeline)
    - Spatial domains: STAGATE, BANKSY (must handle extreme scale)
    - Deconvolution: Cell2location (on aggregated bins)

**Limitations:** Generates the largest spatial datasets of any technology -- a single sample can produce hundreds of millions of data points. Most standard tools fail at this scale without subsampling. BGI ecosystem is less integrated with the Western scverse/Bioconductor toolchains. Fresh-frozen only.

---

## Open-ST

**How it works:** Open-source protocol using commercially available reagents to create spatial barcoding arrays at subcellular resolution. Designed for accessibility -- any lab with standard molecular biology equipment can implement it.

| Property | Value |
|----------|-------|
| Resolution | Subcellular (~0.5-1 um) |
| Throughput | Whole transcriptome |
| Tissue area | Variable (depends on array) |
| Tissue type | Fresh-frozen |
| Commercial status | **Open-source** (Rajewsky lab) |
| Key paper | [Schott et al., Cell 2024](https://doi.org/10.1016/j.cell.2024.04.002) |

!!! info "Open-ST significance"
    Open-ST democratizes high-resolution spatial transcriptomics by removing the need for expensive commercial platforms. The trade-off is more hands-on protocol optimization and less turnkey support. Analysis follows patterns similar to Stereo-seq.

---

## Seq-Scope

**How it works:** Repurposes Illumina sequencing flow cells as spatial barcoding arrays, achieving subcellular resolution by leveraging the existing cluster positions on the flow cell as spatial barcodes.

| Property | Value |
|----------|-------|
| Resolution | Subcellular (~0.5-1 um) |
| Throughput | Whole transcriptome |
| Tissue area | Flow cell tile area |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic protocol** |
| Key paper | [Cho et al., Cell 2021](https://doi.org/10.1016/j.cell.2021.05.010) |

**Limitations:** Requires access to an Illumina sequencing platform for array preparation. Protocol is technically demanding. Limited adoption outside the originating lab.

---

## DBiT-seq

**How it works:** Deterministic barcoding in tissue using two sets of microfluidic channels (horizontal and vertical) that deliver DNA barcodes to create a grid of unique spatial addresses on the tissue.

| Property | Value |
|----------|-------|
| Resolution | 10 um or 25 um (determined by channel width) |
| Throughput | Whole transcriptome; also supports ATAC-seq and protein |
| Tissue area | Variable (depends on microfluidic device) |
| Tissue type | Fresh-frozen, FFPE |
| Commercial status | **Academic protocol** (Fan lab, Yale) |
| Key paper | [Liu et al., Cell 2020](https://doi.org/10.1016/j.cell.2020.09.026) |

!!! info "Multi-omic capability"
    DBiT-seq is notable as one of the earliest spatial multi-omic methods, enabling simultaneous measurement of transcriptome and chromatin accessibility (or protein) from the same tissue section. See [Spatial Multi-Omics](spatial-multiomics.md).

---

## HDST

**How it works:** High-definition spatial transcriptomics using a bead array with 2-um beads, achieving the highest resolution of early bead-based methods.

| Property | Value |
|----------|-------|
| Resolution | 2 um |
| Throughput | Whole transcriptome |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic proof-of-concept** (not widely adopted) |
| Key paper | [Vickovic et al., Nat Methods 2019](https://doi.org/10.1038/s41592-019-0548-y) |

**Limitations:** Very low capture efficiency limited practical utility. Served as an important proof-of-concept for high-resolution sequencing-based spatial transcriptomics but was superseded by Slide-seq V2, Stereo-seq, and Visium HD.

---

## Pixel-seq

**How it works:** Uses polony (polymerase colony) technology to create spatially barcoded arrays at subcellular resolution, with improved transcript capture compared to bead-based approaches.

| Property | Value |
|----------|-------|
| Resolution | Subcellular |
| Throughput | Whole transcriptome |
| Tissue type | Fresh-frozen |
| Commercial status | **Academic protocol** |
| Key paper | [Fu et al., Cell 2022](https://doi.org/10.1016/j.cell.2022.01.015) |

**Limitations:** Limited adoption. The polony-based approach is technically demanding to reproduce. Proof-of-concept data looks promising but independent validation is sparse.

---

## Summary comparison

| Technology | Resolution | Tissue area | FFPE? | Commercial? | Maturity |
|-----------|-----------|------------|-------|-------------|---------|
| Visium | 55 um | 6.5 mm | Yes | Yes | High |
| Visium HD | 2 um | 6.5 mm | Yes | Yes | Medium |
| Slide-seq V2 | 10 um | 3 mm | No | No | Medium |
| Stereo-seq | 500 nm | cm-scale | No | Yes | Medium |
| Open-ST | Subcellular | Variable | No | Open-source | Early |
| Seq-Scope | Subcellular | Flow cell | No | No | Early |
| DBiT-seq | 10-25 um | Variable | Yes | No | Early |
| HDST | 2 um | Small | No | No | Superseded |
| Pixel-seq | Subcellular | Variable | No | No | Early |
