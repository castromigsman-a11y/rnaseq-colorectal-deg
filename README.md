# RNA-seq Differential Expression in Colorectal Cancer

Paired differential-expression and pathway analysis of colorectal tumour versus adjacent
normal tissue.

## Overview

Using RNA-seq counts from the GSE50760 colorectal-cancer study, this project identifies
genes that change expression between primary tumour and matched normal colon, then asks
which biological processes are over-represented among the up-regulated genes. A paired
design accounts for the fact that the two samples in each comparison come from the same
patient.

## Data

Derived from **GEO accession GSE50760** (colorectal cancer; paired normal, primary tumour,
and liver metastasis per patient). A random 10-patient subsample (fixed seed) is used,
restricted to the normal versus primary-tumour comparison (20 samples).

> **Note on the data file:** the analysis expects a `SummarizedExperiment` `.Rda` object.
> Because this file can exceed GitHub's size limits, it is **not committed** to this
> repository — the raw data is publicly available from GEO under accession GSE50760.

## Methods

- Quality control on log-CPM distributions and PCA on the most variable genes.
- Gene filtering with `filterByExpr` and TMM normalisation (edgeR).
- Differential expression with **limma-voom** using a patient-blocked paired design
  (`~ patient_id + condition`).
- Over-representation analysis (hypergeometric test) of the up-regulated genes against GO
  Biological Process terms, with Benjamini–Hochberg correction.

## Key results

Identified **1,010 differentially expressed genes** (FDR < 0.05, |log2FC| > 2): 280
up-regulated and 730 down-regulated in tumour. Over-representation analysis of the
up-regulated set returned 127 enriched GO-BP terms, dominated by inflammation and
chemotaxis, extracellular-matrix remodelling, and epithelial–mesenchymal transition —
consistent with known colorectal-cancer biology.

## Repository contents

- `analysis.Rmd` — full analysis source
- `report.pdf` — rendered report
- `README.md`
- (data not committed — see the Data note; obtain from GEO GSE50760)

## Running the analysis

Requires R with `SummarizedExperiment`, `edgeR`, `limma`, `org.Hs.eg.db`, `GO.db`,
`AnnotationDbi`, `ggplot2`, and `ggrepel`. Place the `.Rda` object in the working directory
and knit `analysis.Rmd`.

## Notes

Developed during my MSc in Bioinformatics and Biostatistics. Code comments are in Spanish.
The enrichment step uses a manual hypergeometric test, equivalent to the over-representation
analysis in clusterProfiler.
