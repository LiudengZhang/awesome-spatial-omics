# Spatial Trajectories & Dynamics

**Pipeline question:** How does spatial organization relate to developmental or dynamic processes — can we trace pseudotemporal trajectories through physical tissue space?

## Overview

Spatial trajectory analysis connects the spatial arrangement of cells in tissue to temporal or developmental processes. In standard scRNA-seq, pseudotime orders cells along inferred developmental trajectories but discards spatial information. Spatial trajectory methods recover this missing dimension — identifying whether developmental progressions follow spatially coherent paths through tissue (e.g., crypt-to-villus in intestine, or zonation in liver). This emerging field bridges spatial organization and temporal dynamics.

## Key Methods

### spaTrack
- **Paper:** [Nature Methods, 2024](https://doi.org/10.1038/s41592-024-02408-1)
- **Code:** [github.com/ybli/spaTrack](https://github.com/ybli/spaTrack)
- **Key innovation:** Optimal transport framework that infers cell state transitions and spatial trajectories by tracking cell movements across spatial transcriptomics data.
- **Strengths:**
    - Principled optimal transport formulation
    - Infers both spatial and transcriptomic trajectories jointly
    - Supports multi-timepoint spatial data when available
- **Limitations:**
    - Optimal transport assumptions may not hold for all biological transitions
    - Requires sufficient density of cells along the trajectory
- **Technology compatibility:** MERFISH, Xenium, Slide-seq, Visium

### ONTraC
- **Paper:** [Nature Methods, 2025](https://doi.org/10.1038/s41592-024-02552-8)
- **Code:** [github.com/gyuanlab/ONTraC](https://github.com/gyuanlab/ONTraC)
- **Key innovation:** Orders cells along spatial Niche Trajectories by integrating niche composition with graph-based trajectory inference, connecting spatial tissue organization to continuous biological processes.
- **Strengths:**
    - Niche-aware trajectory inference captures microenvironment transitions
    - Graph neural network learns spatial organization patterns
    - Can identify tissue axes of variation (e.g., cortical layers)
- **Limitations:**
    - Requires cell-type annotations as input
    - Niche definition is sensitive to neighborhood parameters
- **Technology compatibility:** MERFISH, Xenium, Visium, any spatial platform with cell-type labels

### STORIES
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.07.04.601979)
- **Code:** [github.com/LiLabAtVT/STORIES](https://github.com/LiLabAtVT/STORIES)
- **Key innovation:** Spatial Trajectory inference from Omics data using RNA velocity concepts adapted for spatial contexts.
- **Strengths:**
    - Connects RNA velocity concepts to spatial trajectories
    - Infers directionality of spatial transitions
- **Limitations:**
    - Preprint stage with limited validation
    - RNA velocity in spatial data is still controversial
- **Technology compatibility:** Visium, MERFISH

### scSpace
- **Paper:** [Nature Biotechnology, 2023](https://doi.org/10.1038/s41587-023-01773-0)
- **Code:** [github.com/ZJUFanLab/scSpace](https://github.com/ZJUFanLab/scSpace)
- **Key innovation:** Integrates spatial information with scRNA-seq trajectory analysis, mapping pseudotemporal ordering back to spatial coordinates.
- **Strengths:**
    - Bridges scRNA-seq trajectories and spatial data
    - Identifies spatially coherent trajectory branches
- **Limitations:**
    - Requires matched scRNA-seq and spatial data
    - Relies on existing trajectory inference from scRNA-seq
- **Technology compatibility:** Visium, MERFISH

### SOCS
- **Paper:** [bioRxiv, 2024](https://doi.org/10.1101/2024.05.31.596903)
- **Code:** [github.com/SOCS-team/SOCS](https://github.com/SOCS-team/SOCS)
- **Key innovation:** Spatial Ordering by Cell State — infers continuous cell-state gradients across tissue space without requiring a predefined trajectory.
- **Strengths:**
    - Discovers spatial gradients without trajectory assumptions
    - Identifies tissue axes of continuous variation
- **Limitations:**
    - Preprint with limited validation
    - Continuous gradient assumption may not fit discrete transitions
- **Technology compatibility:** Visium, MERFISH

## Benchmark Summary

Spatial trajectory inference is an emerging field with no formal benchmarks. **spaTrack** provides the most principled framework through optimal transport, while **ONTraC** uniquely integrates niche composition into trajectory inference. The fundamental challenge is validation — ground-truth spatial trajectories are rare and tissue-specific (liver zonation and intestinal crypt-villus are among the few well-characterized systems). Most published results are demonstrated on these same systems, and generalization to novel tissues remains unproven.

!!! warning "Trajectories are hypotheses, not facts"
    Spatial trajectories are computational inferences that should be treated as hypotheses. A spatially coherent ordering does not prove a developmental or dynamic process is occurring — it may reflect tissue architecture without temporal meaning. Validate with independent evidence (lineage tracing, time-course data, known biology).

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| Cell-resolution spatial | Track cell transitions through space | spaTrack | Optimal transport for spatial cell tracking |
| Spatial + cell-type labels | Niche-aware trajectory | ONTraC | Integrates niche composition with trajectories |
| Matched scRNA-seq + spatial | Map scRNA-seq trajectories to space | scSpace | Bridges existing trajectories with spatial coordinates |
| Any spatial data | Discover continuous spatial gradients | SOCS | Gradient discovery without trajectory assumptions |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| spaTrack | Yes | - | Yes | Yes | - | - | - |
| ONTraC | Yes | - | Yes | Yes | - | - | - |
| STORIES | Yes | - | - | Yes | - | - | - |
| scSpace | Yes | - | - | Yes | - | - | - |
| SOCS | Yes | - | - | Yes | - | - | - |
