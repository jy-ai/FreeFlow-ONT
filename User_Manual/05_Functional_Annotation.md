<div align='center' >
<p><font size='70'><strong>FreeFlow-ONT User Manual</strong></font></p>
<font size='100'>(version 1.0)</font>
</div>

- FreeFlow-ONT is a user-friendly and modular platform designed for reference genome-free m^6^A analysis of Oxford Nanopore Technologies direct RNA sequencing (ONT DRS) data. It aims to support streamlined, end-to-end analysis under conditions where high-quality reference genomes or annotations are unavailable. By integrating long-read transcriptome assembly, m^6^A modification identification, differential modification analysis, and machine learning-based prediction into a unified Galaxy environment, FreeFlow-ONT enables reproducible and flexible analysis of plant m^6^A data from raw signals to biologically interpretable results. FreeFlow-ONT comprises eight functional modules:  **Data Preparation, Quality Control, Transcriptome Construction, Transcriptome Evaluation, RNA Modification Identification, Functional Annotation, Differential Modification Analysis, and Machine Learning-based Prediction**.
- FreeFlow-ONT was powered with an advanced  packaging technology, which enables compatibility and portability.
- FreeFlow-ONT project is hosted on https://github.com/jy-ai/FreeFlow-ONT
- FreeFlow-ONT docker image is available at https://hub.docker.com/r/malab/freeflowont

## Functional Annotation Module

This module is designed to provide functional interpretation for assembled transcripts. It includes two steps: **coding potential and ORF annotation** and **functional annotation**. In the first step, **TranslationAI** is used to predict translation initiation and termination related ORF information from transcript sequences. In the second step, predicted transcript-derived protein sequences are functionally annotated using **eggNOG-mapper** based on homology.

| **Tools**                                    | **Description**                                                  | **Input**                                       | **Output**                 | **Time (test data)**  | **Reference** |
| -------------------------------------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------- | -------------------------------- | --------------------------- | ------------------- |
| **Step1: Coding Potential & ORF Annotation** | Perform TranslationAI-based ORF annotation for assembled transcripts   | Assembled transcript FASTA                            | TranslationAI prediction results | Depends on the dataset size | TranslationAI       |
| **Step2: Functional Annotation**             | Perform homology-based functional annotation for assembled transcripts | Assembled transcript FASTA, transcript annotation TXT | Eggnog annotation results        | Depends on the dataset size | eggNOG-mapper       |

## Step1: Coding Potential & ORF Annotation

This function is designed to predict coding potential and ORF-related features from assembled transcript sequences using **TranslationAI**. It analyzes transcript sequences to identify translation initiation sites and termination sites, and generates annotation results in TXT format.

#### Input

- **Assembled transcript file:** A FASTA file containing assembled transcript sequences.

#### Parameters

- **Threshold k values for TIS prediction:** A float specifying the threshold for translation initiation site prediction. If the value is greater than 1, the top-k TISs in each sequence are reported; otherwise, sites with scores greater than or equal to the threshold are reported. The default value is **0.5**.
- **Threshold k values for TSS prediction:** A float specifying the threshold for translation termination site prediction. If the value is greater than 1, the top-k TSSs in each sequence are reported; otherwise, sites with scores greater than or equal to the threshold are reported. The default value is **0.5**.
- **Threads:** An integer specifying the number of CPU threads available for the analysis. The default value is **1**.

#### Output

- **TranslationAI Prediction Results:** A TXT file containing predicted ORF-related annotation results.

![iShot_2026-04-09_22.37.37](./img/iShot_2026-04-09_22.37.37.png)

## Step2: Functional Annotation

This function is designed to perform functional annotation for assembled transcripts using **eggNOG-mapper**. It takes assembled transcript sequences together with a transcript annotation file as input, translates transcripts into peptide sequences through the internal workflow, and identifies potential functions based on homology.

#### Input

- **Assembled transcript file:** A FASTA file containing assembled transcript sequences.
- **Transcript annotation file:** A TXT file containing transcript annotation results, which can be used as input for downstream functional annotation.

#### Parameters

- **Threads:** An integer specifying the number of CPU threads used during eggNOG-mapper annotation. The default value is **1**.

#### Output

- **Eggnog Annotation Results:** A TXT file containing functional annotation results generated by eggNOG-mapper.

![iShot_2026-04-09_22.37.45](./img/iShot_2026-04-09_22.37.45.png)

## Notes

- The first step focuses on **coding potential and ORF-related prediction**, while the second step provides **homology-based functional annotation** for the resulting transcript-derived protein sequences.
- The **Transcript annotation file** used in Step2 is intended to work with the assembled transcript FASTA and can serve as the annotation input for the functional annotation workflow.
