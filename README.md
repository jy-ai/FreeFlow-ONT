## FreeFlow-ONT: a Galaxy-based, end-to-end platform for reference genome-free m<sup>6</sup>A analysis from ONT direct RNA sequencing

<a href="https://hub.docker.com/r/malab/mlpea" target="_blank"><img src="https://img.shields.io/badge/Docker_image-ready-red.svg" target="_blank"></a><a href="https://hub.docker.com/r/malab/mlpea" target="_blank"><img src="https://img.shields.io/docker/pulls/malab/mlpea"></a><a href="https://github.com/cma2015/mlPEA" target="_blank"><img src="https://img.shields.io/badge/Source%20codes-support-blue"></a>

## Introduction
- FreeFlow-ONT is a user-friendly and modular platform designed for reference genome-free m<sup>6</sup>A analysis of ONT direct RNA sequencing (DRS) data. It aims to support streamlined, end-to-end analysis under conditions where high-quality reference genomes or annotations are unavailable. By integrating long-read transcriptome assembly, m<sup>6</sup>A modification identification, differential modification analysis, and machine learning-based prediction into a unified Galaxy environment, FreeFlow-ONT enables reproducible and flexible analysis of plant m<sup>6</sup>A data from raw signals to biologically interpretable results. FreeFlow-ONT comprises eight functional modules: **Data Preparation, Quality Control, Transcriptome Construction, Transcriptome Evaluation, RNA Modification Identification, Functional Annotation, Differential Modification Analysis, and Machine Learning-based Prediction**.
- FreeFlow-ONT project is hosted on https://github.com/jy-ai/FreeFlow-ONT.
- FreeFlow-ONT Docker image can be obtained from https://hub.docker.com/r/malab/freeflowont.

## How to use FreeFlow-ONT
- Test data and tutorial of FreeFlow-ONT are presented at [https://github.com/jy-ai/FreeFlow-ONT/tree/main/User_Manual](https://github.com/jy-ai/FreeFlow-ONT/blob/main/User_Manual/)
- [The installation of FreeFlow-ONT](https://github.com/jy-ai/FreeFlow-ONT/blob/main/User_Manual/00_Installation.md)
- [The Data Preparation and Quality Control Modules](https://github.com/jy-ai/FreeFlow-ONT/blob/main/User_Manual/01_Pre-analysis.md)
- [The Transcriptome Construction Module](https://github.com/jy-ai/FreeFlow-ONT/blob/main/User_Manual//02_Transcriptome_Construction.md)
- [The Transcriptome Evaluation Module](https://github.com/jy-ai/FreeFlow-ONT/blob/main/User_Manual/03_Transcriptome_Evaluation.md)
- [The RNA Modification Identification Module](https://github.com/jy-ai/FreeFlow-ONT/blob/main/User_Manual/04_RNA_Modification_Identification.md)
- [The Functional Annotation Module](https://github.com/jy-ai/FreeFlow-ONT/blob/main/User_Manual/05_Functional_Annotation.md)
- [The Differential Modification Module](https://github.com/jy-ai/FreeFlow-ONT/blob/main/User_Manual/06_Differential_Modification.md)
- [The Machine Learning-based Prediction Module](https://github.com/jy-ai/FreeFlow-ONT/blob/main/User_Manual/07_Machine_Learning-based_Prediction.md)

## How to access help
* Comments/suggestions/bugs/issues are welcome to be reported [here](https://github.com/jy-ai/FreeFlow-ONT/issues) or contact: Jing Yang sxxayj@nwafu.edu.cn, Minggui Song smg@nwafu.edu.cn or Chuang Ma chuangma2006@gmail.com

## Quick start

- **Step 1**: FreeFlow-ONT installation from Docker Hub

```
# pull latest FreeFlow-ONT Docker image from docker hub
$ docker pull malab/freeflowont
```

- **Step 2**: Launch FreeFlow-ONT local server

```
$ docker run -it -p 880:8080 malab/freeflowont bash
$ bash /home/galaxy/run.sh
```

Then, FreeFlow-ONT local server can be accessed via [http://localhost:8080](http://localhost:8080/)

- **Step 3**: Upload ONT DRS data


- **Step 4**: Construct assembled transcriptome

- **Step 5**: Call m<sup>6</sup>A site

- **Step 6**: Functional Exploration


## Change log
- 2026.03 Release FreeFlow-ONT v1.0
- 2026.01 FreeFlow-ONT web server online
- 2025.06 We launched FreeFlow-ONT project
