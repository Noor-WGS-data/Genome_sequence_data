# 4. Setup Your Reference Genome using BWA
###### tags: `2. Main Steps` `tutorials` `reference genome` 

> Program required: BWA

## Step 1: Get your reference genome from FlyBase onto the Cluster
1. Go to this link: [http://ftp.flybase.net/genomes/](http://ftp.flybase.net/genomes/).
2. Click on your species, then reference version number you want.
3. Go into "fasta" directory.
4. Copy the path to the file that you want to download by right clicking the link of your reference sequence (we used all-chromosome option) and choose Copy Link Address
5. Paste the address somewhere and replace the prefix `http` with `ftp`. This will be the link that you use with the`wget` command **on the cluster.**

## Step 2: Setup softwares and directories on your cluster

### Load Softwares: 

Any software you are loading from DCC, you will need to load at the beginning of each session you're using it(i.e. if you load any module from DCC then log off of the cluster entirely, you'll have to load it again when you get back on.) Luckily, it's super easy to load modules from DCC :) 

**1. BWA**: All you need to do is type ```module avail``` and hit enter. This should give you a list of all modules available on the DCC and will tell you exactly which version of BWA to load. 

Then enter the following text into the terminal (once logged onto the cluster). You may need to change the specific version of BWA depending on what is offered at the time you're using this.

```
module load BWA/0.7.17
```


## Step 3. Make an index for reference genome using BWA

### Notes on why we're using bwa index (rather than GATK)
The GATK "Best Practices" recommend converting .fastq files to uBAM (unaligned BAM) before aligning, but this assumes you will be keeping ONLY the uBAMs as backup, and not the original FASTQs. But because we do not want to delete the original raw data, we are going to go bypass this step and align the .fastq files to the reference genome directly. The GATK 'BwaAndMarkDuplicatesPipelineSpark' does not work with .fastq input, so you will need to use bwa mem to align the .fastq with the reference genome. You will also need to create the reference genome indexes using BWA (NOT using GATK commands - they are not the same and will not work with gwa mem!). So that's a long story as to why you need this code.

1. Make a shell script that contains the following code and save it in your 'code' directory (or whatever you'd like) on the cluster.
```
#!/bin/bash
#SBATCH -e slurm.err
#SBATCH -p common
#SBATCH --mem=1G

#this script takes a fasta genome reference file and makes a copy of it in index form
#Uses a tool from bwa called index


bwa index ../ref/dmel-all-chromosome-r6.41.fasta dmel-all-chromosome-r6.41.fasta 
```
2. In this same directory, type: `sbatch name_of_your_file.sh` and hit enter. 
3. You will see many files output in the same directory that contains your reference genome (they will all contain the prefix of the name of your fasta file and .fasta (e.g. the previx is dmel-all-chromosome-r6.41.fasta). The prefix is important later on in the bwa mem step!