---
topic: containers
title: Tutorial3 - FastQC in Nextflow
---

## Tutorial 2: A Nextflow example with `FastQC` software
In this tutorial,  let's use a [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) analaysis tool that involves working with samples from sequencing experiments. Here, we will learn how to:
- use containers in Nextflow pipeline
- run nextflow as a batch job
- review the results
  

### Understand the container set up for FASTQC software
Nextflow pipelines usually contain many analysis steps and often need multiple containers to run a pipeline. However, for this tutorial we start with one analysis (FastQc analysis) to explain the concept of containerised workflow. The material for this tutorial can be downloaded from Allas object storage as below:

```bash
# Navigate to /scratch area of your project and create a directory with your user name if needed
mkdir -p /scratch/<project>/$USER 
cd /scratch/<project>/$USER

# Download and unpack your tutorial material
wget https://a3s.fi/nextflow/fastqc_nextflow.tar.gz
tar -xavf fastqc_nextflow.tar.gz && rm fastqc_nextflow.tar.gz
```

The downloaded `fastqc_nextflow` folder has the necessary files for running this tutorial. You can use your favourite editor (e.g., nano/vi/vim) to view nextflow script (fastqc.nf) and understand the fastqc commands that are run inside the container. And also check configuration file (nextflow.config) to understand how to activate singularity/apptainer container for this workflow. 

```bash
cd fastqc_nextflow
ls
vi fastqc.nf    # or use nano 
vi nextflow.config 
```
### Run Nextflow pipeline

Let's use a batch script to submit the nextflow job to the cluster. If you are directly pulling container images on the fly, please set $APPTAINER_TMPDIR and $APPTAINER_CACHEDIR to either local scratch (i.e. $LOCAL_SCRATCH) or to your scratch folder (/scratch/<project>) in the batch script. Otherwise $HOME directory, the size of which is only 10 GB, will be used.  To avoid any disk quota errors while pulling images, set $APPTAINER_TMPDIR and $APPTAINER_CACHEDIR as in batch script as shown below:

```bash
#!/bin/bash
#SBATCH --time=00:10:00
#SBATCH --partition=test
#SBATCH --account=<project>
#SBATCH --cpus-per-task=4
#SBATCH --mem-per-cpu=4000

# Let's use the scratch area for temporary and cache storage
export APPTAINER_TMPDIR=/scratch/<project>/$USER
export APPTAINER_CACHEDIR=/scratch/<project>/$USER

# This is just to avoid some annoying warnings
unset XDG_RUNTIME_DIR


# Activate  Nextflow on Puhti
module load  nextflow/23.04.3

nextflow run fastqc.nf
#nextflow run fastqc.nf --reads data2/*_{1,2}_subset.fq.gz
```
Please note that if you use $LOCAL_SCRATCH in the batch script, make sure to request NVMe disk space in sbatch directives by adding `#SBATCH --gres=nvme:<value in GB>`

Replace <project> with correct project number and copy the resulting script to a file ( e.g., nextflow_fastqc.sh). And the submit it to HPC cluster as below:

```bash
sbatch nextflow_fastqc.sh 
```
Once Nextflow jobs gets resources on the cluster, it will executed. The script will pull the necessary container images on the fly and run the FastQC analysis inside of the container.
Monitor slurm output file (slurm-<jobid>.out file) in the same directory. Once analysis is finished, you can see that *fastqc* analysis was performed as shown below:  

```bash
ls -l $PWD/work/*/*
```

### Passing parameters to Nextflow pipeline
Nextflow parameters inside a script are declared by prepending to a variable name with the prefix *params*, separated by dot character (e.g., params.reads). Parameters thus specified in script are used by default. The parameter can also be specified on the commandline by prefixing the parameter name with a double dash character (e.g., --reads). 
 
Here is an example to declare parameters (here, input files) to *fastqc* software inside Nextflow script (NOT for running the command on terminal):

```bash
params.reads = "$baseDir/data/*_{1,2}.fq.gz"
input_ch = Channel.fromFilePairs(params.reads)
```
One can also override parameter values (here files inside `$baseDir/data/` directory) in Nextflow script by passing the parameters in commandline when executing script as shown below:

```bash
nextflow run fastqc.nf --reads data2/*_{1,2}_subset.fq.gz
```
Please note that *data2* folder has different samples (i.e., lymphnode4a samples) than the ones (i.e.,lung3e samples) in *data* folder which could have been used by default. You can now update above nextflow command in the batch script and submit job again.

You can see that *fastqc* analysis was performed on a new set of samples now as shown below:  

```bash
ls -l $PWD/work/*/*
```
> **_NB_**: single dash (`-`) represents core nextflow parameters (e.g., *-resume*). double dash (`--`) represents user-defined and completely extensible one -- they are used to populate `params`.

### Moving results to a convenient directory

Checking resulting files from a workflow analysis as shown above is quite tedious especially when there are several processes inside a workflow. Nextflow provides an easy way to collect resulting files to a convenient place using a special directive, *publishDir*.

Open *fastqc.nf* script in any text editor and uncomment (= remove double slashes) the following line:

```bash
// publishDir params.outdir 
```
and then run pipeline again. But this time, let's use *-resume* flag as we don't need to perform quality control analysis again so that actual analysis is skipped due to the capability of nextflow to track cached results from the previous analysis.  

```nextflow
nextflow run fastqc.nf -resume
```
Once the script is run successfully, you can check the files:

```bash
ls -l results/
````
By using `-resume` flag, the resulting files from previous analysis are simply copied to folder *results* .

