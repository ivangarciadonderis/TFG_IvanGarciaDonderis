# Multi-omic reanalysis of celiac disease studies

This repository contains the data-processing and statistical-analysis workflow developed for the Bachelor's Thesis **“Estudio de la enfermedad celíaca a través de datos ómicos y metodologías de ciencia de datos”**.

The project reanalyses three published studies on celiac disease using a common and reproducible data-science framework. The analyses include microbiome, metabolome and transcriptome data and combine exploratory multivariate analysis, longitudinal modelling, multi-omic integration and functional enrichment.

> This project was developed as a Bachelor's Thesis and is also published by the [BiostatOmics](https://github.com/BiostatOmics) research group in [BiostatOmics/TFG_IvanGarciaDonderis](https://github.com/BiostatOmics/TFG_IvanGarciaDonderis). This repository is the author's personal copy.

## Studies included

The repository contains the reanalysis of the following studies:

1. **Leonard et al. (2021)**  
   *Microbiome signatures of progression toward celiac disease onset in at-risk children in a longitudinal prospective cohort study.*  
   Proceedings of the National Academy of Sciences, 118(29), e2020322118.  
   DOI: 10.1073/pnas.2020322118

2. **Leonard et al. (2020)**  
   *Multi-omics analysis reveals the influence of genetic and environmental risk factors on developing gut microbiota in infants at risk of celiac disease.*  
   Microbiome, 8, 130.  
   DOI: 10.1186/s40168-020-00906-w

3. **Loberman-Nachum et al. (2019)**  
   *Defining the Celiac Disease Transcriptome using Clinical Pathology Specimens Reveals Biologic Pathways and Supports Diagnosis.*  
   Scientific Reports, 9, 16163.  
   DOI: 10.1038/s41598-019-52733-1

## Repository structure

```text
TFG/
├── data/
│   ├── raw/
│   │   ├── leonard_2020/
│   │   ├── leonard_2021/
│   │   └── loberman_nachum/
│   └── processed/
│       ├── leonard_2020/
│       ├── leonard_2021/
│       └── loberman_nachum/
│
├── scripts/
│   ├── leonard_2020/
│   ├── leonard_2021/
│   └── loberman_nachum/
│
├── README.md
├── LICENSE
├── .gitignore
├── TFG.Rproj
└── .here
```

- `data/raw/` contains the original public data and supplementary files used in the analyses.
- `data/processed/` contains intermediate and final objects generated during the workflow.
- `scripts/` contains the R Markdown files used for data loading, preprocessing and statistical analysis.
- The generated `.html` files provide rendered versions of the corresponding R Markdown analyses.

## Main methods

The common analytical framework includes:

- data cleaning, filtering and transformation;
- centred log-ratio (CLR) transformation for compositional microbiome data;
- log transformation of metabolomic data;
- RNA-seq filtering and TMM normalisation;
- Principal Component Analysis (PCA);
- outlier exploration using residual distances and Hotelling's T²;
- Linear Mixed Models (LMM) for longitudinal data;
- Partial Least Squares (PLS2) for microbiome-metabolome integration;
- Partial Least Squares Discriminant Analysis (PLS-DA);
- variable selection using VIP, Jackknife and permutation procedures;
- functional enrichment using ToppGene.

## Software requirements

The analyses were developed in **R** using R Markdown.

Main R packages used across the project include:

```r
install.packages(c(
  "tidyverse",
  "readxl",
  "knitr",
  "rmarkdown",
  "here",
  "compositions",
  "gridExtra",
  "ggplot2",
  "igraph",
  "tidygraph",
  "ggraph",
  "lme4",
  "lmerTest",
  "remotes"
))
```

`NOISeq` is distributed through Bioconductor:

```r
if (!requireNamespace("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

BiocManager::install("NOISeq")
```

The multivariate analyses use the **PLSandO** package from the BiostatOmics GitHub organisation:

```r
remotes::install_github("BiostatOmics/PLSandO")
```

## Reproducing the analyses

The project should be opened from `TFG.Rproj` or executed with the repository root as the project root. Paths are defined relative to the project using the `here` package.

### Leonard et al. (2021)

Recommended execution order:

```text
scripts/leonard_2021/01_load_data.Rmd
scripts/leonard_2021/02_preprocessing.Rmd
scripts/leonard_2021/03_PCA.Rmd
scripts/leonard_2021/04_LMM.Rmd
scripts/leonard_2021/05_PLS2.Rmd
```

### Leonard et al. (2020)

Recommended execution order:

```text
scripts/leonard_2020/01_load_data.Rmd
scripts/leonard_2020/02_preprocessing.Rmd
scripts/leonard_2020/03_PCA.Rmd
scripts/leonard_2020/04_LMM.Rmd
scripts/leonard_2020/05_PLS2.Rmd
```

### Loberman-Nachum et al. (2019)

Recommended execution order:

```text
scripts/loberman_nachum/01_load_data.Rmd
scripts/loberman_nachum/02_preprocessing.Rmd
scripts/loberman_nachum/03_PCA.Rmd
scripts/loberman_nachum/04_PLS-DA.Rmd
scripts/loberman_nachum/05_functional_enrichment.Rmd
```

## ToppGene enrichment step

Functional enrichment for the transcriptomic study includes an external ToppGene step.

`04_PLS-DA.Rmd` generates the following gene lists:

```text
toppgene_input_celiac_jk.txt
toppgene_input_control_jk.txt
toppgene_input_celiac_perm.txt
toppgene_input_control_perm.txt
```

These lists are analysed in ToppGene. The exported ToppGene results are stored in:

```text
data/processed/loberman_nachum/
```

using the following filenames:

```text
toppgene_celiac_jk.txt
toppgene_control_jk.txt
toppgene_celiac_perm.txt
toppgene_control_perm.txt
```

`05_functional_enrichment.Rmd` reads these files and performs the comparison with the functional results reported in the original study.

The processed ToppGene output files are included in the repository so that the downstream analysis can be reproduced without repeating the web-based enrichment step.

## Reproducibility notes

- Raw public datasets are preserved separately from processed data.
- Intermediate `.rds` objects are included to make individual analysis stages easier to reproduce and inspect.
- Randomised procedures used in PLS analyses use fixed seeds where required.
- The analysis scripts do not modify the original raw files.
- Results are exploratory and should not be interpreted as clinical diagnostic models or validated biomarkers.

## Data availability

All analyses are based on previously published and publicly available datasets.

The transcriptomic data from Loberman-Nachum et al. are available through the Gene Expression Omnibus under accession **GSE131705**. The Leonard et al. datasets are derived from the supplementary material accompanying the corresponding publications.

No new participant data were collected for this project.

## Author

**Iván García Donderis**  
Bachelor's Degree in Data Science  
Universitat Politècnica de València

## Academic supervision

Supervised by **Sonia Tarazona**.

## License

The code developed for this project is released under the [MIT License](LICENSE).

The original datasets are not covered by this license and remain subject to the terms and conditions of their respective repositories and publications.
