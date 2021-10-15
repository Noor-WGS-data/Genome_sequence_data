# GATK Workflow Overview
###### tags: `1. Overview`

## Overview
This document will outline all the steps that you will need to align short read genome sequence data (from raw fastq to aligned BAM to VCF). Each of the following are listed in order of the steps that we took and will link to more detailed information (including explanations for each step and the script we used). 

We started with *Drosophila melanogaster* short read sequence data from 6 different lines. Our end goal was to output a chromosome 2 VCF for each line. Our next steps were to search for 'high impact variants' in certain regions of chromosome 2 using [SnpEff and SnpSift](http://pcingola.github.io/SnpEff/) (this step is not outlined below, but you can find information on how we did this in our 'Tutorials' GitHub directory).

1. [Download `.fastq` files onto cluster](https://github.com/Noor-WGS-data/Genome_sequence_data/blob/main/1.%20Transfer%20raw%20fastq%20to%20cluster.md)
2. [Run fastQC on your sequence data](https://github.com/Noor-WGS-data/Genome_sequence_data/blob/main/2.%20How_to_QC_fastq.md)
3. [Prepare uBAM for Alignment](https://github.com/Noor-WGS-data/Genome_sequence_data/blob/main/3.%20Raw%20fastq%20to%20uBAM%20and%20mark%20adapters.md)
    a. Convert fastq to uBAM 
    b. Mark Illumina Adapters 
4. [Download and setup the reference genome using BWA and GATK](https://github.com/Noor-WGS-data/Genome_sequence_data/blob/main/4.%20Setup%20Your%20Reference%20Genome%20using%20BWA%20and%20GATK.md)
5. [Align interleaved fastq files with the reference genome using BWA MEM](https://github.com/Noor-WGS-data/Genome_sequence_data/blob/main/5.%20uBAM%20to%20interleaved%20fastq%2C%20align%20with%20reference%20and%20then%20merge.md)
    a. Convert uBAM to interleaved fastq 
    b. Align interleaved fastq with reference genome
    c. Merge uBAM readgroups and adapters with aligned BAM
6. [Mark Duplicates using MarkDuplicatesSpark](https://github.com/Noor-WGS-data/Genome_sequence_data/blob/main/6.%20MarkDuplicatesSpark.md)
7. [Calculate Coverage](https://github.com/Noor-WGS-data/Genome_sequence_data/blob/main/7.%20Calculate_genome_coverage.md)  
8. [Variant Calling and Filtration](https://github.com/Noor-WGS-data/Genome_sequence_data/blob/main/8.Variant_calling_and_filtration.md)
    a. Index BAM
    b. Haplotype Caller
    c. Genotype GVCFs
    d. Select Variants (SNPs and Indels seperate)
    e. Count Variants (before filtration)
    f. Filter and Select Variants
    g. Count Variants (that pass filtration)
    h. Merge SNPs and Indel VCFs into one
    i. Gather VCFs from multiple regions into one (if needed)
  

