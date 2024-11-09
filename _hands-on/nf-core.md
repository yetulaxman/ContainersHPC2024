---
topic: containers
title: Tutorial4 - Run nf-core pipelines at CSC
---
# Running nf-core pipeline on Puhti 

> This tutorial is done on **Puhti**, which requires that:
- You have a [user account at CSC](https://docs.csc.fi/accounts/how-to-create-new-user-account/).
- Your account belongs to a project [that has access to the Puhti service](https://docs.csc.fi/accounts/how-to-add-service-access-for-project/).

  
nf-core is a community effort to collect a curated set of analysis pipelines built using Nextflow. Here we use [single-cell RNA-seq pipeline](https://github.com/nf-core/scrnaseq/tree/2.7.1) as an example nf-core pipeline.

## Wrap nf-core pipeline as a normal batch job

Here is an example batch script to run the pipeline on Puhti:
```bash
#!/bin/bash
#SBATCH --time=01:00:00
#SBATCH --partition=small
#SBATCH --account=<project_2xxxx>
#SBATCH --cpus-per-task=4
#SBATCH --mem-per-cpu=4000


export SINGULARITY_TMPDIR=$PWD
export SINGULARITY_CACHEDIR=$PWD
unset XDG_RUNTIME_DIR

# Activate  Nextflow on Puhti
module load nextflow/23.04.3

# nf-core pipeline example here
nextflow run nf-core/scrnaseq  -profile test,singularity -resume --outdir .
```

‼️ Note! If you are directly pulling multiple images on the fly, please set `$APPTAINER_TMPDIR` and `$APPTAINER_CACHEDIR` to either local scratch (i.e. `$LOCAL_SCRATCH`) or to your scratch folder (`/scratch/<project>`) in the batch script. Otherwise `$HOME` directory, the size of which is  only 10 GB, will be used. To avoid any disk quota errors while pulling images, set `$APPTAINER_TMPDIR` and `$APPTAINER_CACHEDIR` in your batch script as below:

```bash
     export APPTAINER_TMPDIR=$LOCAL_SCRATCH
     export APPTAINER_CACHEDIR=$LOCAL_SCRATCH
```
Note that this also requires requesting NVMe disk in the batch script by adding `#SBATCH --gres=nvme:<value in GB>`. For example, add  `#SBATCH --gres=nvme:100` to request 100 GB of space on `$LOCAL_SCRATCH`.

> Please make sure to specify the correct version of the Nextflow module as some nf-core pipelines require a specific version of Nextflow.

copy and paste the above script to a file named scrna_nfcore.sh and replace your project number with \<project_xxxx\> in slurm directives.

Finally, submit your job

```bash
sbatch scrna_nfcore.sh
```

## Monitor the status of submitted *Slurm* job:

   ```
   squeue -j <slurmjobid>
   # or
   squeue --me
   # or
   squeue -u $USER
   ```


