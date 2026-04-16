<div align='center' >
<p><font size='70'><strong>FreeFlow-ONT User Manual</strong></font></p>
<font size='100'>(version 1.0)</font>
</div>


- FreeFlow-ONT is a Galaxy-based framework for the analysis of Oxford Nanopore Technologies direct RNA sequencing (ONT DRS) data. It provides an integrated and user-friendly environment that supports key steps of ONT DRS analysis, including data preprocessing, alignment, transcript-level analysis, and downstream functional exploration. The framework is designed to improve accessibility, standardization, and efficiency in ONT DRS data analysis for both routine and advanced research applications.
- FreeFlow-ONT was powered with an advanced  packaging technology, which enables compatibility and portability.
- FreeFlow-ONT project is hosted on https://github.com/jy-ai/FreeFlow-ONT
- FreeFlow-ONT docker image is available at https://hub.docker.com/r/malab/freeflowont

## Machine Learning-based Prediction Module

This module is designed to support machine learning-based analysis from transcriptome assemblies and ONT direct RNA sequencing data. It currently includes two tools: **Consensus Transcriptome Generation** and **Prediction System Construction**. The first tool generates a consensus transcriptome by integrating long-read and short-read assemblies, evaluating transcript support from ONT and short-read evidence, filtering low-confidence candidates, and selecting representative transcript sequences. The second tool constructs a reference-free training dataset for RNA modification prediction by combining m6Anet-supported sites, peak regions, alignment-derived features, and signal features, and then trains CatBoost and baseline models for downstream prediction tasks.

| **Tools** | **Description** | **Input** | **Output** | **Time (test data)** | **Reference** |
|-------------------------------|------------------------------------------------------------|-----------------------------------------------|-------------------------------------------------|----------------------------|------------------------------------------------------------|
| **Consensus Transcriptome Generation** | Generate a consensus transcriptome from long-read and short-read assemblies using support filtering and representative sequence selection | Long-read contigs FASTA, short-read contigs FASTA, corrected long reads FASTA, paired short reads FASTQ | final.rep.fa and supporting evidence tables | Depends on the dataset size and parameter settings | Custom hybrid consensus pipeline |
| **Prediction System Construction** | Build a reference-free training dataset from m6Anet-supported sites and ONT-derived features, then train CatBoost and baseline models | Transcript FASTA, m6Anet site probability CSV, peak BED, reads FASTQ, eventalign shards | CatBoost model, feature matrix, labels, metrics summary, and CV summary | Depends on the dataset size and parameter settings | Custom machine learning pipeline |

## Consensus Transcriptome Generation

This function is designed to generate a consensus transcriptome from long-read and short-read transcript assemblies. It first merges long-read and short-read contigs into a unified candidate FASTA file, then evaluates transcript support using both ONT long reads and short-read data, filters low-confidence candidates, clusters redundant sequences, calculates evidence scores, and finally selects representative transcript sequences to produce a consensus transcriptome.

#### Input

- **Long-read contigs FASTA:** A FASTA file containing transcript sequences assembled from long-read data.
- **Short-read contigs FASTA:** A FASTA file containing transcript sequences assembled from short-read data.
- **Corrected long reads FASTA:** A FASTA file containing corrected long reads used to evaluate ONT support for candidate transcripts.
- **Short-read R1 FASTQ / Short-read R2 FASTQ:** Paired-end short-read FASTQ files used for candidate support evaluation and expression quantification.

#### Parameters

- **Threads:** An integer specifying the number of CPU threads used during the workflow. The default value is **24**.
- **Short-read minimum MAPQ:** Minimum mapping quality used when summarizing short-read coverage. The default value is **10**.
- **Salmon library type:** Library type used for Salmon quantification. The default value is **A**.
- **Long-read prefix / Short-read prefix:** Prefixes added to long-read and short-read contig IDs when building the candidate FASTA file. The default values are **LR_** and **SR_**.
- **Long-read backbone filtering parameters:** Thresholds used to retain candidate transcripts based on ONT support and short-read support.
- **Short-read support parameters:** Thresholds used to retain short-read-supported candidates and optionally require ONT support.
- **MMseqs minimum identity / coverage / coverage mode:** Parameters controlling sequence clustering before representative selection.
- **Evidence score weights:** Weights assigned to ONT read count, ONT breadth, ONT identity, short-read breadth, short-read depth, and TPM during representative scoring.
- **Length tie-break rule:** Rule used to break ties when multiple sequences receive similar support. The default value is **longer**.

#### Output

