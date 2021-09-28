# How to QC Raw Data

Use FastQC to look at the quality of the raw sequence data. 

****ADD TEXT TO EXPLAIN HOW TO DOWNLOAD PNGS and look at them

### Code 

```
#!/bin/bash
#SBATCH -e slurm-%j.err
#SBATCH -o slurm-%j.out
#SBATCH -p common
#SBATCH --mem=1G

# Manual
# fastqc seqfile1 seqfile2 .. seqfileN
# fastqc [-o output dir] [--(no)extract] [-f fastq|bam|sam] [-c contaminant file] seqfile1 ... seqfileN

# gunzip is the same as gzip -d
gunzip -k ../data/2021_09_21_fastq/*.fastq.gz
for FILE in ../data/2021_09_21_fastq/*.fastq; do
fastqc -o ../data/2021_09_21_fastq/QC_report ${FILE}
done
```