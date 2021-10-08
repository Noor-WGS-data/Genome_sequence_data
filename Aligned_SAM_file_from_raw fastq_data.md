# 5. Align interleaved fastq with reference genome
###### tags: `2. Main Steps` `tutorials` `aligned BAM`
> Required software: BWA

## Step 1: Write .sh script  that loops through all sets of forward and reverse fastq files

This code will use bwa mem to output aligned SAM files from your uBAM and the reference genome fasta file. If you only have a single genome you are trying to align, then this step will be a bit less complicated than the examples we've shown below. We are trying to align 6 different individual genomes and would have created a SLURM job array to submit the same bwa mem command for all six sets of forward and reverse sequence. We've also learned that there are many ways to run jobs in parallel. The SLURM job array shown below is preferred by DCC (and especially important if you are trying to run 100s-1000s of jobs at once). If you are curious about other ways to run parallel jobs, see [this tutorial](https://github.com/Noor-WGS-data/Genome_sequence_data/blob/main/Running_jobs_in_parallel.md).

,30,34,45,59,73

## Using SLURM job array (bwa_mem.sh)

```
#!/bin/bash
#SBATCH -e errs/%a_alignment-%A.err 
#SBATCH -o outs/%a_alignment-%A.out 
#SBATCH --mem-per-cpu=20G 
#SBATCH -p scavenger
#SBATCH --cpus-per-task=4 
#SBATCH --mail-type=ALL --mail-user=rp280@duke.edu
#SBATCH -a 19
LL=${SLURM_ARRAY_TASK_ID}

module load BWA/0.7.17

bwa mem -M -t 4 -p ../ref/dmel-all-chromosome-r6.41.fasta ../data/2021_09_21_fastq/rgadded_adapter_L${LL}.fastq > ../data/2021_09_21_fastq/alignedBAMs/aligned_L${LL}.bam 
```
## Step 2: Run your shell script in the terminal

`sbatch bwa_mem.sh`


## Step 3: MergeBamAlignment

This step merges the alignment information from the product fastq from BWA MEM and the read groups and metadata from uBAM from steps earlier together. 

```
#!/bin/bash
#SBATCH -e errs/%a_mergeBam-%A.err 
#SBATCH -o outs/%a_mergeBam-%A.out 
#SBATCH --mem-per-cpu=20G 
#SBATCH -p scavenger
#SBATCH --cpus-per-task=4 
#SBATCH --mail-type=ALL --mail-user=rp280@duke.edu
#SBATCH -a 19
LL=${SLURM_ARRAY_TASK_ID}

module load GATK/4.1.9.0 

java -Xmx16G -jar $GATK MergeBamAlignment \
-O ../data/2021_09_21_fastq/mergedBAMs/merged_L${LL}.bam \
-R ../ref/dmel-all-chromosome-r6.41.fasta \
-UNMAPPED_BAM ../data/2021_09_21_fastq/uBAMs/unmapped_L${LL}.bam \
-ALIGNED_BAM ../data/2021_09_21_fastq/alignedBAMs/aligned_L${LL}.bam \
-CREATE_INDEX true \
-ADD_MATE_CIGAR true \
-CLIP_ADAPTERS false \
-CLIP_OVERLAPPING_READS true \
-INCLUDE_SECONDARY_ALIGNMENTS true \
-MAX_INSERTIONS_OR_DELETIONS -1 \
-PRIMARY_ALIGNMENT_STRATEGY MostDistant \
-ATTRIBUTES_TO_RETAIN XS \
-TMP_DIR /work/$USER
```
ran into the error
```
 Could not find dictionary next to reference file
```
LMAO WE NEEDED THE DICTIONARY AND INDEX FOR GATK AFTER ALLLLLL
Psych!! It's good that we kept 'em.
