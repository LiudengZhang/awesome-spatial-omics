# Literature Timeline

Interactive timeline of key publications and tool releases in spatial omics. Hover over items for details. Filter by category.

<div id="timeline-controls" style="margin-bottom: 1rem; display: flex; gap: 0.5rem; flex-wrap: wrap;">
  <button class="tl-btn active" data-group="all" style="padding: 0.3rem 0.8rem; border-radius: 4px; border: 1px solid #009688; background: #009688; color: white; cursor: pointer; font-size: 0.85rem;">All</button>
  <button class="tl-btn" data-group="Technology" style="padding: 0.3rem 0.8rem; border-radius: 4px; border: 1px solid #1976D2; background: white; color: #1976D2; cursor: pointer; font-size: 0.85rem;">Technology</button>
  <button class="tl-btn" data-group="Segmentation" style="padding: 0.3rem 0.8rem; border-radius: 4px; border: 1px solid #E64A19; background: white; color: #E64A19; cursor: pointer; font-size: 0.85rem;">Segmentation</button>
  <button class="tl-btn" data-group="SVG Detection" style="padding: 0.3rem 0.8rem; border-radius: 4px; border: 1px solid #7B1FA2; background: white; color: #7B1FA2; cursor: pointer; font-size: 0.85rem;">SVG Detection</button>
  <button class="tl-btn" data-group="Spatial Domains" style="padding: 0.3rem 0.8rem; border-radius: 4px; border: 1px solid #2E7D32; background: white; color: #2E7D32; cursor: pointer; font-size: 0.85rem;">Spatial Domains</button>
  <button class="tl-btn" data-group="Deconvolution" style="padding: 0.3rem 0.8rem; border-radius: 4px; border: 1px solid #F57F17; background: white; color: #F57F17; cursor: pointer; font-size: 0.85rem;">Deconvolution</button>
  <button class="tl-btn" data-group="CCC" style="padding: 0.3rem 0.8rem; border-radius: 4px; border: 1px solid #00695C; background: white; color: #00695C; cursor: pointer; font-size: 0.85rem;">CCC</button>
  <button class="tl-btn" data-group="Framework" style="padding: 0.3rem 0.8rem; border-radius: 4px; border: 1px solid #5D4037; background: white; color: #5D4037; cursor: pointer; font-size: 0.85rem;">Framework</button>
  <button class="tl-btn" data-group="Benchmark" style="padding: 0.3rem 0.8rem; border-radius: 4px; border: 1px solid #455A64; background: white; color: #455A64; cursor: pointer; font-size: 0.85rem;">Benchmark</button>
</div>

<div id="timeline" style="width: 100%; height: 500px; border: 1px solid #e0e0e0; border-radius: 8px;"></div>

<link href="https://unpkg.com/vis-timeline@7.7.3/styles/vis-timeline-graph2d.min.css" rel="stylesheet" />
<script src="https://unpkg.com/vis-timeline@7.7.3/standalone/umd/vis-timeline-graph2d.min.js"></script>

<script>
var groups = new vis.DataSet([
  {id: 'Technology', content: 'Technology', style: 'color: #1976D2;'},
  {id: 'Segmentation', content: 'Segmentation', style: 'color: #E64A19;'},
  {id: 'SVG Detection', content: 'SVG Detection', style: 'color: #7B1FA2;'},
  {id: 'Spatial Domains', content: 'Spatial Domains', style: 'color: #2E7D32;'},
  {id: 'Deconvolution', content: 'Deconvolution', style: 'color: #F57F17;'},
  {id: 'CCC', content: 'CCC', style: 'color: #00695C;'},
  {id: 'Framework', content: 'Framework', style: 'color: #5D4037;'},
  {id: 'Benchmark', content: 'Benchmark', style: 'color: #455A64;'}
]);

