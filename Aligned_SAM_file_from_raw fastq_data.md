# How to Create an Aligned SAM file from raw fastq data
###### tags: `tutorials`
> Required software: BWA

## Step 1: Set up Folders in the Cluster



## Step 2: Write .sh script  that loops through all sets of forward and reverse fastq files

This code will use bwa mem to output aligned SAM files from your raw .fastq files (forward and reverse) and the reference genome fasta file. If you only have a single genome you are trying to align, then this step will be a bit less complicated than the examples we've shown below. We are trying to align 6 different individual genomes and would have created a SLURM job array to submit the same bwa mem command for all six sets of forward and reverse sequence. We've also learned that there are many ways to run jobs in parallel. The SLURM job array shown below is preferred by DCC (and especially important if you are trying to run 100s-1000s of jobs at once). If you are curious about other ways to run parallel jobs, see [this tutorial](https://github.com/Noor-WGS-data/Genome_sequence_data/blob/main/Running_jobs_in_parallel.md).

## Using SLURM job array 

```
#!/bin/bash
#SBATCH -e errs/%a_alignment-%A.err 
#SBATCH -o outs/%a_alignment-%A.out 
#SBATCH --mem-per-cpu=20G 
#SBATCH -p scavenger
#SBATCH --cpus-per-task=4 
#SBATCH --mail-type=ALL --mail-user={rp280@duke.edu,sbm34@duke.edu}
#SBATCH -a 19,30,34,45,5973
LL=${SLURM_ARRAY_TASK_ID}

module load BWA/0.7.17

bwa mem -t 4 ../ref/dmel-all-chromosome-r6.41.fasta ../data/2021_09_21_fastq/L${LL}_R1_001.fastq ../data/2021_09_21_fastq/L${LL}_R2_001.fastq > ../data/2021_09_21_fastq/aligned_sams/aligned_L${LL}.sam
```

Mistakes that we ran into:
1. Forgot to type `G` after `--mem=1`
2. Make sure everything is on the same line
3. Ran out of space on the noor partition! And got a FAILED email from the cluster :( So, make sure you have 500+ GB of space on the partition you are working in... 


