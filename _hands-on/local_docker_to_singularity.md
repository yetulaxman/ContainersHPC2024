---
topic: docker
title: Tutorial2 -  Converting a local Docker image to Apptainer 
---
Sometimes, container images may not be readily available in registries for our purpose. In that case, we have to either modify an existing docker image or build a new one. Unfortunately, the docker-related operations can only be done on our local machines or any host machine where we have privileged root access. This tutorial explains how to build a Apptainer image from a local docker image in play-with-docker environment. 

###  Expected outcome of this tutorial:
After this tutorial, you will learn to:
- Save a docker image locally ( In PWD environment)
- Launch a Appainer container from a local docker image (In Puhti environment)

### Converting a local docker image to Apptainer 

1. Let's use the [trimmomatic software] as an example software as available in [Red hat Quay Registry](https://quay.io/). <a href="http://labs.play-with-docker.com/" target="_blank"> In PWD terminal</a>, run the following command to pull an image:

   ```bash
    docker pull quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
   ```
   In a real-world use case, you may want to modify the image pulled from registries or even build new one on your local machine to meet your needs. 
  
2. Once an image is pulled or built on your local machine, the image is stored in local registry (usually at /var/lib/docker) on host machines. In order to find
   the stored docker images, use **docker images** command. 
  
   ```bash  
   docker images
   ```
   From the above command, you need to find an image ID of trimmomatic image to save it locally. 
  
3. Create a tarball of the Docker image (with image id which in my case is cc8b303fee58)  using the **docker save** command as below:
  
   ```bash
   docker save cc8b303fee58 -o trimmomatic_image.tar  # in this case image_id is : cc8b303fee58
   ls -l  trimmomatic_image.tar  # to see the local copy of image tarball
   ```

4. Copy the image tarball from PWD to your **scratch** folder on Puhti 

   ```  
   scp trimmomatic_image.tar yourcscusername@puhti.csc.fi:/scratch/project_xxx/yourcscusername
   ```

5. Build Apptainer image from the tarball. 
 
    ```bash
    cd /scratch/project_2xxxx/yourcscusername  # replace `project_2xxxx` with a valid project number 
    apptainer build local_trimmomatic_image.sif docker-archive://trimmomatic_image.tar
    ```
  
6. Launch Apptainer container and check if you can get a command-line help for trimmomatic software

    ```bash
   appatiner exec local_trimmomatic_image.sif trimmomatic --help
   ```
 
