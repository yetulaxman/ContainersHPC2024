---
topic: containers
title: Tutorial4 - Run nf-core pipelines at CSC
---
# Running nf-core pipeline on Puhti 

> This tutorial is done on **Puhti**, which requires that:
- You have a [user account at CSC](https://docs.csc.fi/accounts/how-to-create-new-user-account/).
- Your account belongs to a project [that has access to the Puhti service](https://docs.csc.fi/accounts/how-to-add-service-access-for-project/).


💬 nf-core is a community effort to collect a curated set of analysis pipelines built using Nextflow. nf-core facilitates  standardisation of Nextflow pipelines with best practices, guidelines, and templates for bioinformatics community. These workflows are modular, scalable, and portable. Here we use [single-cell RNA-seq pipeline](https://github.com/nf-core/scrnaseq/tree/2.7.1) as an example nf-core pipeline.

## Wrap nf-core pipeline as a normal batch job

Here is an example batch script to run the pipeline on Puhti:
```bash
#!/bin/bash
#SBATCH --time=01:00:00
#SBATCH --partition=small
#SBATCH --account=<project_2xxxx>
#SBATCH --cpus-per-task=4
#SBATCH --mem-per-cpu=4000

# Let's use the fast local drive for temporary storage
export SINGULARITY_TMPDIR=$PWD
export SINGULARITY_CACHEDIR=$PWD

# This is just to avoid some annoying warnings
unset XDG_RUNTIME_DIR
unset XDG_RUNTIME_DIR

# Load Nextflow on Puhti
module load nextflow/23.04.3

# nf-core pipeline example here
nextflow run nf-core/scrnaseq  -profile test,singularity -resume --outdir .
```

‼️ Note! If you are directly pulling multiple images on the fly, please set `$APPTAINER_TMPDIR` and `$APPTAINER_CACHEDIR` to either local scratch (i.e. `$LOCAL_SCRATCH`) or to your scratch folder (`/scratch/<project>`) in the batch script. Otherwise `$HOME` directory, the size of which is  only 10 GB, will be used. To avoid any disk quota errors while pulling images, set `$APPTAINER_TMPDIR` and `$APPTAINER_CACHEDIR` in your batch script.

> Please make sure to specify the correct version of the Nextflow module as some nf-core pipelines require a specific version of Nextflow.

copy and paste the above script to a file named scrna_nfcore.sh and replace your project number with \<project_2xxxx\> in slurm directives.

Finally, submit your job as below:

```bash
sbatch scrna_nfcore.sh
```
💬 When you are running a nf-core  pipeline directly using Nextflow as above, all the GitHub scripts of a given pipeline are download to a directory: ```$HOME/.nextflow/assets/nf-core/```. You can navigate to the directory and explore Nextflow script and configurations settings. Alternatively, you can clone a GitHub repository of a nf-core pipeline and run the pipeline from the same directory.

## Monitor the status of submitted *Slurm* job:

   ```
   squeue -j <slurmjobid>
   # or
   squeue --me
   # or
   squeue -u $USER
   ```


