<p align="center">
  <img src="https://img.shields.io/badge/RNA--seq-portfolio-7C3AED?style=for-the-badge&logo=databricks&logoColor=white" alt="RNA-seq portfolio badge" />
  <img src="https://img.shields.io/badge/workflow-Snakemake-0F766E?style=for-the-badge&logo=snakemake&logoColor=white" alt="Snakemake badge" />
  <img src="https://img.shields.io/badge/environment-Conda-16A34A?style=for-the-badge&logo=anaconda&logoColor=white" alt="Conda badge" />
  <img src="https://img.shields.io/badge/design-temperature%20time--series-2563EB?style=for-the-badge" alt="time-series badge" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/focus-reproducibility-0891B2?style=flat-square" alt="reproducibility badge" />
  <img src="https://img.shields.io/badge/focus-deliverables-F59E0B?style=flat-square" alt="deliverables badge" />
  <img src="https://img.shields.io/badge/focus-DEG%20%26%20enrichment-DC2626?style=flat-square" alt="DEG and enrichment badge" />
  <img src="https://img.shields.io/badge/status-in%20progress-6B7280?style=flat-square" alt="status badge" />
</p>

# rnaseq-portfolio

A reproducible, deliverable-style bulk RNA-seq analysis project built with **Snakemake + Conda** for a **temperature time-series experiment**.

> [!IMPORTANT]
> This repository is designed as both:
> - a **portfolio-style project showcase** for RNA-seq analysis
> - a **reproducible workflow** that can be rerun from metadata and configuration files

---

## Highlights

- End-to-end bulk RNA-seq workflow with reproducible execution
- Deliverable-oriented output structure for clear result navigation
- Designed for a temperature time-series experiment
- Includes explicit contrast definitions for downstream differential expression analysis
- Built to be understandable for both technical reviewers and non-specialist readers

---

## Study design

This project analyzes bulk RNA-seq samples from a temperature time-series design:

- Low-temperature induction: **L1–L10**
- High-temperature control: **H1–H10**

Planned contrasts:

| Contrast ID | Test group | Control group | Rationale |
|---|---|---|---|
| `L123_vs_H123` | `L1, L2, L3` | `H1, H2, H3` | early-stage comparison |
| `L34_vs_H34` | `L3, L4` | `H3, H4` | transition-stage comparison |
| `L56_vs_H56` | `L5, L6` | `H5, H6` | mid-stage comparison |
| `L7to10_vs_H7to10` | `L7, L8, L9, L10` | `H7, H8, H9, H10` | late-stage comparison |

> [!NOTE]
> `L3` is intentionally included in two contrasts because its morphology is ambiguous and needs to be evaluated under both comparison schemes.

---

## Workflow overview

The analysis is organized into the following major steps:

1. **Sample metadata preparation**  
   Define sample information, FASTQ paths, and contrast groups.

2. **Quality control**  
   Assess raw read quality and summarize QC metrics across samples.

3. **Read processing / quantification**  
   Perform transcript quantification or count generation depending on workflow configuration.

4. **Expression profiling**  
   Generate count/expression matrices and sample-level exploratory analyses such as PCA and correlation.

5. **Differential expression analysis**  
   Run contrast-based comparisons and summarize significantly changed genes/transcripts.

6. **Functional enrichment analysis**  
   Interpret differential expression results using GO / KEGG / related enrichment approaches.

7. **Deliverable generation**  
   Organize outputs into a report-oriented folder structure for easy review.

### Tool stack

<p>
  <img src="https://img.shields.io/badge/QC-FastQC-4C1D95?style=flat-square" alt="FastQC badge" />
  <img src="https://img.shields.io/badge/Summary-MultiQC-7C2D12?style=flat-square" alt="MultiQC badge" />
  <img src="https://img.shields.io/badge/Quantification-Salmon-E11D48?style=flat-square" alt="Salmon badge" />
  <img src="https://img.shields.io/badge/Differential%20Expression-DESeq2-B91C1C?style=flat-square" alt="DESeq2 badge" />
