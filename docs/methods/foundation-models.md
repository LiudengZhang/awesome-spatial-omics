# Foundation Models & AI Agents

**Pipeline question:** Can large pretrained models — trained on millions of cells or tissue images — provide general-purpose spatial omics analysis without task-specific training?

## Overview

Foundation models represent the newest frontier in spatial omics, applying the large-scale pretraining paradigm (successful in NLP and computer vision) to biological data. These models learn general representations from massive datasets and can be fine-tuned or prompted for specific downstream tasks. The field includes both tissue-level foundation models trained on spatial expression data and cell segmentation models built on vision foundation architectures. Most were published in 2024-2025, and community validation is still limited.

## Tissue-Level Foundation Models

### Nicheformer
- **Paper:** [Nature, 2025](https://doi.org/10.1038/s41586-025-08832-9)
- **Code:** [github.com/theislab/nicheformer](https://github.com/theislab/nicheformer)
- **Key innovation:** Transformer foundation model pretrained on millions of single cells and spatial cells, learning niche-aware cellular representations that encode both cell state and spatial context.
- **Strengths:**
    - Largest spatial foundation model — pretrained on 110M+ cells
    - Jointly models cellular identity and spatial niche context
    - Zero-shot and few-shot transfer to new tissues
- **Limitations:**
    - Very large model requiring significant compute for inference
    - Pretraining data bias toward well-studied tissues
    - Published 2025 — limited independent validation
- **Technology compatibility:** Visium, MERFISH, Xenium, Slide-seq, Stereo-seq

### Novae
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.09.09.611346)
- **Code:** [github.com/MICS-Lab/novae](https://github.com/MICS-Lab/novae)
- **Key innovation:** Graph-based foundation model for spatial domain identification that uses self-supervised contrastive learning on spatial neighborhood graphs.
- **Strengths:**
    - Pretrained model enables zero-shot spatial domain detection
    - Graph-based architecture naturally handles irregular cell positions
    - Lightweight compared to large transformer FMs
- **Limitations:**
    - Focused on domain detection — less general-purpose than Nicheformer
    - Preprint stage
- **Technology compatibility:** Xenium, MERFISH, CosMx, any cell-resolution spatial platform

### DECIPHER
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.04.02.587625)
- **Code:** [github.com/theislab/DECIPHER](https://github.com/theislab/DECIPHER)
- **Key innovation:** Variational autoencoder that learns interpretable spatial representations, decomposing tissue organization into overlapping spatial programs.
- **Strengths:**
    - Interpretable latent factors correspond to biological programs
    - Handles overlapping tissue structures (unlike discrete domain methods)
    - Part of the scverse ecosystem
- **Limitations:**
    - VAE architecture may not capture long-range spatial dependencies
    - Not a true foundation model — trained per dataset
- **Technology compatibility:** Visium, Slide-seq, MERFISH

### SpatialFusion
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.10.11.617889)
- **Code:** [github.com/SpatialFusion-team/SpatialFusion](https://github.com/SpatialFusion-team/SpatialFusion)
- **Key innovation:** Multi-modal fusion framework that combines spatial transcriptomics with histology images through a shared foundation model architecture.
- **Strengths:**
    - Fuses molecular and image data in a unified model
    - Leverages pretrained vision models for histology understanding
- **Limitations:**
    - Preprint with limited validation
    - Multi-modal fusion adds complexity
- **Technology compatibility:** Visium (with H&E), any platform with paired histology

## Cell Segmentation Foundation Models

### CellSAM
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2023.11.17.567630)
- **Code:** [github.com/vanvalenlab/cellSAM](https://github.com/vanvalenlab/cellSAM)
- **Key innovation:** Adapts the Segment Anything Model (SAM) for cell segmentation, achieving strong zero-shot generalization across imaging modalities.
- **Strengths:**
    - Foundation model generalization — works across microscopy modalities
    - Minimal fine-tuning needed for new tissue types
    - Leverages massive vision foundation model
- **Limitations:**
    - Large model with high compute requirements
    - Performance varies with cell density
- **Technology compatibility:** Any platform with imaging data (Xenium, MERFISH, CosMx, CODEX)

See [Cell Segmentation](cell-segmentation.md) for a full discussion of segmentation methods.

## AI Agents for Spatial Analysis

### ChatSpatial
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.09.23.614587)
- **Code:** [github.com/LLM-for-spatial/ChatSpatial](https://github.com/LLM-for-spatial/ChatSpatial)
- **Key innovation:** LLM-powered conversational agent that interprets spatial omics data through natural language, enabling non-expert users to perform spatial analyses via chat.
- **Strengths:**
    - Natural language interface lowers the barrier to spatial analysis
    - Integrates multiple spatial analysis tools behind a chat interface
- **Limitations:**
    - LLM hallucination risk for statistical claims
    - Limited to analyses the underlying tools support
    - Still experimental — not recommended for production research
- **Technology compatibility:** Visium, MERFISH (via integrated tools)

### SpatialAgent
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.12.07.627357)
- **Code:** [github.com/Genentech/SpatialAgent](https://github.com/Genentech/SpatialAgent)
- **Key innovation:** LangGraph-based AI agent that autonomously plans and executes multi-step spatial omics analysis workflows using tool-calling and code generation.
- **Strengths:**
    - Autonomous multi-step analysis pipeline construction
    - Integrates dataset search, preprocessing, and analysis
    - Built on modern agentic AI framework (LangGraph)
- **Limitations:**
    - Agent reliability depends on LLM reasoning quality
    - Output requires expert validation
    - Very early stage — zero external users at time of publication
- **Technology compatibility:** Any platform accessible through CZ CELLxGENE

### CELLama
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.05.03.592369)
- **Code:** [github.com/cellama/CELLama](https://github.com/cellama/CELLama)
- **Key innovation:** LLM-based cell-type annotation that uses language models to interpret marker gene lists and assign cell-type labels, replacing manual curation.
- **Strengths:**
    - Automates the manual step of interpreting marker genes
    - Can leverage LLM knowledge of cell biology
    - Works with any marker gene input, including from spatial data
- **Limitations:**
    - LLM biological knowledge may be outdated or incorrect
    - Not spatial-aware — uses marker lists, not spatial patterns
    - Annotation quality is hard to validate systematically
- **Technology compatibility:** Any platform (technology-agnostic — operates on marker gene lists)

## Benchmark Summary

Foundation models for spatial omics are too new for meaningful benchmarks. **Nicheformer** is the most ambitious tissue-level FM, but independent validation is needed to assess whether pretraining truly generalizes across tissues and conditions. For cell segmentation, **CellSAM** demonstrates that vision foundation models transfer well to biological imaging. AI agents (**SpatialAgent**, **ChatSpatial**) are experimental tools that lower barriers to entry but introduce LLM reliability concerns for scientific analysis.

!!! warning "Foundation models are not validated replacements"
    These models are promising but have not been independently validated at scale. For publication-grade analyses, established task-specific methods (Cellpose for segmentation, Cell2location for deconvolution, etc.) remain the safer choice. Use foundation models for exploration, hypothesis generation, and efficiency gains.

!!! tip "AI agents require expert oversight"
    ChatSpatial and SpatialAgent can accelerate exploratory analysis, but every result they produce must be critically evaluated by a domain expert. LLMs generate plausible-looking but sometimes incorrect biological interpretations.

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| Large spatial dataset | General-purpose embeddings | Nicheformer | Pretrained on 110M+ cells with spatial context |
| Cell-resolution spatial | Zero-shot domain detection | Novae | Lightweight pretrained graph model |
| Any spatial data | Interpretable spatial programs | DECIPHER | VAE with interpretable latent factors |
| Any imaging data | Zero-shot cell segmentation | CellSAM | SAM-based foundation model for cells |
| Exploratory analysis | Rapid prototyping via chat | SpatialAgent or ChatSpatial | Natural language interface to analysis tools |
| Marker gene lists | Automated cell-type annotation | CELLama | LLM-based marker interpretation |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| Nicheformer | Yes | - | Yes | Yes | - | - | Yes |
| Novae | - | - | Yes | Yes | Yes | - | - |
| DECIPHER | Yes | - | - | Yes | - | - | - |
| CellSAM | - | - | Yes | Yes | Yes | Yes | - |
| SpatialFusion | Yes | - | - | - | - | - | - |
| ChatSpatial | Yes | - | - | Yes | - | - | - |
| SpatialAgent | Yes | - | - | - | - | - | - |
| CELLama | Yes | - | Yes | Yes | Yes | - | - |