var items = new vis.DataSet([
  // Technology
  {id: 1, content: 'Spatial Transcriptomics', start: '2016-06-01', group: 'Technology', title: 'Spatial Transcriptomics — Spatially resolved transcriptomics via tissue barcoding (Science, 2016)', style: 'background-color: #BBDEFB;'},
  {id: 2, content: 'MERFISH', start: '2015-04-01', group: 'Technology', title: 'MERFISH — Multiplexed error-robust FISH for single-cell transcriptomics (Science, 2015)', style: 'background-color: #BBDEFB;'},
  {id: 3, content: 'seqFISH', start: '2014-03-01', group: 'Technology', title: 'seqFISH — Sequential barcoding FISH for multiplex RNA imaging (Neuron, 2014)', style: 'background-color: #BBDEFB;'},
  {id: 4, content: 'Slide-seq', start: '2019-03-01', group: 'Technology', title: 'Slide-seq — Spatial transcriptomics via DNA-barcoded bead arrays (Science, 2019)', style: 'background-color: #BBDEFB;'},
  {id: 5, content: 'Visium launch', start: '2019-10-01', group: 'Technology', title: 'Visium — 10x Genomics commercial spatial transcriptomics platform (2019)', style: 'background-color: #BBDEFB;'},
  {id: 6, content: 'STARmap', start: '2018-06-01', group: 'Technology', title: 'STARmap — In situ RNA sequencing via SNAIL probes (Science, 2018)', style: 'background-color: #BBDEFB;'},
  {id: 7, content: 'Stereo-seq', start: '2022-05-01', group: 'Technology', title: 'Stereo-seq — Subcellular-resolution spatial transcriptomics at centimeter scale (Cell, 2022)', style: 'background-color: #BBDEFB;'},
  {id: 8, content: 'CODEX', start: '2018-08-01', group: 'Technology', title: 'CODEX — CO-Detection by indEXing for multiplexed protein imaging (Cell, 2018)', style: 'background-color: #BBDEFB;'},
  {id: 9, content: 'IMC', start: '2014-03-01', group: 'Technology', title: 'IMC — Imaging Mass Cytometry for multiplexed protein spatial imaging (Nat Methods, 2014)', style: 'background-color: #BBDEFB;'},
  {id: 10, content: 'Xenium', start: '2023-01-01', group: 'Technology', title: 'Xenium — 10x Genomics in situ single-cell spatial platform (2023)', style: 'background-color: #BBDEFB;'},
  {id: 11, content: 'MERSCOPE', start: '2021-06-01', group: 'Technology', title: 'MERSCOPE — Vizgen commercial MERFISH platform (2021)', style: 'background-color: #BBDEFB;'},
  {id: 12, content: 'CosMx', start: '2022-09-01', group: 'Technology', title: 'CosMx — NanoString spatial molecular imager for RNA and protein (Nat Biotech, 2022)', style: 'background-color: #BBDEFB;'},
  {id: 13, content: 'Visium HD', start: '2023-12-01', group: 'Technology', title: 'Visium HD — Single-cell resolution Visium with 2 um bins (2023)', style: 'background-color: #BBDEFB;'},
  {id: 14, content: 'Open-ST', start: '2024-01-01', group: 'Technology', title: 'Open-ST — Open-source spatial transcriptomics at subcellular resolution (Cell, 2024)', style: 'background-color: #BBDEFB;'},
  {id: 15, content: 'Slide-tags', start: '2024-03-01', group: 'Technology', title: 'Slide-tags — Spatial single-nucleus multi-omics via barcoded tags (Nature, 2024)', style: 'background-color: #BBDEFB;'},

  // Segmentation
  {id: 16, content: 'Cellpose', start: '2021-01-01', group: 'Segmentation', title: 'Cellpose — Generalist deep-learning cell segmentation (Nat Methods, 2021)', style: 'background-color: #FFCCBC;'},
  {id: 17, content: 'StarDist', start: '2018-09-01', group: 'Segmentation', title: 'StarDist — Star-convex polygon cell detection (MICCAI, 2018)', style: 'background-color: #FFCCBC;'},
  {id: 18, content: 'Baysor', start: '2022-01-01', group: 'Segmentation', title: 'Baysor — Bayesian transcript-based cell segmentation (Nat Biotech, 2022)', style: 'background-color: #FFCCBC;'},
  {id: 19, content: 'Mesmer/DeepCell', start: '2022-01-01', group: 'Segmentation', title: 'Mesmer — Deep-learning whole-cell segmentation for tissue imaging (Nat Biotech, 2022)', style: 'background-color: #FFCCBC;'},
  {id: 20, content: 'CellSAM', start: '2024-01-01', group: 'Segmentation', title: 'CellSAM — Foundation model for cell segmentation via SAM (2024)', style: 'background-color: #FFCCBC;'},
  {id: 21, content: 'Bin2Cell', start: '2024-06-01', group: 'Segmentation', title: 'Bin2Cell — Segmentation-free cell-level analysis from Visium HD bins (2024)', style: 'background-color: #FFCCBC;'},

  // SVG Detection
  {id: 22, content: 'SpatialDE', start: '2018-02-01', group: 'SVG Detection', title: 'SpatialDE — Gaussian process-based spatially variable gene detection (Nat Methods, 2018)', style: 'background-color: #E1BEE7;'},
  {id: 23, content: 'SPARK', start: '2020-03-01', group: 'SVG Detection', title: 'SPARK — Generalized linear spatial kernel test for SVG detection (Nat Methods, 2020)', style: 'background-color: #E1BEE7;'},
  {id: 24, content: 'SPARK-X', start: '2021-07-01', group: 'SVG Detection', title: 'SPARK-X — Non-parametric SVG detection, scalable to millions of cells (Genome Bio, 2021)', style: 'background-color: #E1BEE7;'},
  {id: 25, content: 'nnSVG', start: '2023-03-01', group: 'SVG Detection', title: 'nnSVG — Nearest-neighbor Gaussian process SVG detection (Nat Comm, 2023)', style: 'background-color: #E1BEE7;'},
  {id: 26, content: 'Hotspot', start: '2021-04-01', group: 'SVG Detection', title: 'Hotspot — Local autocorrelation for spatially informative genes (Cell Systems, 2021)', style: 'background-color: #E1BEE7;'},

  // Spatial Domains
  {id: 27, content: 'BayesSpace', start: '2021-05-01', group: 'Spatial Domains', title: 'BayesSpace — Bayesian spatial clustering with enhanced resolution (Nat Biotech, 2021)', style: 'background-color: #C8E6C9;'},
  {id: 28, content: 'SpaGCN', start: '2021-10-01', group: 'Spatial Domains', title: 'SpaGCN — Graph convolutional network for spatial domain identification (Nat Methods, 2021)', style: 'background-color: #C8E6C9;'},
  {id: 29, content: 'STAGATE', start: '2022-06-01', group: 'Spatial Domains', title: 'STAGATE — Graph attention auto-encoder for spatial clustering (Nat Comm, 2022)', style: 'background-color: #C8E6C9;'},
  {id: 30, content: 'GraphST', start: '2023-04-01', group: 'Spatial Domains', title: 'GraphST — Self-supervised contrastive learning for spatial transcriptomics (Nat Comm, 2023)', style: 'background-color: #C8E6C9;'},
  {id: 31, content: 'BANKSY', start: '2024-02-01', group: 'Spatial Domains', title: 'BANKSY — Spatial domain segmentation via neighborhood expression (Nat Genetics, 2024)', style: 'background-color: #C8E6C9;'},
  {id: 32, content: 'BASS', start: '2022-08-01', group: 'Spatial Domains', title: 'BASS — Bayesian analytics for spatial segmentation (Genome Bio, 2022)', style: 'background-color: #C8E6C9;'},

  // Deconvolution
  {id: 33, content: 'Cell2location', start: '2022-01-01', group: 'Deconvolution', title: 'Cell2location — Bayesian spatial deconvolution with scRNA-seq reference (Nat Biotech, 2022)', style: 'background-color: #FFF9C4;'},
  {id: 34, content: 'RCTD', start: '2022-01-01', group: 'Deconvolution', title: 'RCTD — Robust cell type decomposition of spatial transcriptomics (Nat Biotech, 2022)', style: 'background-color: #FFF9C4;'},
  {id: 35, content: 'Tangram', start: '2021-10-01', group: 'Deconvolution', title: 'Tangram — Deep learning alignment of scRNA-seq to spatial data (Nat Methods, 2021)', style: 'background-color: #FFF9C4;'},
  {id: 36, content: 'CARD', start: '2022-10-01', group: 'Deconvolution', title: 'CARD — Conditional autoregressive-based deconvolution (Nat Biotech, 2022)', style: 'background-color: #FFF9C4;'},
  {id: 37, content: 'STdeconvolve', start: '2022-04-01', group: 'Deconvolution', title: 'STdeconvolve — Reference-free deconvolution via topic modeling (Nat Comm, 2022)', style: 'background-color: #FFF9C4;'},
  {id: 38, content: 'CytoSPACE', start: '2023-05-01', group: 'Deconvolution', title: 'CytoSPACE — Single-cell spatial mapping via optimal transport (Nat Biotech, 2023)', style: 'background-color: #FFF9C4;'},
  {id: 39, content: 'SPOTlight', start: '2021-06-01', group: 'Deconvolution', title: 'SPOTlight — NMF-based deconvolution for spatial transcriptomics (NAR, 2021)', style: 'background-color: #FFF9C4;'},

  // CCC
  {id: 40, content: 'NicheNet', start: '2020-02-01', group: 'CCC', title: 'NicheNet — Ligand-target modeling for cell-cell communication (Nat Methods, 2020)', style: 'background-color: #B2DFDB;'},
  {id: 41, content: 'CellChat', start: '2021-02-01', group: 'CCC', title: 'CellChat — Inference of intercellular communication networks (Nat Comm, 2021)', style: 'background-color: #B2DFDB;'},
  {id: 42, content: 'COMMOT', start: '2023-01-01', group: 'CCC', title: 'COMMOT — Optimal transport-based spatial cell-cell communication (Nat Methods, 2023)', style: 'background-color: #B2DFDB;'},
  {id: 43, content: 'MISTy', start: '2022-01-01', group: 'CCC', title: 'MISTy — Multi-view learning of tissue organization (Genome Bio, 2022)', style: 'background-color: #B2DFDB;'},
  {id: 44, content: 'LIANA+', start: '2024-03-01', group: 'CCC', title: 'LIANA+ — Unified CCC framework with spatial awareness (Nat Cell Bio, 2024)', style: 'background-color: #B2DFDB;'},
  {id: 45, content: 'SpatialDM', start: '2023-06-01', group: 'CCC', title: 'SpatialDM — Spatial co-expression-based ligand-receptor interactions (Nat Comm, 2023)', style: 'background-color: #B2DFDB;'},

  // Framework
  {id: 46, content: 'Squidpy', start: '2022-01-01', group: 'Framework', title: 'Squidpy — Scalable spatial omics analysis in Python (Nat Methods, 2022)', style: 'background-color: #D7CCC8;'},
  {id: 47, content: 'Giotto', start: '2021-03-01', group: 'Framework', title: 'Giotto — Comprehensive toolbox for spatial transcriptomics (Genome Bio, 2021)', style: 'background-color: #D7CCC8;'},
  {id: 48, content: 'SpatialData', start: '2024-02-01', group: 'Framework', title: 'SpatialData — Unified data framework for spatial omics (Nat Methods, 2024)', style: 'background-color: #D7CCC8;'},
  {id: 49, content: 'Voyager', start: '2023-06-01', group: 'Framework', title: 'Voyager — Spatial statistics framework bridging geospatial and genomics (Bioinformatics, 2023)', style: 'background-color: #D7CCC8;'},
  {id: 50, content: 'Nicheformer', start: '2025-01-01', group: 'Framework', title: 'Nicheformer — Foundation model for single-cell and spatial transcriptomics (2025)', style: 'background-color: #D7CCC8;'},
  {id: 51, content: 'Novae', start: '2025-03-01', group: 'Framework', title: 'Novae — Graph-based foundation model for spatial transcriptomics (2025)', style: 'background-color: #D7CCC8;'},

  // Benchmark
  {id: 52, content: 'Museum of ST', start: '2022-05-01', group: 'Benchmark', title: 'Museum of Spatial Transcriptomics — Comprehensive review and benchmark resource (Nat Methods, 2022)', style: 'background-color: #CFD8DC;'},
  {id: 53, content: 'Deconv benchmark', start: '2022-06-01', group: 'Benchmark', title: 'Deconvolution benchmark — Systematic comparison of spatial deconvolution methods (Nat Comm, 2022)', style: 'background-color: #CFD8DC;'},
  {id: 54, content: 'Segmentation benchmark', start: '2023-04-01', group: 'Benchmark', title: 'Segmentation benchmark — Comparison of cell segmentation methods for spatial omics (Nat Biotech, 2023)', style: 'background-color: #CFD8DC;'},
  {id: 55, content: 'SVG benchmark', start: '2023-03-01', group: 'Benchmark', title: 'SVG benchmark — Systematic evaluation of spatially variable gene detection methods (Genome Bio, 2023)', style: 'background-color: #CFD8DC;'}
]);

