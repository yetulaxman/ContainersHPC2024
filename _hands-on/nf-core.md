---
topic: containers
title: Tutorial4 - nf-core pipeline
---

nf-core is a community effort to collect a curated set of analysis pipelines built using Nextflow. Here we use [single-cell RNA-seq pipeline](https://github.com/nf-core/scrnaseq/tree/2.7.1) as an example nf-core pipeline.

Here is an example batch script to run the pipeline on Puhti:
```bash
#!/bin/bash
#SBATCH --time=01:00:00
#SBATCH --partition=small
#SBATCH --account=<project_xxxx>
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
copy and paste the above script to a file named scrna_nfcore.sh and replace your project number with \<project_xxxx\> in slurm directives.

Finally, submit your job

```bash
sbatch scrna_nfcore.sh
```

