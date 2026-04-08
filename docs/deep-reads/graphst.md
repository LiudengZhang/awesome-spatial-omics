# GraphST

**Verdict:** Self-supervised contrastive learning on spatial graphs beats supervised methods for spatial domain identification.

**Citation:** Long Y, Ang KS, Li M, et al. "Spatially informed clustering, integration, and deconvolution of spatial transcriptomics with GraphST." *Nature Communications* 14, 1155 (2023). [DOI: 10.1038/s41467-023-36796-3](https://doi.org/10.1038/s41467-023-36796-3)

## Problem Setup

Identifying spatial domains -- contiguous tissue regions with coherent gene expression programs -- is one of the most common spatial transcriptomics analysis tasks. Standard clustering methods (Leiden, Louvain) applied to expression data alone ignore spatial coordinates, producing fragmented clusters that do not respect tissue architecture. The challenge is to integrate gene expression similarity with spatial proximity in a principled way, without requiring annotated training data that is scarce for most tissues.

## Method

GraphST constructs a spatial graph where spots are nodes and edges connect spatially adjacent spots (typically k-nearest neighbors in physical space). Each node's initial features are the spot's gene expression profile. A graph neural network (GNN) then propagates information across the graph, so each spot's learned representation incorporates both its own expression and the expression patterns of its spatial neighborhood.

The training objective is self-supervised contrastive learning: the model learns to make representations of spatially adjacent spots more similar while pushing apart representations of distant spots. This is achieved through graph augmentation (randomly masking nodes or edges) and a contrastive loss that does not require any ground-truth domain labels. After training, spots are clustered in the learned embedding space using standard methods (e.g., mclust or Leiden).

Beyond spatial domain identification, GraphST extends to two additional tasks: batch integration across multiple tissue sections (by aligning embeddings across sections) and spot-level deconvolution (by mapping scRNA-seq reference cells to spatial locations in the embedding space). This multi-task design is enabled by the general-purpose nature of the learned spatial embeddings.

## Evaluation

On the DLPFC benchmark dataset (12 tissue sections with manually annotated cortical layers), GraphST achieved an adjusted Rand index (ARI) of 0.60, outperforming SpaGCN (0.45), BayesSpace (0.52), and STAGATE (0.56). The method correctly recovered the six-layer cortical structure with smooth domain boundaries. On mouse brain and breast cancer datasets, GraphST identified biologically meaningful domains that corresponded to known anatomical regions.

For batch integration, GraphST produced well-mixed embeddings across sections while preserving biological variation, outperforming Harmony and Scanorama on spatial data. Deconvolution performance was competitive with but not superior to dedicated methods like [Cell2location](cell2location.md).

## Honest Assessment

**Strengths:**

- Self-supervised design eliminates the need for annotated training data, making the method applicable to any tissue without domain-specific labels.
- Multi-task capability (clustering, integration, deconvolution) from a single framework reduces the number of tools in a pipeline.
- Contrastive learning produces embeddings that naturally respect spatial continuity, yielding smoother domain boundaries than expression-only clustering.
- Strong benchmark performance across multiple tissue types and platforms.

**Limitations:**

- Computationally expensive relative to simpler methods: the GNN training loop requires GPU and can take 10--30 minutes per dataset, compared to seconds for BayesSpace or Leiden clustering.
- Hyperparameter sensitive -- the number of neighbors in the spatial graph, augmentation rates, and contrastive temperature all affect results, and optimal settings vary across datasets.
- Deconvolution performance, while reasonable, does not match dedicated Bayesian methods like [Cell2location](cell2location.md) that explicitly model expression signatures.
- The DLPFC benchmark is overrepresented in evaluation (see [Benchmark Synthesis](benchmark-synthesis.md)), raising questions about generalization to less-structured tissues.

**Design Decision:** The central bet is that self-supervised contrastive learning on spatial graphs can learn tissue structure without labels, outperforming both supervised methods (which need annotations) and statistical methods (which make parametric assumptions). The strong DLPFC results support this, but the real test is on tissues without clear layered architecture, where the inductive bias toward spatial smoothness may be too strong.