var container = document.getElementById('timeline');
var options = {
  stack: true,
  horizontalScroll: true,
  zoomKey: 'ctrlKey',
  maxHeight: 500,
  start: '2013-01-01',
  end: '2026-01-01',
  tooltip: {followMouse: true, overflowMethod: 'cap'},
  groupOrder: function(a, b) {
    var order = ['Technology', 'Segmentation', 'SVG Detection', 'Spatial Domains', 'Deconvolution', 'CCC', 'Framework', 'Benchmark'];
    return order.indexOf(a.id) - order.indexOf(b.id);
  }
};

var timeline = new vis.Timeline(container, items, groups, options);

// Filter buttons
document.querySelectorAll('.tl-btn').forEach(function(btn) {
  btn.addEventListener('click', function() {
    document.querySelectorAll('.tl-btn').forEach(function(b) {
      b.style.background = 'white';
      b.style.color = b.style.borderColor;
    });
    this.style.background = this.style.borderColor || '#009688';
    this.style.color = 'white';

    var group = this.getAttribute('data-group');
    if (group === 'all') {
      items.forEach(function(item) { items.update({id: item.id, visible: true}); });
    } else {
      items.forEach(function(item) {
        items.update({id: item.id, visible: item.group === group});
      });
    }
  });
});
</script>

