# GUI User Manual

The Cocoabox GUI was designed with a range of application areas in mind. It allows you to analyse time series relating to the economy, the climate, the human brain, or any other complex system. For the purposes of this manual, let's suppose that you have resting-state [functional magnetic resonance imaging (fMRI)](https://en.wikipedia.org/wiki/Functional_magnetic_resonance_imaging) measurements of two groups of subjects, would like to compute their connectivity matrices, and see whether there is a statistically significant difference between the two groups. 

The following dataset, made publicly available by our group, can be imported into the GUI without further modifications, thus making it simple to quickly test the GUI on real data: https://osf.io/7ekmw/. The dataset contains preprocessed resting-state fMRI data of 64 patients with multiple sclerosis and 65 healthy controls, sorted by the group (patients first), stored in a 3D matrix (timepoints * ROIs * subjects), of equal length for both groups (same number of timepoints).

<p align="center">
    <img src="/GUI_screenshot.png" alt="GUI screenshot" width="65%" height="65%">
</p>

Each tab on the left side of the GUI window presents a step of the data analysis pipeline. We recommend going through the tabs in the default order (top to bottom), as described below. Some of the steps (e.g. Preprocessing) are optional - you may choose to skip them, depending on your data and objective. Optional steps can be distinguished by a darker grey tab-background colour. Visualisations can be generated at various steps of the analysis.

Pressing a button (e.g. "Label data", "Compute") performs the given operation or analysis using the parameter values specified above the button. The default values for each parameter can be changed by the user.

Create a subfolder named "Data" within the main cocoabox folder, and move any .mat files to be imported there.

--- 

## Analysis steps (GUI tabs)

### DATA IMPORT

Options:
- **Data source**: import own data, import matrix/matrices and generate data, generate data from scratch

The parameters to be specified depend on the data source selected at the top.

#### A. DATA SOURCE: import own data

Select this option to import and analyse your own time series.

`Import data` -> load .mat file with the data to be analysed

The input data is currently expected to be:
- preprocessed
- representing two groups of subjects
- sorted by the group (with data for group 1 coming first, followed by data for group 2)
- stored in a 3D matrix (timepoints * ROIs * subjects)
- of equal length for both groups (same number of timepoints)

#### B. DATA SOURCE: import matrix/matrices and generate data

Select this option to import a matrix or a set of matrices from which the time series to be analysed will be generated.

`Import matrix/matrices` -> load .mat file containing a matrix or a set of matrices from which the data will be generated

The input is expected to be:\
a) of size n * n - to generate the time series for each realisation from the same matrix; with number of realisations specified below\
b) of size n * n * R - to generate the time series for each realisation from a different matrix; with R = number of realisations\
Each matrix should be a square real matrix with largest (in absolute value) eigenvalue < 1 (for stationarity of the process).

Parameters:
- **Number of matrices** - the number of matrices that have been imported (filled in automatically)
- **Number of timepoints** - the length of the time series to be generated
- **Number of regions** - the number of regions to be generated (filled in automatically; given by n - the size of the matrix)
- **Number of realisations** - the number of realisations (subjects) to be generated (filled in automatically if Number of matrices > 1; given by R - the number of matrices in the set)

#### C. DATA SOURCE: generate data from scratch

Select this option to *generate* one or more matrices from which the time series to be analysed will be generated in turn.

