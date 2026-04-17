<div align='center' >
<p><font size='70'><strong>FreeFlow-ONT User Manual</strong></font></p>
<font size='100'>(version 1.0)</font>
</div>

- FreeFlow-ONT is a user-friendly and modular platform designed for reference genome-free m^6^A analysis of Oxford Nanopore Technologies direct RNA sequencing (ONT DRS) data. It aims to support streamlined, end-to-end analysis under conditions where high-quality reference genomes or annotations are unavailable. By integrating long-read transcriptome assembly, m^6^A modification identification, differential modification analysis, and machine learning-based prediction into a unified Galaxy environment, FreeFlow-ONT enables reproducible and flexible analysis of plant m^6^A data from raw signals to biologically interpretable results. FreeFlow-ONT comprises eight functional modules:  **Data Preparation, Quality Control, Transcriptome Construction, Transcriptome Evaluation, RNA Modification Identification, Functional Annotation, Differential Modification Analysis, and Machine Learning-based Prediction**.
- FreeFlow-ONT was powered with an advanced  packaging technology, which enables compatibility and portability.
- FreeFlow-ONT project is hosted on https://github.com/jy-ai/FreeFlow-ONT
- FreeFlow-ONT docker image is available at https://hub.docker.com/r/malab/freeflowont

## RNA Modification Identification Module

This module is designed to identify RNA modification sites from ONT direct RNA sequencing data. It includes three major steps: **alignment of reads to the reference transcriptome**, **generation of eventalign tables**, and **m6Anet-based feature preparation and inference**. In the current implementation, reads are first aligned to the reference transcriptome using **minimap2**, then processed by **f5c** or **Nanopolish** to generate eventalign results, and finally analyzed by **m6Anet** for downstream modification inference.

| **Tools**                                  | **Description**                                                                | **Input**                                                                 | **Output**                          | **Time (test data)**                         | **Reference** |
| ------------------------------------------------ | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------- | ----------------------------------------- | -------------------------------------------------- | ------------------- |
| **Step1: Minimap2 align to transcriptome** | Align reads to reference transcriptome with minimap2                                 | Reads FASTQ, reference transcriptome FASTA                                      | aligned.bam; aligned.bam.bai              | Depends on the dataset size                        | minimap2            |
| **Step2: Eventalign (f5c / Nanopolish)**   | Generate eventalign table for m6Anet using f5c or Nanopolish                         | Aligned BAM, reads FASTQ, reference transcriptome FASTA, FAST5 directory tar.gz | eventalign.tsv.gz; eventalign.summary.txt | Depends on the dataset size and parameter settings | f5c / Nanopolish    |
| **Step3: m6Anet features**                 | Run m6Anet dataprep on eventalign data and perform inference with a selectable model | Eventalign TSV (gz)                                                             | m6anet_outputs.tar.gz                     | Depends on the dataset size and parameter settings | m6Anet              |

## Step1: Minimap2 Align to Transcriptome

This function is designed to align sequencing reads to a reference transcriptome using **minimap2**. The alignment results are converted to BAM format, sorted, and indexed automatically for downstream analysis.

#### Input

- **Reads FASTQ:** A FASTQ file containing sequencing reads.
- **Reference transcriptome FASTA:** A FASTA file containing reference transcript sequences.

#### Parameters

- **Threads:** An integer specifying the number of CPU threads used during alignment. The default value is **20**.

#### Output

- **aligned.bam:** A sorted BAM file containing the alignment results.
- **aligned.bam.bai:** An index file for the aligned BAM file.

![iShot_2026-04-09_22.31.14](./img/iShot_2026-04-09_22.31.14.png)

## Step2: Eventalign (f5c / Nanopolish)

This function is designed to generate an eventalign table for downstream **m6Anet** analysis using either **f5c** or **Nanopolish**. It takes transcriptome alignment results, sequencing reads, the reference transcriptome, and a compressed FAST5 directory as input. The resulting eventalign table is compressed in gzip format for downstream processing.

#### Input

- **Aligned BAM (to transcriptome):** A BAM file containing reads aligned to the reference transcriptome.
- **Reads FASTQ (same as alignment):** A FASTQ file containing the same reads used for alignment.
- **Reference transcriptome FASTA:** A FASTA file containing reference transcript sequences.
- **FAST5 directory (tar.gz):** A compressed archive containing the FAST5 files.

#### Parameters

- **Eventalign tool:** Select either **f5c** or **Nanopolish** for eventalign generation. The default value is **f5c**.
- **Threads:** An integer specifying the number of CPU threads used during processing. The default value is **20**.

#### Output

- **eventalign.tsv.gz:** A gzipped eventalign table for downstream m6Anet analysis.
- **eventalign.summary.txt:** A summary file generated by Nanopolish. When **f5c** is selected, an empty placeholder file is produced.

![iShot_2026-04-09_22.31.59](./img/iShot_2026-04-09_22.31.59.png)

## Step3: m6Anet Features

This function is designed to run **m6Anet dataprep** on the eventalign table and then perform **m6Anet inference** using either a pretrained model or a user-supplied custom model. In the current implementation, this step includes both feature preparation and inference, and the dataprep and inference outputs are packaged together into a compressed archive.

#### Input

- **Eventalign TSV (gz):** A gzipped eventalign file generated from the previous step.

#### Parameters

- **Processes (n_processes):** An integer specifying the number of processes used by m6Anet. The default value is **20**.
- **Inference batch size:** An integer specifying the batch size used during inference. The default value is **64**.
- **Model selection:** Select either a **Pretrained model** or a **Custom model**. The default setting is **Pretrained model**.
- **Pretrained model name:** When the pretrained option is selected, available models include **Hct116_RNA002**, **arabidopsis**, and **HEK293T_RNA004**. The default value is **Hct116_RNA002**.
- **Model config (toml):** A user-supplied model configuration file for custom inference.
- **Model weights (state_dict):** A user-supplied model weight file for custom inference.

#### Output

- **m6anet_outputs.tar.gz:** A compressed archive containing both the **dataprep** and **inference** result directories generated by m6Anet.

![iShot_2026-04-09_22.33.50](./img/iShot_2026-04-09_22.33.50.png)
