# COMMOT

**Verdict:** Optimal transport makes cell-cell communication truly spatial -- no arbitrary radius cutoffs.

**Citation:** Cang Z, Zhao Y, Almet AA, et al. "Screening cell-cell communication in spatial transcriptomics via collective optimal transport." *Nature Methods* 20, 218--228 (2023). [DOI: 10.1038/s41592-022-01728-4](https://doi.org/10.1038/s41592-022-01728-4)

## Problem Setup

Cell-cell communication (CCC) inference from spatial transcriptomics data requires determining which cells are sending signals and which are receiving them. Most CCC methods (CellChat, CellPhoneDB) were designed for scRNA-seq and infer communication from co-expression of ligand-receptor pairs without considering physical distance. Spatial CCC methods that do use distance typically impose a hard radius cutoff -- cells within X microns can communicate, cells outside cannot. This is biologically unrealistic: signaling molecules diffuse through tissue with distance-dependent decay, not binary on/off.

## Method

COMMOT (COllective optimal transport for Mapping cell-cell coMMunication in spatial Transcriptomics) frames spatial CCC as an optimal transport problem. For each ligand-receptor pair, the method constructs a transport plan that moves "signal mass" from cells expressing the ligand to cells expressing the receptor, with a transport cost proportional to physical distance. The optimal transport framework naturally penalizes long-distance signaling without imposing a hard cutoff.

The distance-dependent cost function can incorporate prior knowledge about ligand diffusion ranges -- secreted ligands like growth factors are allowed to signal over longer distances than membrane-bound ligands like Notch-Delta, which require direct cell contact. This biologically informed cost structure is encoded through a distance threshold parameter that softly controls the effective signaling range.

The output is a spatial communication score for each ligand-receptor pair at each location, quantifying the total communication flux through each cell. These scores can be aggregated to identify spatial regions of active signaling (communication "hotspots") and directional signaling patterns (e.g., signal flowing from tumor to stroma).

COMMOT also provides a downstream analysis module that identifies genes whose expression correlates with communication activity, connecting spatial CCC to transcriptional programs.

## Evaluation

On simulated spatial data with known ground-truth communication patterns, COMMOT correctly recovered directional signaling gradients that radius-based methods missed. On mouse embryo Visium data, COMMOT identified biologically validated signaling patterns including Wnt signaling gradients in developing limb buds and FGF signaling in the neural tube, consistent with decades of developmental biology knowledge.

Comparison with CellChat and stLearn on the same datasets showed that COMMOT produced smoother, more biologically coherent spatial communication patterns, particularly for secreted ligands where the diffusion range is intermediate (neither strictly local nor global).

## Honest Assessment

**Strengths:**

- The optimal transport framework eliminates arbitrary radius cutoffs, replacing them with continuous distance-dependent costs that better reflect ligand diffusion physics.
- Ligand-specific distance parameters allow different signaling modes (juxtacrine, paracrine, endocrine) to be modeled differently within the same framework.
- Produces directional communication maps, not just pairwise scores, enabling analysis of signaling flow across tissue.
- The mathematical framework is principled and connects spatial CCC to a well-studied optimization problem with efficient solvers.

**Limitations:**

- Computationally expensive: optimal transport scales superlinearly with the number of cells, making it slow for large imaging-based datasets (>100,000 cells) without approximation.
- Limited to known ligand-receptor pairs from curated databases -- cannot discover novel signaling interactions.
- The distance threshold parameters, while more principled than hard cutoffs, still require user specification or estimation, and results can be sensitive to these choices.
- Does not model downstream signaling cascades -- communication is inferred at the ligand-receptor level only, not at the pathway or transcriptional response level.

**Design Decision:** The central bet is that optimal transport provides a better mathematical framework for spatial CCC than either radius-based methods or expression-correlation methods. The biological results support this -- diffusion-like signaling patterns are captured naturally. The remaining challenge is scaling: as spatial datasets grow to millions of cells with [Visium HD](visium-hd.md) or imaging platforms, approximate optimal transport solvers will be needed to keep the approach practical.