- **candidate.fa:** Combined candidate transcript FASTA file containing long-read and short-read contigs.
- **candidate.ont_support.tsv:** A tabular file summarizing ONT support for each candidate transcript.
- **candidate.sr_cov.tsv:** A tabular file summarizing short-read coverage across candidate transcripts.
- **candidate.quant.sf:** A Salmon quantification table for candidate transcripts.
- **final.keep.ids:** A text file containing retained candidate transcript IDs after evidence-based filtering.
- **final.keep.fa:** A FASTA file containing retained candidate transcripts.
- **mmseqs_clusters.tsv:** A tabular file containing MMseqs clustering results for retained candidates.
- **candidate.evidence_score.tsv:** A tabular file containing evidence scores used for representative selection.
- **final.rep.ids:** A text file containing the selected representative transcript IDs.
- **mmseqs_clusters.picked.tsv:** A tabular file recording cluster membership and representative selection.
- **final.rep.fa:** The final consensus transcriptome FASTA file containing representative transcript sequences.
- **consensus_workdir.tar.gz:** A compressed archive containing the full working directory and intermediate files.

#### Notes

- The candidate FASTA file is generated by concatenating long-read and short-read contigs after adding source-specific prefixes to transcript IDs.
- ONT support is evaluated from long-read alignments to the candidate set, while short-read support is evaluated from paired-end read mapping and Salmon quantification.
- Candidate filtering combines structural support from long reads with additional evidence from short reads and expression.
- Representative transcripts are selected after MMseqs clustering using a weighted evidence score and optional long-read preference.

![iShot_2026-04-16_22.34.13](./img/iShot_2026-04-16_22.34.13.png)

## Prediction System Construction

This function is designed to construct a reference-free training dataset for RNA modification prediction and to train machine-learning models from ONT-derived features. It first prepares candidate modification sites from transcript sequences, m6Anet predictions, and peak regions, then filters and subsamples negative sites, extracts sequence, alignment/base-quality, and signal features, builds a training matrix, and finally trains CatBoost and baseline classifiers with additional group-aware cross-validation.

#### Input

- **Transcript FASTA:** A FASTA file containing transcript sequences used for candidate-site construction and sequence-based feature extraction.
- **m6Anet site probability CSV:** A CSV file containing m6Anet site-level prediction results used to define high-confidence modification-supported sites.
- **Peak BED:** A BED file containing peak regions used together with m6Anet-supported sites to define positive training examples.
- **Reads FASTQ:** A FASTQ file containing sequencing reads used for BAM generation and downstream feature extraction.
- **Eventalign shard archive:** A tar.gz archive containing eventalign shard files used for signal feature extraction.

#### Parameters

- **Minimum reads for m6Anet sites:** Minimum read support required for retaining m6Anet-supported sites. The default value is **20**.
- **Minimum probability for m6Anet sites:** Minimum probability threshold for retaining m6Anet-supported sites. The default value is **0.5**.
- **m6Anet position base:** Position coordinate convention used for m6Anet sites. The default value is **0**.
- **Exclude tolerance around m6Anet sites:** Exclusion radius used when generating negative sites around m6Anet-supported positions. The default value is **4 bp**.
- **Minimum depth for negative sites:** Minimum total read depth required for retaining negative sites. The default value is **20**.
- **Negative sampling ratio:** Maximum negative-to-positive ratio retained for model training. The default value is **5**.
- **Number of buckets:** Number of hash buckets used for sharding candidate sites and eventalign files. The default value is **128**.
- **Threads:** Number of CPU threads used during processing. The default value is **24**.
- **Maximum open files:** Maximum number of simultaneously open files during eventalign sharding. The default value is **256**.
- **Eventalign position base:** Position coordinate convention used for eventalign-based signal extraction. The default value is **0**.
- **Skip completed steps if outputs already exist:** Reuse previously generated intermediate files when available.

#### Output

- **pos_sites.bed:** A BED file containing positive training sites supported by both m6Anet and peak regions.
- **neg_final.bed:** A BED file containing filtered and optionally subsampled negative training sites.
- **X.npy:** A NumPy matrix containing the extracted feature matrix for model training.
- **y.npy:** A NumPy file containing binary class labels for the training dataset.
- **meta.tsv:** A metadata table describing the sites or groups included in the training matrix.
- **catboost.cbm:** The trained CatBoost model file.
- **metrics.json:** A JSON file containing the main CatBoost training metrics.
- **metrics_summary.default_thr.tsv:** A tabular summary of model evaluation metrics at the default threshold.
- **cv5.summary.tsv:** A tabular summary of 5-fold group-aware cross-validation results for multiple models.
- **prediction_workdir.tar.gz:** A compressed archive containing the full working directory and intermediate files.

![iShot_2026-04-16_22.34.25](./img/iShot_2026-04-16_22.34.25.png)

#### Notes

- Positive sites are defined as the overlap between m6Anet-supported sites and peak regions.
- Negative candidate sites are derived from RRACH motifs after excluding peak-associated positions and nearby m6Anet-supported sites.
- The workflow applies depth filtering to negative sites and can optionally subsample negatives to control class imbalance.
- Feature extraction includes sequence features, alignment/base-quality features, and signal features derived from eventalign results.
- In addition to CatBoost, the workflow also evaluates multiple baseline models and performs 5-fold group-aware cross-validation to summarize model robustness.
