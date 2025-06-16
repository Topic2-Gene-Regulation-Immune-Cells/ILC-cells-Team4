# Conda Usage

## Conda Setup
To create the conda environment run
```
conda env create -f environment.yml
```
or update it using
```
conda env update --file environment.yml --prune
```

To test if the environment was installed successfully use
 ```
 conda activate data_analysis
 conda list
 ```
 
## Installing new packages
First make sure that you have the current version of *enivronment.yml* by syncing via the Source Control tab in VS Code. Then update your conda environment to the newest version by running
```
conda env update --file environment.yml --prune
```
in the terminal. After this you can install a new package by executing
```
conda activate data_analysis
conda install *thepackageyouwanttoadd*
```  
If the installation was successfull you can now update the *enivronment.yml* via
```
conda export --no-builds -f environment.yml
```
Then commit (stating the package(s) you added) and sync the updated *enivronment.yml*.

# Project Task Checklist

## 1. Sample-Level Quality Control
- **ATAC-seq QC:**
  - Compute per-cell mean, median, standard deviation of accessibility.
  - Correlate these metrics with QC metadata (PF.reads, TSS.enrichment, InputCellNumber).
  - Plot distributions (histograms, scatterplots) of accessibility metrics.
  - Identify and remove or normalize outlier samples.
- **RNA-seq QC:**
  - Compute per-cell mean, median, standard deviation, and zero-expression fraction.
  - Correlate expression metrics with QC metadata (PF.reads, InputCellNumber).
  - Plot distributions of expression metrics.
  - Identify and remove or normalize outlier samples.

## 2. Peak-Level Quality & Annotation
- Compute per-peak statistics (mean accessibility, variance, coefficient of variation).
- Filter out peaks with low mean accessibility or low variance.
- Annotate peaks as promoters vs. enhancers (±1 kb TSS overlap).
- Calculate distance of each peak to the nearest TSS.
- Annotate peaks as intronic vs. intergenic using exon coordinates.
- Plot peak-level distributions (promoter vs enhancer, distance vs accessibility, intronic vs intergenic).

## 3. Chromatin Landscape Analysis
- Compare promoter vs. enhancer accessibility (boxplots, statistical tests).
- Analyze relationship between peak accessibility and TSS distance (scatterplot, correlation).
- Compare intronic vs. intergenic peak accessibility.

## 4. Cell-Type Clustering
- Perform dimensionality reduction (PCA or UMAP) on the ATAC-seq matrix.
- Hierarchical clustering of cell–cell correlation matrix for ATAC-seq.
- Repeat PCA/UMAP and clustering on the RNA-seq matrix.
- Quantify clustering quality (silhouette score, intra- vs. inter-lineage correlations).

## 5. CRE Module Discovery
- Standardize each peak’s accessibility profile (row-wise z-score).
- Cluster peaks into modules (k-means or hierarchical clustering).
- Visualize modules as heatmaps and centroid profiles.
- Annotate modules by lineage specificity and perform motif enrichment.

## 6. ATAC–RNA Integration & Modeling
- Link peaks to genes by proximity and correlation of accessibility vs. expression.
- Build multivariate regression models predicting gene expression from associated peaks.
- Summarize variance explained (R² distribution) and identify key peaks.
- Classify peaks as activating vs. repressing based on regression coefficients.

## 7. Network Inference & TF Analysis
- Identify TF motifs in CREs and integrate with TF expression data.
- Construct directed TF → CRE → gene regulatory networks.
- Rank TFs by network centrality or variance explained.
- Visualize key regulatory circuits.

## 8. Reporting & Presentation
- Compile QC, clustering, module, regression, and network figures into final report.
- Draft methods, results, and discussion sections.
- Prepare poster for final presentation.

## Milestone 1: Data Preparation & QC

| Issue                                                         | Assignee     | Description                                                                          |
|---------------------------------------------------------------|--------------|--------------------------------------------------------------------------------------|
| Collapse ATAC replicates by CellType                          | ATAC_QC      | Group ATAC‐seq columns by CellType and average replicates.                           |
| Collapse RNA replicates by CellType                           | RNA_QC       | Group RNA‐seq columns by CellType and average replicates.                            |
| Compute ATAC sample-level QC metrics                          | ATAC_QC      | Calculate mean/median/std accessibility per cell; merge QC metrics from metadata.    |
| Compute RNA sample-level QC metrics                           | RNA_QC       | Calculate mean/median/std expression and zero-fraction per cell; merge QC metrics.   |
| Plot ATAC QC distributions                                    | ATAC_QC      | Histograms and scatterplots of ATAC QC metrics.                                      |
| Plot RNA QC distributions                                     | RNA_QC       | Histograms and scatterplots of RNA QC metrics.                                       |

---

## Milestone 2: Peak-Level Analysis

