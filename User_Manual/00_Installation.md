<div align='center' >
<p><font size='70'><strong>FreeFlow-ONT User Manual</strong></font></p>
<font size='100'>(version 1.0)</font>
</div>

- FreeFlow-ONT is a user-friendly and modular platform designed for reference genome-free m^6^A analysis of Oxford Nanopore Technologies direct RNA sequencing (ONT DRS) data. It aims to support streamlined, end-to-end analysis under conditions where high-quality reference genomes or annotations are unavailable. By integrating long-read transcriptome assembly, m^6^A modification identification, differential modification analysis, and machine learning-based prediction into a unified Galaxy environment, FreeFlow-ONT enables reproducible and flexible analysis of plant m^6^A data from raw signals to biologically interpretable results. FreeFlow-ONT comprises eight functional modules:  **Data Preparation, Quality Control, Transcriptome Construction, Transcriptome Evaluation, RNA Modification Identification, Functional Annotation, Differential Modification Analysis, and Machine Learning-based Prediction**.
- FreeFlow-ONT was powered with an advanced  packaging technology, which enables compatibility and portability.
- FreeFlow-ONT project is hosted on https://github.com/jy-ai/FreeFlow-ONT
- FreeFlow-ONT docker image is available at https://hub.docker.com/r/malab/freeflowont

## FreeFlow-ONT installation

- **Step 1**: Docker installation

  **i) Docker installation and start ([Official installation tutorial](https://docs.docker.com/install))**

  For **Windows (Only available for Windows 10 Prefessional and Enterprise version):**

  - Download [Docker](https://download.docker.com/win/stable/Docker for Windows Installer.exe) for windows;
  - Double click the EXE file to open it;
  - Follow the wizard instruction and complete installation;
  - Search docker, select **Docker for Windows** in the search results and click it.

  For **Mac OS X (Test on macOS Sierra version 10.12.6 and macOS High Sierra version 10.13.3):**

  - Download [Docker](https://download.docker.com/mac/stable/Docker.dmg) for Mac OS;
  - Double click the DMG file to open it;
  - Drag the docker into Applications and complete installation;
  - Start docker from Launchpad by click it.

  For **Ubuntu (Test on Ubuntu 18.04 LTS):**

  - Go to [Docker](https://download.docker.com/linux/ubuntu/dists/), choose your Ubuntu version, browse to **pool/stable** and choose **amd64, armhf, ppc64el or s390x**. Download the **DEB** file for the Docker version you want to install;
  - Install Docker, supposing that the DEB file is download into following path:**"/home/docker-ce~ubuntu_amd64.deb"**

    ```
      $ sudo dpkg -i /home/docker-ce<version-XXX>~ubuntu_amd64.deb    
      $ sudo apt-get install -f
    ```

  **ii) Verify if Docker is installed correctly**

  Once Docker installation is completed, we can run `hello-world` image to verify if Docker is installed correctly. Open terminal in Mac OS X and Linux operating system and open CMD for Windows operating system, then type the following command:

```
 $ docker run hello-world
```

**Note:** root permission is required for Linux operating system.

- **Step 2**: FreeFlow-ONT installation from Docker Hub

```
# pull latest FreeFlow-ONT Docker image from docker hub
$ docker pull malab/freeflowont
```

- **Step 3**: Launch FreeFlow-ONT local server

```
$ docker run -it -p 8080:8080 malab/freeflowont bash
$ bash ./run.sh
```

Then, FreeFlow-ONT local server can be accessed via [http://localhost:8080](http://localhost:8080/)
