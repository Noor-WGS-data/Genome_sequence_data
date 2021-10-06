# 5. Align uBAM with reference genome
###### tags: `2. Main Steps` `tutorials` `fastq` `aligned SAM`
> Required software: BWA

## Step 1: Write .sh script  that loops through all sets of forward and reverse fastq files

This code will use bwa mem to output aligned SAM files from your uBAM and the reference genome fasta file. If you only have a single genome you are trying to align, then this step will be a bit less complicated than the examples we've shown below. We are trying to align 6 different individual genomes and would have created a SLURM job array to submit the same bwa mem command for all six sets of forward and reverse sequence. We've also learned that there are many ways to run jobs in parallel. The SLURM job array shown below is preferred by DCC (and especially important if you are trying to run 100s-1000s of jobs at once). If you are curious about other ways to run parallel jobs, see [this tutorial](https://github.com/Noor-WGS-data/Genome_sequence_data/blob/main/Running_jobs_in_parallel.md).

## Using SLURM job array (bwa_mem.sh)

```
#!/bin/bash
#SBATCH -e errs/%a_alignment-%A.err 
#SBATCH -o outs/%a_alignment-%A.out 
#SBATCH --mem-per-cpu=20G 
#SBATCH -p scavenger
#SBATCH --cpus-per-task=4 
#SBATCH --mail-type=ALL --mail-user=rp280@duke.edu
#SBATCH -a 19,30,34,45,5973
LL=${SLURM_ARRAY_TASK_ID}

module load BWA/0.7.17

bwa mem -M -p -t 4 ../ref/dmel-all-chromosome-r6.41.fasta ../data/2021_09_21_fastq/uBAMs/unmapped_adapter_L${LL}.bam ../data/2021_09_21_fastq/alignedBAMs/aligned_L${LL}.bam
```
## Step 2: Run your shell script in the terminal

`sbatch bwa_mem.sh`