| Issue                                                         | Assignee     | Description                                                                          |
|---------------------------------------------------------------|--------------|--------------------------------------------------------------------------------------|
| Compute per-peak accessibility statistics                     | PEAK_ANNOT   | Calculate mean, variance, CV for each peak; save `peak_stats.csv`.                   |
| Filter low-signal/low-variance peaks                           | PEAK_ANNOT   | Remove peaks below mean/accessibility and variance thresholds.                       |
| Annotate peaks as promoters vs enhancers                      | PEAK_ANNOT   | Label peaks overlapping ±1kb of TSS as promoters, others as enhancers.               |
| Compute distance to nearest TSS for each peak                 | PEAK_ANNOT   | Calculate `dist_to_tss` and add to `peak_stats.csv`.                                 |
| Annotate intronic vs intergenic peaks                         | PEAK_ANNOT   | Label peaks within exons as intronic, others as intergenic.                          |
| Visualize peak-level distributions                            | PEAK_ANNOT   | Boxplots and scatterplots comparing promoters, enhancers, intronic vs intergenic.    |

---

## Milestone 3: Clustering & Module Discovery

| Issue                                                         | Assignee       | Description                                                                          |
|---------------------------------------------------------------|----------------|--------------------------------------------------------------------------------------|
| Perform PCA/UMAP on ATAC matrix                               | CLUSTER_MODEL  | Run PCA/UMAP on `atac_by_celltype.csv`; save embeddings and plots.                   |
| Hierarchical clustering on ATAC correlation matrix             | CLUSTER_MODEL  | Compute cell–cell Pearson correlation heatmap.                                       |
| Perform PCA/UMAP on RNA matrix                                | CLUSTER_MODEL  | Run PCA/UMAP on `rna_by_celltype.csv`; save embeddings and plots.                    |
| Hierarchical clustering on RNA correlation matrix              | CLUSTER_MODEL  | Compute sample–sample RNA correlation heatmap.                                       |
| Define CRE modules via k-means                                 | CLUSTER_MODEL  | Cluster row-standardized peaks into modules (k≈50); save module assignments.         |
| Visualize CRE modules & centroid profiles                      | CLUSTER_MODEL  | Heatmaps of CRE modules and line plots of module centroids.                         |

---

## Milestone 4: ATAC–RNA Integration & Network Inference

| Issue                                                         | Assignee       | Description                                                                          |
|---------------------------------------------------------------|----------------|--------------------------------------------------------------------------------------|
| Link peaks to genes by proximity and correlation              | PEAK_ANNOT     | Assign peaks to nearby genes and compute accessibility–expression correlation.       |
| Build regression models for gene expression                   | CLUSTER_MODEL  | Fit multivariate linear models: expression ~ accessibility of associated peaks.      |
| Summarize variance explained by CREs                          | CLUSTER_MODEL  | Plot R² distribution; identify top CRE explainers.                                   |
| Identify activating vs repressing CREs                        | PEAK_ANNOT     | Classify CREs based on regression coefficients (positive vs negative).               |
| Perform TF motif enrichment in lineage-specific modules       | PEAK_ANNOT     | Run motif enrichment per module; save top motifs.                                    |
| Construct TF → CRE → gene regulatory network                  | CLUSTER_MODEL  | Integrate motif, accessibility, and expression to build and visualize networks.      |

---

## Milestone 5: Reporting & Presentation

| Issue                                                         | Assignee       | Description                                                                          |
|---------------------------------------------------------------|----------------|--------------------------------------------------------------------------------------|
| Compile QC & clustering figures into report                   | CLUSTER_MODEL  | Collect all figures into a PDF or markdown report.                                   |
| Draft methods section                                         | PEAK_ANNOT     | Document all data processing, QC, annotation, and analysis methods.                  |
| Draft results & discussion                                    | RNA_QC         | Summarize findings: variability, clusters, CRE modules, regression, networks.        |
| Develop slide deck for project proposal                       | CLUSTER_MODEL  | Create 10-minute proposal slides.                                                    |
| Develop slide deck for final presentation                     | CLUSTER_MODEL  | Prepare 15-minute final presentation slides.                                         |

---

**Roles:**
- **ATAC_QC:** ATAC-seq Quality Control & Exploration  
- **RNA_QC:** RNA-seq Quality Control & Exploration  
- **PEAK_ANNOT:** Peak Annotation & Integration  
- **CLUSTER_MODEL:** Clustering, Modeling, Visualization & Reporting 

# ILC-cells-Team4
This is a repository for the current team that the students will work on &amp; submit.


Dear Team4, 

you can use this ReadMe file to construct your project, summarise ideas, and present results. 

The project guideline and requirements were kindly summarised by Alexander [here](https://github.com/maiwen-ch/2025_Data_Analysis_Topic_02_Gene_Regulation_of_Immune_Cells). Please read through this repo very carefully and start gathering your own ideas!

You will have to submit a Jupyter Notebook with your code & plotting/results, so make sure we can find it in this repository at the end. 

Also, do not forget to clean up this ReadMe and edit it, so that any external member of the course could read & comprehend what you did in your project. 

Good luck! 