</p>

---

## Quickstart

### 1. Create environment

```bash
cd /home/zmq/projects/rnaseq-portfolio
conda env create -f env/environment.yml
conda activate rnaseq-portfolio
```

### 2. Run mini smoke test

This checks whether the workflow can run end-to-end on a small test dataset before using real data.

```bash
snakemake --profile profiles/local --configfile tests/mini/config/config.yaml
```

### 3. Run on real data

Before running, prepare:

- `resources/samples.csv`
- `resources/contrasts.csv`
- `config/config.yaml`
- `resources/references.md`

Then run:

```bash
snakemake --profile profiles/local
```

> [!TIP]
> For reviewers or HR readers, the most important end-point is usually the final report under `deliverables/report/`.

---

## Inputs

### Sample sheet

Edit `resources/samples.csv`:

```csv
sample,group,timepoint,replicate,fastq_1,fastq_2
L1,L,1,1,/path/to/L1_R1.fastq.gz,/path/to/L1_R2.fastq.gz
L2,L,2,1,/path/to/L2_R1.fastq.gz,/path/to/L2_R2.fastq.gz
L3,L,3,1,/path/to/L3_R1.fastq.gz,/path/to/L3_R2.fastq.gz
L4,L,4,1,/path/to/L4_R1.fastq.gz,/path/to/L4_R2.fastq.gz
L5,L,5,1,/path/to/L5_R1.fastq.gz,/path/to/L5_R2.fastq.gz
L6,L,6,1,/path/to/L6_R1.fastq.gz,/path/to/L6_R2.fastq.gz
L7,L,7,1,/path/to/L7_R1.fastq.gz,/path/to/L7_R2.fastq.gz
L8,L,8,1,/path/to/L8_R1.fastq.gz,/path/to/L8_R2.fastq.gz
L9,L,9,1,/path/to/L9_R1.fastq.gz,/path/to/L9_R2.fastq.gz
L10,L,10,1,/path/to/L10_R1.fastq.gz,/path/to/L10_R2.fastq.gz
H1,H,1,1,/path/to/H1_R1.fastq.gz,/path/to/H1_R2.fastq.gz
H2,H,2,1,/path/to/H2_R1.fastq.gz,/path/to/H2_R2.fastq.gz
H3,H,3,1,/path/to/H3_R1.fastq.gz,/path/to/H3_R2.fastq.gz
H4,H,4,1,/path/to/H4_R1.fastq.gz,/path/to/H4_R2.fastq.gz
H5,H,5,1,/path/to/H5_R1.fastq.gz,/path/to/H5_R2.fastq.gz
H6,H,6,1,/path/to/H6_R1.fastq.gz,/path/to/H6_R2.fastq.gz
H7,H,7,1,/path/to/H7_R1.fastq.gz,/path/to/H7_R2.fastq.gz
H8,H,8,1,/path/to/H8_R1.fastq.gz,/path/to/H8_R2.fastq.gz
H9,H,9,1,/path/to/H9_R1.fastq.gz,/path/to/H9_R2.fastq.gz
H10,H,10,1,/path/to/H10_R1.fastq.gz,/path/to/H10_R2.fastq.gz
```

### Contrast sheet

Edit `resources/contrasts.csv`:

```csv
contrast_id,test_samples,control_samples
L123_vs_H123,"L1;L2;L3","H1;H2;H3"
L34_vs_H34,"L3;L4","H3;H4"
L56_vs_H56,"L5;L6","H5;H6"
L7to10_vs_H7to10,"L7;L8;L9;L10","H7;H8;H9;H10"
```

---

## Outputs

> [!TIP]
> After a successful run, the main entry point is:
>
> `deliverables/report/report.html`

### Key output files

#### Quality control

