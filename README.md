# PBMC Single-Cell RNA-seq Analysis Pipeline  
**scvi-tools + py_monocle edition**
!(output_0_15.png)
---

Welcome to a modern, reproducible, and highly customizable pipeline for single-cell RNA-seq (scRNA-seq) analysis. This workflow is designed to take you from raw 10x-style data downloads through to cell type annotation, differential expression, pseudo-bulk analysis, and pseudotime inference—all in a single script.

---

## The Story: From Raw Data to Biological Insight

Imagine you have just downloaded several PBMC (peripheral blood mononuclear cell) single-cell datasets from GEO. You want to:

- Merge and harmonize these datasets, correcting for batch effects.
- Filter out low-quality cells and genes to ensure robust results.
- Cluster cells and identify cell types using marker genes.
- Compare conditions (e.g., pre- vs. post-treatment) within each cell type.
- Summarize gene expression at the pseudo-bulk level for downstream analyses.
- Infer cellular trajectories to understand dynamic processes.

This pipeline does all of that—and more—with clear code, modern algorithms, and flexibility for your own research questions.

---

## How Does the Pipeline Work?

### 1. Automated Data Download and Merging
You provide a list of GEO samples (with accession numbers and file prefixes). The pipeline fetches the raw 10x files, decompresses them, and merges everything into a single AnnData object. No more manual downloads or format headaches!

### 2. Quality Control (QC)
Cells and genes are filtered based on customizable thresholds for:
- Minimum genes per cell
- Minimum cells per gene
- Maximum mitochondrial content
- Total UMI counts per cell

This ensures that only high-quality data moves forward.

### 3. Dimensionality Reduction with scvi-tools
Batch effects? Not a problem. The pipeline uses a deep generative model (scVI) to create a batch-corrected latent space, ideal for clustering and visualization.

### 4. Clustering and Marker Detection
Cells are clustered (Leiden algorithm), and marker genes for each cluster are identified using robust statistics.

### 5. Automated Cell Type Annotation
A customizable dictionary of marker genes is used to annotate clusters. You can easily edit this dictionary to match your tissue or organism of interest.

### 6. Differential Expression Analysis
Within each cluster, the pipeline compares gene expression between conditions (e.g., pre vs. post), outputting tables of significant genes.

### 7. Pseudo-bulk Utilities
Aggregate counts across clusters or conditions for downstream analyses (e.g., pathway enrichment, bulk-like comparisons).

### 8. Pseudotime Inference
Using py_monocle, the pipeline infers cellular trajectories, helping you visualize dynamic biological processes.

### 9. Results and Visualization
All results are saved in a single AnnData file, with additional tables and plots for easy interpretation.

---

## Getting Started

### Requirements

Install the following Python packages (via conda or pip):

- scanpy
- anndata
- scvi-tools
- py-monocle
- seaborn
- matplotlib
- pandas
- numpy
- scipy
- scikit-learn

---

## Input: Sample Metadata

Edit the `sample_data` list in the script to include your GEO samples and metadata (condition, subject, label).  
**Example:**

| accession   | file_prefix           | condition | subject | label   |
|-------------|----------------------|-----------|---------|---------|
| GSM6611295  | GSM6611295_P15306_5001 | pre       | 1       | s1_pre  |
| GSM6611296  | GSM6611296_P15306_5002 | post      | 1       | s1_post |
| ...         | ...                  | ...       | ...     | ...     |

---

## Customization: Make It Yours

### Quality Control Parameters

You can tune these in the `basic_qc()` function:

| Parameter   | Purpose                          | Default  |
|-------------|----------------------------------|----------|
| min_genes   | Minimum genes per cell           | 300      |
| min_cells   | Minimum cells per gene           | 20       |
| max_mito    | Maximum % mitochondrial reads    | 15       |
| min_counts  | Minimum total counts per cell    | 1,000    |
| max_counts  | Maximum total counts per cell    | 15,000   |

### Clustering and Latent Space

- `n_latent`: Number of latent dimensions for scVI (default: 30)
- `batch_key`: Metadata column for batch correction (default: 'batch')
- `resolution`: Leiden clustering resolution (default: 0.5)

### Cell Type Annotation

Edit the `marker_db` dictionary to add or change marker genes for your cell types. Adjust `top_n` to consider more/fewer top markers per cluster.

### Differential Expression

- `p_cut`: Adjusted p-value cutoff (default: 0.05)
- `min_cell_threshold`: Minimum cells per group for DE (default: 5)

### Pseudo-bulk and Pseudotime

- `groupby`: Aggregate counts by any metadata column (e.g., clusters, condition)
- `root_cluster`: Set the root for pseudotime inference (default: '0')
- `k_nn`: Number of neighbors for trajectory inference (default: 25)

---

## Output: What You Get

- `PBMC_scvi_monocle.h5ad`: All results in one AnnData file.
- `de_results/`: Differential expression tables for each cluster.
- `pseudobulk_by_cluster.csv`: Pseudo-bulk count matrix.
- **Plots:** UMAPs, dotplots, heatmaps, pseudotime visualizations (displayed; can be saved).

---

## Extending the Pipeline

- Add more cell types or markers to the annotation dictionary.
- Change clustering or dimensionality reduction methods.
- Integrate more samples or different conditions.
- Use pseudo-bulk outputs for pathway or network analysis.
- Customize plots for publication.

---

## Troubleshooting & Tips

- **GEO download issues:** Double-check accession numbers and file prefixes.
- **Empty clusters?** Adjust clustering resolution or QC thresholds.
- **Annotation not matching?** Update marker genes or increase `top_n`.
- **Missing dependencies?** Ensure all required packages are installed.

---

## Why This Pipeline Helps You as a Bioinformatician

- **Saves time:** Automates tedious steps like downloading, merging, and QC.
- **Reproducible:** All parameters are explicit and easy to change.
- **Modern:** Uses best-in-class algorithms for normalization, batch correction, and trajectory inference.
- **Flexible:** Adapt to any tissue, organism, or experimental design.
- **Extensible:** Build on top of this workflow for your own research questions.

---

*Happy single-cell analysis!*

---

Let us know if you want a shorter version, example outputs, or usage screenshots.  
For questions, suggestions, or contributions, please open an issue or pull request on GitHub.