<style>
.vis-item { border-radius: 4px; font-size: 0.8rem; }
.vis-item .vis-item-content { padding: 3px 8px; }
</style>

---

## How to Read This Timeline

- **Scroll horizontally** to navigate through time (2014-2025)
- **Hover** over items for paper details
- **Filter** by category using the buttons above
- **Zoom** with Ctrl+scroll

The timeline reveals several patterns:

1. **2014-2016 -- Founding technologies**: seqFISH, IMC, MERFISH, and Spatial Transcriptomics established the core modalities (imaging-based vs. sequencing-based).
2. **2018-2019 -- Platform convergence**: Commercial instruments launched (Visium), analysis tools emerged (SpatialDE, StarDist), and new sequencing approaches (Slide-seq, STARmap) expanded the design space.
3. **2021-2022 -- Methods explosion**: The field hit critical mass with tools for every analysis step -- segmentation (Cellpose, Baysor), deconvolution (Cell2location, RCTD, Tangram), spatial domains (BayesSpace, STAGATE), CCC (CellChat, MISTy), and frameworks (Squidpy, Giotto).
4. **2023-2024 -- Maturation and single-cell resolution**: Next-generation platforms (Xenium, Visium HD, CosMx) pushed toward subcellular resolution. Graph neural network methods (GraphST, BANKSY) became dominant for spatial domains. Benchmark studies systematically compared methods.
5. **2025 -- Foundation models arrive**: Nicheformer and Novae represent a shift toward pre-trained models that learn spatial representations across datasets, moving beyond task-specific tools.
