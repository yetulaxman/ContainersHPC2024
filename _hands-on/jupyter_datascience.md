---
topic: containers
title: Tutorial1 - Launch Jupyter notebook on Puhti web interface using a pre-built docker image from a container registry
---

In this tutorial, you will learn how to:
  - Install Jupyter notebook based on a pre-existing docker image from a docker registry.
  - Launch the Jupyter notebook from [Puhti Web Interface](https://www.puhti.csc.fi/public/).
    
## Search a data science notebook image from a Docker registry

For illustration, let's use a data science Jupyter notebook managed by the [Jupyter project](https://github.com/jupyter). One can find such ready-made Docker container images in Docker registries such as [Docker Hub](https://hub.docker.com/) and [Red Hat Quay](https://quay.io/). On these registries, you can search with keyword "data sciene" and then select "jupyter/datascience-notebook" to see the images for Jupyter notebook.

## Install a Jupyter docker image to *projappl* directory using *tykky* container wrapper
While the container images could be pulled and used directly as singularity containers on HPC systems, CSC’s [Tykky](https://docs.csc.fi/computing/containers/tykky/) container wrapper provides an easy method to install them to be usable without any special container commands. For the purpose of this tutorial, we use a data science image from [Docker Hub](https://hub.docker.com/r/jupyter/datascience-notebook). Please prepare and install Jupyter notebook environment to *projappl* directory as below:  

```bash
# Navigate to /projappl area of your course project 
cd /projappl/project_20xxxx/      # Make sure to replace 20xxxx with correct course project number
module purge                      # Clean your environment
module load tykky                 # load the Tykky container wrapper
mkdir -p /projappl/project_20xxxx/$USER  &&  mkdir -p /projappl/project_20xxxx/$USER/Notebook
# You can use the wrap-container command from tykky module to install image binaries to /projappl
wrap-container -w /opt/conda/bin docker://docker.io/jupyter/datascience-notebook:x86_64-ubuntu-22.04 --prefix /projappl/project_200xxxx/$USER/Notebook
# This installation can take for a while
# The -w option specifies the installation directory inside the container. For this data science container image, path is /opt/conda/bin
# The --prefix option specifies the directory where we want to install the software on the host system.
```
Upon succesfull installation, the executables of the Jupyter notebook will be available in the directory /projappl/project_200xxxx/$USER/Notebook/bin. 

## Download a sample Jupyter notebook 

You can download an example notebook to perform basic data analysis tasks using the installed Jupyter notebook as below: 
```bash
cd /projappl/project_200xxxx/$USER
wget https://a3s.fi/CSC_training/course_notebook.tar.gz
tar -xavf course_notebook.tar.gz
```

## Launch the installed Jupyter notebook from the Puhti web interface

1. Log in to [Puhti web interface](https://www.puhti.csc.fi)
2. Login with CSC/HAKA/VIRTU credentials (Please note that a user should have accepted Puhti service in [myCSC](https://my.csc.fi/welcome) 
    page under a course (or own) project before using this service). 
3. Once login is successful, select "Jupyter" icon from the pinned apps on the landing page.  Once you open the Jupyter, use the following settings:
    Reservation: use course reservation if available  
    Project: project_20xxxx   
    Partition: small  
    Number of CPU cores: 2  
    Memory (Gb): 2  
    Local disk: 0  
    Time: 0:30:00  
    Python: custom path  
    Custom Python interpreter: /projappl/project_200xxxx/$USER/Notebook/bin  
    Working directory: /projappl/project_200xxxx/  
    and finally "Launch" notebook  
   
 4. This may take a while before seeing "Connect to Jupyter" button upon successful launching. Once Jupyter notebook is opened, navigate to your own $USER area (under /projappl/project_200xxxx/) where you have downloaded example Jupyter notebook earlier. Finally, click on "explore_datascience.ipynb" to open a sample Jupyter notebook on your browser.

###  Useful CSC documentation

- [Tykky containerisation](https://docs.csc.fi/computing/containers/tykky/)
- [Jupyter for course](https://docs.csc.fi/computing/webinterface/jupyter-for-courses/)

