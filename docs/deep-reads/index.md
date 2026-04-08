# Deep Reads

Twelve papers that define the spatial omics field, selected to cover every major pipeline step from raw data to biological interpretation. Each deep read provides an honest assessment -- strengths, limitations, and the key design bet the paper makes.

| Paper | Pipeline Step | Verdict |
|-------|--------------|---------|
| [Museum of ST (Moses & Pachter, 2022)](museum-of-st.md) | Field overview | The definitive taxonomy -- start here |
| [Visium HD (10x, 2023)](visium-hd.md) | Technology | Near single-cell resolution, but analysis tools lag behind |
| [Cellpose (Stringer et al., 2021)](cellpose.md) | Segmentation | The generalist that just works -- retrain for your tissue |
| [nnSVG (Weber et al., 2023)](nnsvg.md) | SVG detection | Best accuracy-scalability balance for SVG detection |
| [GraphST (Long et al., 2023)](graphst.md) | Spatial domains | Self-supervised contrastive learning beats supervised methods |
| [Cell2location (Kleshchevnikov et al., 2022)](cell2location.md) | Deconvolution | Bayesian deconvolution done right -- best accuracy, worth the compute |
| [COMMOT (Cang et al., 2023)](commot.md) | CCC | Optimal transport makes CCC truly spatial |
| [Tangram (Biancalani et al., 2021)](tangram.md) | Integration | The bridge between scRNA-seq and spatial data |
| [Nicheformer (Schaar et al., 2025)](nicheformer.md) | Foundation models | 49M params beats 444M -- spatial context matters more than scale |
| [SpatialData (Marconato et al., 2024)](spatialdata.md) | Infrastructure | FAIR data framework -- the scverse answer to data fragmentation |
| [Squidpy (Palla et al., 2022)](squidpy.md) | Infrastructure | The Swiss Army knife for spatial analysis |
| [Benchmark Synthesis](benchmark-synthesis.md) | Benchmarks | What all benchmarks agree on (and where they disagree) |

## How to use these reads

Each deep read follows the same structure: **Problem Setup**, **Method**, **Evaluation**, and an **Honest Assessment** with strengths, limitations, and the key design decision. They are written to complement the [methods overview](../methods/index.md) and [benchmarks](../benchmarks/index.md) pages -- those pages provide breadth, while deep reads provide depth on the most influential work.

Reading order depends on background. For newcomers to the field, start with the Museum of ST for vocabulary, then SpatialData and Squidpy for infrastructure. For analysts building a pipeline, follow the pipeline order: segmentation (Cellpose), SVG detection (nnSVG), spatial domains (GraphST), deconvolution (Cell2location), CCC (COMMOT), and integration (Tangram). For those interested in where the field is heading, read Nicheformer and the Benchmark Synthesis last.