- `deliverables/01.Quality_Statistics/multiqc_report.html`
- `deliverables/01.Quality_Statistics/Total_RNA_Quality_Summary.tsv`
- `deliverables/01.Quality_Statistics/Total_RNA_Quality_Images/`

#### Mapping / quantification

Depending on the configured route:

- `deliverables/02.Mapping_Evaluation/`
- `deliverables/03.mRNA_Expression_Profile/Raw_Reads_Count/`
- `deliverables/03.mRNA_Expression_Profile/FPKM/`
- `deliverables/03.mRNA_Expression_Profile/correlation.pdf`
- `deliverables/03.mRNA_Expression_Profile/pca.pdf`

#### Differential expression

- `deliverables/04.Differential_Expression_Analysis/<CONTRAST>_Gene_Images/`
- `deliverables/04.Differential_Expression_Analysis/<CONTRAST>_Transcript_Images/`
- `deliverables/04.Differential_Expression_Analysis/mRNA_Gene_Differential_Expression_Summary.tsv`
- `deliverables/04.Differential_Expression_Analysis/mRNA_Transcript_Differential_Expression_Summary.tsv`

#### Enrichment analysis

- `deliverables/05.Gene_Set_Enrichment_Analysis/<CONTRAST>/`
- `deliverables/05.Gene_Set_Enrichment_Analysis/*GO*_Summary.tsv`
- `deliverables/05.Gene_Set_Enrichment_Analysis/*KEGG*_Summary.tsv`

#### Reproducibility / audit trail

- `deliverables/software/software_versions.txt`
- `results/logs/`

---

## Deliverables layout

The `deliverables/` directory is intentionally organized like an external analysis delivery package:

| Directory | Description |
|---|---|
| `01.Quality_Statistics/` | Read-level QC summary tables and plots |
| `02.Mapping_Evaluation/` | Mapping QC results, if alignment-based route is enabled |
| `03.mRNA_Expression_Profile/` | Expression matrices, PCA, and sample correlation plots |
| `04.Differential_Expression_Analysis/` | DEG tables and per-contrast figures |
| `05.Gene_Set_Enrichment_Analysis/` | GO / KEGG / related enrichment summaries |
| `report/` | Human-readable final report |
| `software/` | Software version snapshot for auditability |

---

## Repository structure

| Path | Description |
|---|---|
| `workflow/` | Snakemake workflow and modular rules |
| `scripts/` | R / Python helper scripts |
| `config/` | Pipeline configuration files |
| `resources/` | Metadata tables and reference notes |
| `env/` | Conda environment definitions |
| `tests/mini/` | Mini test dataset and smoke-test configuration |
| `deliverables/` | Final result package for external review |

---

## Methods at a glance

- Workflow engine: **Snakemake**
- Environment management: **Conda**
- QC aggregation: **MultiQC**
- Quantification: configurable (for example **Salmon**)
- Differential expression: **DESeq2** on count-based data
- Enrichment analysis: configurable downstream interpretation modules

For more detail, see:

- `docs/methods.md`
- `config/config.yaml`
- `resources/references.md`

---

## Why this repository is structured this way

This project is not only intended to run an RNA-seq workflow, but also to demonstrate:

- reproducible analysis design
- interpretable folder organization
- contrast-aware downstream analysis
- deliverable-style presentation for external review

The repository therefore separates **workflow logic**, **metadata/configuration**, and **final deliverables**.

---

## Current project status

This repository is under active development.

<p>
  <img src="https://img.shields.io/badge/current-focus-contrast%20design-2563EB?style=flat-square" alt="contrast design badge" />
  <img src="https://img.shields.io/badge/current-focus-metadata%20standardization-0F766E?style=flat-square" alt="metadata badge" />
  <img src="https://img.shields.io/badge/current-focus-report%20polishing-F59E0B?style=flat-square" alt="report polishing badge" />
</p>

Current focus:

- finalizing sample metadata
- standardizing contrast definitions
- improving result organization
- polishing report-style outputs for portfolio presentation