Parameters:
- **Number of matrices** - the number of matrices to be used for data generation
- **Number of timepoints** - the length of the time series to be generated
- **Number of regions** - the number of regions to be generated
- **Number of realisations** (subjects) - the number of realisations (subjects) to be generated (filled in automatically if Number of matrices > 1; given by the number of matrices)<br><br>
- **Matrix density** - percentage of nonzero elements
- **Largest eigenvalue** - normalisation parameter specifying the biggest eigenvalue of the matrix (max(abs(eig(matrix)))
- **symmetric** - checked: make all matrices symmetric; unchecked: make all matrices nonsymmetric
- **autocorrelated** - checked: make all matrices autocorrelated; unchecked: make all matrices not autocorrelated

*Note: If Number of matrices = 1, the time series for each realisation will be generated from the same matrix. If Number of matrices > 1, the time series for each realisation will be generated from a different matrix. The number of realisations is then given by the number of matrices.*

#### Data labeling
Enter dataset and group **Labels** (optional, to be used in visualisations), and the subject **Count** for each group. 

#### Subset selection (optional)
To select a subset of the data, enter the indices of **Timepoints**, **Regions**, and/or **Realisations** (subjects) in the respective text fields (as comma-separated lists, numeric ranges, or a combination of the two), and use the switch button to indicate whether they are to be included or excluded. 

`Reset` -> return to original data. (To create a new subset of the original data, click on `Reset` first).

*Note: Make sure current dimensions of your data (displayed in the text area at the bottom of the GUI window) agree with the expected dimensions before proceeding to the next step.*

*Warning: If you intend to compute connectivity matrices and perform statistical inference, and/or graph theory analysis, make sure the subsets are not too small, and contain at least one realisation (subject) per group.*

---

### PREPROCESSING (optional)

*Coming soon*

---

### DIMENSIONALITY REDUCTION (optional)

Reduces dimensionality of the data using the selected decomposition or clustering method. 

Parameters:
- **Method**: PCA (default), ICA, NNMF (non-negative matrix factorisation), k-means clustering
- **Number of components** (for decomposition) / **Number of clusters** (for clustering)
- **Dimension**: temporal, spatial (k-means: only temporal)

`Reset` -> undo dimensionality reduction.

*Note: Dimensionality reduction should be performed on normalised data.*

#### Visualisation (optional)

Parameters:
- **Method**: individual (default), summarised

The "individual" method will plot each region separately. The time series of each subject will be displayed on a separate figure, and split into several figures if their count exceeds 15. If more than 30 figures are to be shown, the method will automatically change to "summarised". Method "summarised" will create a single plot for all regions.

Optionally: change the default indices of **Timepoints**, **Components**/**Clusters**, and/or **Realisations** (subjects) to be visualised in the respective text fields (as comma-separated lists, numeric ranges, or a combination of the two), and use the switch button to indicate whether those are to be included or excluded. 

*Note: If you are working with a subset of the original data, the indices to be visualised refer to the subset, not to the original data (i.e. the subset to be visualised is a subset of the subset).*

*Warning: If you intend to compute connectivity matrices and perform statistical inference, and/or graph theory analysis, make sure that Number of clusters > 1.*

---

### CONNECTIVITY MATRICES

Computes the selected functional or effective connectivity measure.

Parameters:
- **Connectivity measure**: Pearson correlation (default), partial Pearson correlation, Spearman correlation, mutual information, Granger causality

*Note: Clicking on "Compute" repeatedly will overwrite previous results, allowing you to try various connectivity measures.*

#### Visualisation (optional)

**Options**:
- first matrix (default) - connectivity matrix of the first realisation (subject)
- number of matrices + **Matrices count** - specify the number of matrices to be visualised. Entering "10", for example, will visualise the connectivity matrices of the first and last 5 realisations (subjects).
- selected matrices + **Matrices** - enter the indices of connectivity matrices to be visualised (as comma-separated lists, numeric ranges, or a combination of the two).

*Note: If you are working with a subset of the original data, the indices to be visualised refer to the subset, not to the original data (i.e. the subset to be visualised is a subset of the subset).* 

---

### STATISTICAL INFERENCE

Performs statistical analysis to compare group connectivity. The parameters to be specified depend on the statistical analysis method selected at the top.

#### A. METHOD: p-value masking

For element-wise comparison of group connectivity. Finds regions with significant differences between groups. The group mean values of every element are compared using the selected group comparison test. 

Parameters:
- **Group comparison test**: Mann-Whitney U-test (default; non-parametric), t-test (parametric)
- **Multiple correction method**: FWE - Hochberg's procedure (default), FWE - Bonferroni correction, FDR (Benjamini-Hochberg procedure), none (no multiple correction)
- **alpha threshold** (default: 0.05)

#### B. METHOD: ROC 

For comparing group connectivity by a 1-dimensional feature. For the selected feature, the ROC curve is computed for classification based on a set of thresholds. AUC (Area Under Curve) is calculated. The group mean values of the feature are compared using the Mann-Whitney U-test. 

**Features:**
- global connectivity (default) - averaged connectivity per realisation (subject)
- typicality of functional connectivity (TFC) - computed as the Pearson correlation between every realisation's (subject's) connectivity matrix and the gold standard matrix; needs gold standard matrix
- mask connectivity - selects elements based on a mask; needs mask matrix
- average mask connectivity - averaged connectivity per subject, after masking; needs mask matrix 
- PCA - coefficient of the first PCA component of every realisations (subject's) connectivity matrix 	

*Note: For some of the features, uploading an **Additional matrix** - a Boolean mask or a "gold standard" matrix (the TFC matrix of the control group) is necessary. The additional matrix should be 2D (regions * regions), and saved in the "Data" subfolder of the main cocoabox folder.*

---

### GRAPH THEORY ANALYSIS (optional)

Computes the following graph theoretical properties of (symmetric) connectivity matrices: clustering coefficient, characteristic path length, small-world coefficient, efficiency, transitivity, and assortativity. Performs a statistical comparison of the difference between the two groups (for each graph theoretical property), using the Mann-Whitney U-test.

Parameters:
- **Threshold density** - the thresholding density (percentage of nonzero elements) to which the input connectivity matrices are first thresholded before the graph theoretical properties are computed (default: 0.25)
- **alpha threshold** - p-value for the statistical test (default: 0.05)

*Note: Graph theory analysis currently only works for symmetric matrices of size > 1. If the connectivity matrices are too small, or not symmetric, all components of this tab are automatically disabled.*
