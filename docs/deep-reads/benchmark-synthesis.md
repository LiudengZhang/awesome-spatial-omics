# Benchmark Synthesis

**Verdict:** Benchmarks converge on clear winners for deconvolution and segmentation, but spatial clustering and CCC rankings remain dataset-dependent -- and everyone benchmarks on DLPFC.

This page is not a deep read of a single paper. It synthesizes findings across major published benchmarks to identify what the field collectively agrees on, where disagreements remain, and what is missing from current evaluation practices.

## Deconvolution

**What benchmarks agree on:** [Cell2location](cell2location.md) and RCTD consistently rank at or near the top across independent benchmarks (Li et al. 2023; Yan & Sun 2023). Cell2location provides the best accuracy for rare cell types and is the only top method with calibrated uncertainty quantification. RCTD offers a faster alternative with slightly lower accuracy. NMF-based methods (SPOTlight) and simple regression approaches consistently underperform.

**Where they disagree:** Rankings shift depending on whether accuracy is measured by correlation, RMSE, or JSD (Jensen-Shannon divergence), and whether rare or abundant cell types are emphasized. On well-separated cell types, most methods perform adequately; the gap widens on closely related cell types (e.g., subtypes of excitatory neurons).

**Consensus:** Use Cell2location when compute allows; use RCTD for speed. Reference quality matters more than method choice.

## Spatial Clustering / Domain Identification

**What benchmarks agree on:** Methods that integrate spatial coordinates with expression data (BayesSpace, [GraphST](graphst.md), STAGATE) consistently outperform expression-only clustering (Leiden/Louvain). The improvement is most dramatic in tissues with clear spatial architecture (cortical layers, tissue zones).

**Where they disagree:** Rankings across methods are highly dataset-dependent. GraphST wins on some DLPFC sections, BayesSpace on others, and STAGATE on yet others. No single method dominates across all tissues and resolutions. The choice of clustering resolution parameter often matters more than the choice of method.

**Consensus:** Any spatially-aware method beats expression-only clustering. Beyond that, the specific winner depends on tissue type, resolution, and parameter tuning. Run multiple methods and look for consensus domains.

## Segmentation

**What benchmarks agree on:** [Cellpose](cellpose.md) is the default generalist for fluorescence and H&E-based segmentation. StarDist performs comparably on round, well-separated cells (e.g., lymphocytes) but struggles with irregular shapes. For transcript-based segmentation (assigning transcripts to cells without images), Baysor outperforms coordinate-only approaches.

**Where they disagree:** Performance rankings shift substantially between tissue types. Dense tissues (lymph nodes, tumors) are harder for all methods, and the gap between methods narrows. Cellpose 2.0 with fine-tuning can match or beat specialized models, but the default pretrained model has limitations on tissue types far from its training distribution.

**Consensus:** Start with Cellpose; fine-tune if results are poor. Use Baysor for transcript-only segmentation.

## Spatially Variable Gene Detection

**What benchmarks agree on:** [nnSVG](nnsvg.md) provides the best accuracy among GP-based methods at practical dataset sizes. SPARK-X is fastest and works well for clear spatial patterns. SpatialDE, the original method, is accurate but too slow for modern dataset sizes. Simple metrics like Moran's I (available in [Squidpy](squidpy.md)) provide a reasonable approximation for exploratory analysis.

**Where they disagree:** The definition of ground truth for SVG detection is itself contested. Some benchmarks use manually annotated layer marker genes, others use simulated data, and results can differ. Methods also disagree on genes with subtle or gradient-like spatial patterns, where the boundary between "spatially variable" and "noise" is ambiguous.

**Consensus:** nnSVG for thoroughness, SPARK-X for speed, Moran's I for quick exploration. The choice matters less than the biological interpretation of results.

## Cell-Cell Communication

**What benchmarks agree on:** Spatial CCC methods ([COMMOT](commot.md), SpatialDM) outperform non-spatial methods (CellPhoneDB, CellChat) when applied to spatial data -- unsurprisingly, since they use the additional distance information. Among spatial methods, optimal transport-based approaches (COMMOT) produce more biologically coherent spatial patterns than radius-based methods.

**Where they disagree:** CCC benchmarking is the weakest area because ground truth is nearly impossible to establish. Most evaluations rely on known biology (e.g., "Wnt signaling should be active in this region") rather than quantitative metrics, making objective ranking difficult.

**Consensus:** Use spatial methods for spatial data. Treat all CCC results as hypotheses requiring experimental validation.

## The DLPFC Problem

The dorsolateral prefrontal cortex (DLPFC) Visium dataset from Maynard et al. (2021) has become the de facto benchmark for spatial clustering, deconvolution, and SVG detection. Its popularity is understandable: 12 tissue sections with manually annotated cortical layers provide clear ground truth. But over-reliance on a single dataset creates risks:

- **Overfitting to cortical architecture:** The DLPFC has a clean layered structure. Methods optimized for this geometry may not generalize to tumors, immune tissues, or developing organs where spatial domains are irregular.
- **Benchmark gaming:** When every paper evaluates on DLPFC, there is implicit pressure to tune methods for this specific dataset, inflating reported performance.
- **Limited tissue diversity:** A single brain region from a single species cannot represent the diversity of spatial biology.

## What Is Missing

- **Multi-technology benchmarks:** Most benchmarks evaluate methods on a single platform (usually Visium). Cross-platform evaluation (Visium vs. MERFISH vs. Xenium on matched tissue) is rare but essential as analysts increasingly combine technologies.
- **Scalability benchmarks:** Few benchmarks systematically evaluate how methods perform as dataset size increases from thousands to millions of spots/cells, which is critical for [Visium HD](visium-hd.md) and imaging-based platforms.
- **End-to-end pipeline evaluation:** Benchmarks evaluate individual steps (segmentation, clustering, deconvolution) in isolation. How errors propagate through a full pipeline -- bad segmentation leading to wrong cell types leading to incorrect CCC -- is largely unstudied.
- **Ground truth beyond DLPFC:** The field urgently needs curated benchmark datasets for diverse tissues (tumor, immune, developmental) with expert annotations, matched multi-modal measurements, and clearly defined evaluation criteria.
