# Explorer

Interactive maps for navigating the spatial omics landscape. Switch between the **Pipeline Map** (analysis workflow) and the **Technology Compatibility Map** (platform-tool matrix).

<style>
.tab-container { margin-bottom: 1rem; }
.tab-btn { padding: 0.5rem 1.2rem; border: 1px solid #009688; background: white; color: #009688; cursor: pointer; font-size: 0.9rem; font-weight: 500; border-radius: 4px 4px 0 0; margin-right: 2px; }
.tab-btn.active { background: #009688; color: white; }
.tab-panel { display: none; }
.tab-panel.active { display: block; }
.cy-container { width: 100%; height: 600px; border: 1px solid #e0e0e0; border-radius: 0 8px 8px 8px; background: #fafafa; }
.legend { display: flex; gap: 1rem; flex-wrap: wrap; margin-top: 0.5rem; font-size: 0.8rem; color: #5a5a7a; }
.legend-item { display: flex; align-items: center; gap: 0.3rem; }
.legend-dot { width: 12px; height: 12px; border-radius: 50%; display: inline-block; }
</style>

<div class="tab-container">
  <button class="tab-btn active" onclick="switchTab('pipeline')">Pipeline Map</button>
  <button class="tab-btn" onclick="switchTab('compat')">Technology Compatibility</button>
</div>

<div id="pipeline-panel" class="tab-panel active">
  <div id="cy-pipeline" class="cy-container"></div>
  <div class="legend">
    <div class="legend-item"><span class="legend-dot" style="background:#9E9E9E;"></span> Raw Data</div>
    <div class="legend-item"><span class="legend-dot" style="background:#90CAF9;"></span> Pre-processing</div>
    <div class="legend-item"><span class="legend-dot" style="background:#E64A19;"></span> Segmentation</div>
    <div class="legend-item"><span class="legend-dot" style="background:#7B1FA2;"></span> SVG Detection</div>
    <div class="legend-item"><span class="legend-dot" style="background:#2E7D32;"></span> Spatial Domains</div>
    <div class="legend-item"><span class="legend-dot" style="background:#F57F17;"></span> Deconvolution</div>
    <div class="legend-item"><span class="legend-dot" style="background:#E91E63;"></span> Cell Annotation</div>
    <div class="legend-item"><span class="legend-dot" style="background:#00695C;"></span> CCC</div>
    <div class="legend-item"><span class="legend-dot" style="background:#1B5E20;"></span> Niches</div>
    <div class="legend-item"><span class="legend-dot" style="background:#CE93D8;"></span> DE</div>
    <div class="legend-item"><span class="legend-dot" style="background:#FFB300;"></span> Integration</div>
    <div class="legend-item"><span class="legend-dot" style="background:#283593;"></span> Foundation Models</div>
    <div class="legend-item"><span class="legend-dot" style="background:#5D4037;"></span> 3D Reconstruction</div>
    <div class="legend-item"><span class="legend-dot" style="background:#00ACC1;"></span> Trajectories</div>
    <div class="legend-item"><span class="legend-dot" style="background:#78909C;"></span> Visualization</div>
  </div>
  <p style="font-size:0.85rem; color:#5a5a7a; margin-top:0.5rem;">Click a node to highlight its connections. Category nodes show representative tools as smaller child nodes. Drag nodes to rearrange.</p>
</div>

<div id="compat-panel" class="tab-panel">
  <div id="cy-compat" class="cy-container"></div>
  <div class="legend">
    <div class="legend-item"><span class="legend-dot" style="background:#1976D2;"></span> Technology Platform</div>
    <div class="legend-item"><span class="legend-dot" style="background:#E64A19;"></span> Analysis Tool</div>
  </div>
  <p style="font-size:0.85rem; color:#5a5a7a; margin-top:0.5rem;">Click a technology or tool node to see its connections. Edges indicate documented compatibility.</p>
</div>

<script src="https://unpkg.com/cytoscape@3.30.4/dist/cytoscape.min.js"></script>
<script src="https://unpkg.com/dagre@0.8.5/dist/dagre.min.js"></script>
<script src="https://unpkg.com/cytoscape-dagre@2.5.0/cytoscape-dagre.js"></script>

<script>
function switchTab(tab) {
  document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
  document.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
  if (tab === 'pipeline') {
    document.querySelectorAll('.tab-btn')[0].classList.add('active');
    document.getElementById('pipeline-panel').classList.add('active');
  } else {
    document.querySelectorAll('.tab-btn')[1].classList.add('active');
    document.getElementById('compat-panel').classList.add('active');
    if (!window.cyCompatInitialized) { initCompat(); window.cyCompatInitialized = true; }
  }
}

// === PIPELINE MAP ===
var cyPipeline = cytoscape({
  container: document.getElementById('cy-pipeline'),
  style: [
    {selector: 'node', style: {
      'label': 'data(label)', 'text-valign': 'center', 'text-halign': 'center',
      'background-color': 'data(color)', 'color': '#fff', 'font-size': '10px',
      'width': 'data(size)', 'height': 'data(size)', 'text-wrap': 'wrap', 'text-max-width': '90px',
      'border-width': 1, 'border-color': '#333'
    }},
    {selector: 'node[type="tool"]', style: {
      'font-size': '8px', 'width': 25, 'height': 25, 'text-max-width': '60px',
      'border-width': 0.5, 'opacity': 0.9
    }},
    {selector: 'edge', style: {
      'width': 1.5, 'line-color': '#bbb', 'target-arrow-color': '#bbb',
      'target-arrow-shape': 'triangle', 'curve-style': 'bezier', 'arrow-scale': 0.8
    }},
    {selector: 'edge[type="tool-link"]', style: {
      'width': 0.8, 'line-color': '#ddd', 'line-style': 'dotted',
      'target-arrow-shape': 'none'
    }},
    {selector: 'node:selected', style: {'border-width': 3, 'border-color': '#000'}},
    {selector: '.highlighted', style: {'border-width': 3, 'border-color': '#FF5722'}},
    {selector: '.faded', style: {'opacity': 0.15}}
  ],
  elements: {
    nodes: [
      // Category nodes
      {data: {id: 'raw', label: 'Raw Data', color: '#9E9E9E', size: 45}},
      {data: {id: 'preproc', label: 'Pre-processing', color: '#90CAF9', size: 45}},
      {data: {id: 'seg', label: 'Segmentation', color: '#E64A19', size: 50}},
      {data: {id: 'svg', label: 'SVG Detection', color: '#7B1FA2', size: 45}},
      {data: {id: 'domains', label: 'Spatial\nDomains', color: '#2E7D32', size: 48}},
      {data: {id: 'deconv', label: 'Deconvolution', color: '#F57F17', size: 48}},
      {data: {id: 'cellanno', label: 'Cell\nAnnotation', color: '#E91E63', size: 42}},
      {data: {id: 'ccc', label: 'CCC', color: '#00695C', size: 45}},
      {data: {id: 'niches', label: 'Niches', color: '#1B5E20', size: 42}},
      {data: {id: 'de', label: 'DE', color: '#CE93D8', size: 40}},
      {data: {id: 'integration', label: 'Integration', color: '#FFB300', size: 45}},
      {data: {id: 'foundation', label: 'Foundation\nModels', color: '#283593', size: 45}},
      {data: {id: 'recon3d', label: '3D Recon', color: '#5D4037', size: 40}},
      {data: {id: 'traj', label: 'Trajectories', color: '#00ACC1', size: 40}},
      {data: {id: 'viz', label: 'Visualization', color: '#78909C', size: 42}},

      // Segmentation tools
      {data: {id: 't_cellpose', label: 'Cellpose', color: '#E64A19', size: 25, type: 'tool', parent_cat: 'seg'}},
      {data: {id: 't_baysor', label: 'Baysor', color: '#E64A19', size: 25, type: 'tool', parent_cat: 'seg'}},
      {data: {id: 't_stardist', label: 'StarDist', color: '#E64A19', size: 25, type: 'tool', parent_cat: 'seg'}},

      // SVG tools
      {data: {id: 't_nnsvg', label: 'nnSVG', color: '#7B1FA2', size: 25, type: 'tool', parent_cat: 'svg'}},
      {data: {id: 't_spatialde', label: 'SpatialDE', color: '#7B1FA2', size: 25, type: 'tool', parent_cat: 'svg'}},
      {data: {id: 't_sparkx', label: 'SPARK-X', color: '#7B1FA2', size: 25, type: 'tool', parent_cat: 'svg'}},

      // Domain tools
      {data: {id: 't_graphst', label: 'GraphST', color: '#2E7D32', size: 25, type: 'tool', parent_cat: 'domains'}},
      {data: {id: 't_stagate', label: 'STAGATE', color: '#2E7D32', size: 25, type: 'tool', parent_cat: 'domains'}},
      {data: {id: 't_banksy', label: 'BANKSY', color: '#2E7D32', size: 25, type: 'tool', parent_cat: 'domains'}},
      {data: {id: 't_bayesspace', label: 'BayesSpace', color: '#2E7D32', size: 25, type: 'tool', parent_cat: 'domains'}},

      // Deconvolution tools
      {data: {id: 't_c2l', label: 'Cell2location', color: '#F57F17', size: 25, type: 'tool', parent_cat: 'deconv'}},
      {data: {id: 't_rctd', label: 'RCTD', color: '#F57F17', size: 25, type: 'tool', parent_cat: 'deconv'}},
      {data: {id: 't_tangram', label: 'Tangram', color: '#F57F17', size: 25, type: 'tool', parent_cat: 'deconv'}},

      // CCC tools
      {data: {id: 't_cellchat', label: 'CellChat', color: '#00695C', size: 25, type: 'tool', parent_cat: 'ccc'}},
      {data: {id: 't_commot', label: 'COMMOT', color: '#00695C', size: 25, type: 'tool', parent_cat: 'ccc'}},
      {data: {id: 't_liana', label: 'LIANA+', color: '#00695C', size: 25, type: 'tool', parent_cat: 'ccc'}},

      // Framework tools
      {data: {id: 't_squidpy', label: 'Squidpy', color: '#78909C', size: 25, type: 'tool', parent_cat: 'viz'}},
      {data: {id: 't_spatialdata', label: 'SpatialData', color: '#78909C', size: 25, type: 'tool', parent_cat: 'viz'}},
      {data: {id: 't_giotto', label: 'Giotto', color: '#78909C', size: 25, type: 'tool', parent_cat: 'viz'}},

      // Foundation model tools
      {data: {id: 't_nicheformer', label: 'Nicheformer', color: '#283593', size: 25, type: 'tool', parent_cat: 'foundation'}},
      {data: {id: 't_novae', label: 'Novae', color: '#283593', size: 25, type: 'tool', parent_cat: 'foundation'}}
    ],
    edges: [
      // Main pipeline flow
      {data: {source: 'raw', target: 'preproc'}},
      {data: {source: 'preproc', target: 'seg'}},
      {data: {source: 'seg', target: 'svg'}},
      {data: {source: 'seg', target: 'domains'}},
      {data: {source: 'seg', target: 'deconv'}},
      {data: {source: 'seg', target: 'ccc'}},
      {data: {source: 'seg', target: 'de'}},
      {data: {source: 'deconv', target: 'cellanno'}},
      {data: {source: 'deconv', target: 'ccc'}},
      {data: {source: 'ccc', target: 'niches'}},
      {data: {source: 'domains', target: 'niches'}},
      {data: {source: 'domains', target: 'recon3d'}},
      {data: {source: 'preproc', target: 'integration'}},
      {data: {source: 'seg', target: 'integration'}},
      {data: {source: 'domains', target: 'integration'}},
      {data: {source: 'preproc', target: 'foundation'}},
      {data: {source: 'seg', target: 'foundation'}},
      {data: {source: 'domains', target: 'foundation'}},
      {data: {source: 'seg', target: 'traj'}},
      {data: {source: 'domains', target: 'viz'}},
      {data: {source: 'svg', target: 'viz'}},
      {data: {source: 'deconv', target: 'viz'}},
      {data: {source: 'ccc', target: 'viz'}},
      {data: {source: 'niches', target: 'viz'}},
      {data: {source: 'de', target: 'viz'}},
      {data: {source: 'integration', target: 'viz'}},
      {data: {source: 'foundation', target: 'viz'}},
      {data: {source: 'recon3d', target: 'viz'}},
      {data: {source: 'traj', target: 'viz'}},

      // Tool-to-category links
      {data: {source: 'seg', target: 't_cellpose', type: 'tool-link'}},
      {data: {source: 'seg', target: 't_baysor', type: 'tool-link'}},
      {data: {source: 'seg', target: 't_stardist', type: 'tool-link'}},
      {data: {source: 'svg', target: 't_nnsvg', type: 'tool-link'}},
      {data: {source: 'svg', target: 't_spatialde', type: 'tool-link'}},
      {data: {source: 'svg', target: 't_sparkx', type: 'tool-link'}},
      {data: {source: 'domains', target: 't_graphst', type: 'tool-link'}},
      {data: {source: 'domains', target: 't_stagate', type: 'tool-link'}},
      {data: {source: 'domains', target: 't_banksy', type: 'tool-link'}},
      {data: {source: 'domains', target: 't_bayesspace', type: 'tool-link'}},
      {data: {source: 'deconv', target: 't_c2l', type: 'tool-link'}},
      {data: {source: 'deconv', target: 't_rctd', type: 'tool-link'}},
      {data: {source: 'deconv', target: 't_tangram', type: 'tool-link'}},
      {data: {source: 'ccc', target: 't_cellchat', type: 'tool-link'}},
      {data: {source: 'ccc', target: 't_commot', type: 'tool-link'}},
      {data: {source: 'ccc', target: 't_liana', type: 'tool-link'}},
      {data: {source: 'viz', target: 't_squidpy', type: 'tool-link'}},
      {data: {source: 'viz', target: 't_spatialdata', type: 'tool-link'}},
      {data: {source: 'viz', target: 't_giotto', type: 'tool-link'}},
      {data: {source: 'foundation', target: 't_nicheformer', type: 'tool-link'}},
      {data: {source: 'foundation', target: 't_novae', type: 'tool-link'}}
    ]
  },
  layout: {name: 'dagre', rankDir: 'TB', nodeSep: 40, rankSep: 60, padding: 30}
});

cyPipeline.on('tap', 'node', function(evt) {
  cyPipeline.elements().removeClass('highlighted faded');
  var node = evt.target;
  var neighborhood = node.neighborhood().add(node);
  neighborhood.addClass('highlighted');
  cyPipeline.elements().not(neighborhood).addClass('faded');
});
cyPipeline.on('tap', function(evt) {
  if (evt.target === cyPipeline) { cyPipeline.elements().removeClass('highlighted faded'); }
});

// === TECHNOLOGY COMPATIBILITY MAP (lazy init) ===
function initCompat() {
  var cyCompat = cytoscape({
    container: document.getElementById('cy-compat'),
    style: [
      {selector: 'node', style: {
        'label': 'data(label)', 'text-valign': 'center', 'text-halign': 'center',
        'background-color': 'data(color)', 'color': '#fff', 'font-size': '9px',
        'width': 'data(size)', 'height': 'data(size)', 'text-wrap': 'wrap', 'text-max-width': '80px',
        'border-width': 1, 'border-color': '#333'
      }},
      {selector: 'edge', style: {
        'width': 1.2, 'line-color': '#ccc', 'curve-style': 'bezier',
        'target-arrow-shape': 'none'
      }},
      {selector: 'node:selected', style: {'border-width': 3, 'border-color': '#000'}},
      {selector: '.highlighted', style: {'border-width': 3, 'border-color': '#FF5722'}},
      {selector: '.faded', style: {'opacity': 0.15}}
    ],
    elements: {
      nodes: [
        // Technology platforms
        {data: {id: 'p_visium', label: 'Visium', color: '#1976D2', size: 42, type: 'tech'}},
        {data: {id: 'p_visiumhd', label: 'Visium HD', color: '#1976D2', size: 38, type: 'tech'}},
        {data: {id: 'p_xenium', label: 'Xenium', color: '#1976D2', size: 40, type: 'tech'}},
        {data: {id: 'p_merfish', label: 'MERFISH', color: '#1976D2', size: 40, type: 'tech'}},
        {data: {id: 'p_cosmx', label: 'CosMx', color: '#1976D2', size: 38, type: 'tech'}},
        {data: {id: 'p_codex', label: 'CODEX', color: '#1976D2', size: 36, type: 'tech'}},
        {data: {id: 'p_stereoseq', label: 'Stereo-seq', color: '#1976D2', size: 38, type: 'tech'}},

        // Analysis tools
        {data: {id: 'a_c2l', label: 'Cell2location', color: '#F57F17', size: 35, type: 'tool'}},
        {data: {id: 'a_rctd', label: 'RCTD', color: '#F57F17', size: 33, type: 'tool'}},
        {data: {id: 'a_cellpose', label: 'Cellpose', color: '#E64A19', size: 35, type: 'tool'}},
        {data: {id: 'a_baysor', label: 'Baysor', color: '#E64A19', size: 33, type: 'tool'}},
        {data: {id: 'a_graphst', label: 'GraphST', color: '#2E7D32', size: 33, type: 'tool'}},
        {data: {id: 'a_stagate', label: 'STAGATE', color: '#2E7D32', size: 33, type: 'tool'}},
        {data: {id: 'a_banksy', label: 'BANKSY', color: '#2E7D32', size: 33, type: 'tool'}},
        {data: {id: 'a_commot', label: 'COMMOT', color: '#00695C', size: 33, type: 'tool'}},
        {data: {id: 'a_cellchat', label: 'CellChat', color: '#00695C', size: 33, type: 'tool'}},
        {data: {id: 'a_nnsvg', label: 'nnSVG', color: '#7B1FA2', size: 33, type: 'tool'}},
        {data: {id: 'a_tangram', label: 'Tangram', color: '#F57F17', size: 33, type: 'tool'}},
        {data: {id: 'a_squidpy', label: 'Squidpy', color: '#78909C', size: 35, type: 'tool'}},
        {data: {id: 'a_spatialdata', label: 'SpatialData', color: '#78909C', size: 35, type: 'tool'}},
        {data: {id: 'a_bayesspace', label: 'BayesSpace', color: '#2E7D32', size: 30, type: 'tool'}},
        {data: {id: 'a_spatialde', label: 'SpatialDE', color: '#7B1FA2', size: 30, type: 'tool'}},
        {data: {id: 'a_stardist', label: 'StarDist', color: '#E64A19', size: 33, type: 'tool'}}
      ],
      edges: [
        // Visium compatibility (spot-based, widely supported)
        {data: {source: 'p_visium', target: 'a_c2l'}},
        {data: {source: 'p_visium', target: 'a_rctd'}},
        {data: {source: 'p_visium', target: 'a_tangram'}},
        {data: {source: 'p_visium', target: 'a_graphst'}},
        {data: {source: 'p_visium', target: 'a_stagate'}},
        {data: {source: 'p_visium', target: 'a_banksy'}},
        {data: {source: 'p_visium', target: 'a_bayesspace'}},
        {data: {source: 'p_visium', target: 'a_nnsvg'}},
        {data: {source: 'p_visium', target: 'a_spatialde'}},
        {data: {source: 'p_visium', target: 'a_squidpy'}},
        {data: {source: 'p_visium', target: 'a_spatialdata'}},
        {data: {source: 'p_visium', target: 'a_cellchat'}},

        // Visium HD
        {data: {source: 'p_visiumhd', target: 'a_banksy'}},
        {data: {source: 'p_visiumhd', target: 'a_graphst'}},
        {data: {source: 'p_visiumhd', target: 'a_squidpy'}},
        {data: {source: 'p_visiumhd', target: 'a_spatialdata'}},
        {data: {source: 'p_visiumhd', target: 'a_nnsvg'}},

        // Xenium (single-cell imaging)
        {data: {source: 'p_xenium', target: 'a_cellpose'}},
        {data: {source: 'p_xenium', target: 'a_baysor'}},
        {data: {source: 'p_xenium', target: 'a_banksy'}},
        {data: {source: 'p_xenium', target: 'a_commot'}},
        {data: {source: 'p_xenium', target: 'a_squidpy'}},
        {data: {source: 'p_xenium', target: 'a_spatialdata'}},
        {data: {source: 'p_xenium', target: 'a_cellchat'}},
        {data: {source: 'p_xenium', target: 'a_stardist'}},

        // MERFISH
        {data: {source: 'p_merfish', target: 'a_cellpose'}},
        {data: {source: 'p_merfish', target: 'a_baysor'}},
        {data: {source: 'p_merfish', target: 'a_banksy'}},
        {data: {source: 'p_merfish', target: 'a_commot'}},
        {data: {source: 'p_merfish', target: 'a_squidpy'}},
        {data: {source: 'p_merfish', target: 'a_cellchat'}},
        {data: {source: 'p_merfish', target: 'a_stardist'}},

        // CosMx
        {data: {source: 'p_cosmx', target: 'a_cellpose'}},
        {data: {source: 'p_cosmx', target: 'a_baysor'}},
        {data: {source: 'p_cosmx', target: 'a_squidpy'}},
        {data: {source: 'p_cosmx', target: 'a_commot'}},
        {data: {source: 'p_cosmx', target: 'a_cellchat'}},
        {data: {source: 'p_cosmx', target: 'a_stardist'}},

        // CODEX (protein)
        {data: {source: 'p_codex', target: 'a_cellpose'}},
        {data: {source: 'p_codex', target: 'a_stardist'}},
        {data: {source: 'p_codex', target: 'a_squidpy'}},
        {data: {source: 'p_codex', target: 'a_banksy'}},

        // Stereo-seq
        {data: {source: 'p_stereoseq', target: 'a_cellpose'}},
        {data: {source: 'p_stereoseq', target: 'a_graphst'}},
        {data: {source: 'p_stereoseq', target: 'a_stagate'}},
        {data: {source: 'p_stereoseq', target: 'a_banksy'}},
        {data: {source: 'p_stereoseq', target: 'a_nnsvg'}},
        {data: {source: 'p_stereoseq', target: 'a_squidpy'}}
      ]
    },
    layout: {name: 'cose', idealEdgeLength: 130, nodeRepulsion: 10000, padding: 40, nodeOverlap: 20}
  });

  cyCompat.on('tap', 'node', function(evt) {
    cyCompat.elements().removeClass('highlighted faded');
    var node = evt.target;
    var neighborhood = node.neighborhood().add(node);
    neighborhood.addClass('highlighted');
    cyCompat.elements().not(neighborhood).addClass('faded');
  });
  cyCompat.on('tap', function(evt) {
    if (evt.target === cyCompat) { cyCompat.elements().removeClass('highlighted faded'); }
  });
}
</script>

---

## About These Maps

### Pipeline Map

Shows the **data flow** through a typical spatial omics analysis pipeline. Nodes are colored by analysis step, and directed edges indicate which steps feed into each other. Small tool nodes branch off each category to show representative methods.

Key patterns visible in the map:

- **Segmentation is the central hub** -- most downstream analyses (SVG detection, spatial domains, deconvolution, CCC, DE) depend on cell-level data from segmentation
- **Multiple paths to niches** -- niche identification can be reached via spatial domains or CCC, reflecting different analytical philosophies
- **Convergence at visualization** -- all analysis steps ultimately feed into visualization and interpretation frameworks like Squidpy and SpatialData

### Technology Compatibility Map

Shows which **analysis tools** have been validated with which **technology platforms**. Edges indicate documented compatibility in the tool's paper or documentation.

Key patterns:

- **Visium has the broadest tool support** -- as the most widely adopted platform, nearly every tool has been tested on Visium data
- **Imaging-based platforms (Xenium, MERFISH, CosMx) share segmentation tools** -- Cellpose, Baysor, and StarDist work across all single-molecule resolution platforms
- **Spot-based tools (BayesSpace, RCTD, Cell2location) cluster around Visium** -- these methods were designed for the multi-cell spot resolution
- **Framework tools (Squidpy, SpatialData) are technology-agnostic** -- they connect to nearly every platform
