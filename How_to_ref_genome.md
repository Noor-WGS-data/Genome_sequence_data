# How to Setup Your Reference Genome using GATK
###### tags: `tutorials` `reference genome` `GATK`

Use this (i.e. steps 3 and above) if you are planning to create a uBAM first and use GATK BwaAndMarkDuplicatesPipelineSpark to align with the reference genome.

## Step 1: Get your reference genome from FlyBase onto the Cluster
1. Go to this link: [http://ftp.flybase.net/genomes/](http://ftp.flybase.net/genomes/).
2. Click on your species, then reference version number you want.
3. Go into "fasta" directory.
4. Copy the path to the file that you want to download by right clicking the link of your reference sequence (we used all-chromosome option) and choose Copy Link Address
5. Paste the address somewhere and replace the prefix `http` with `ftp`. This will be the link that you use with the`wget` command **on the cluster.**

## Step 2: Setup softwares and directories on your cluster

### Load Softwares: 

Any software you are loading from DCC, you will need to load at the beginning of each session you're using it(i.e. if you load any module from DCC then log off of the cluster entirely, you'll have to load it again when you get back on.) Luckily, it's super easy to load modules from DCC :) 

**1. GATK**: All you need to do is type ```module avail``` and hit enter. This should give you a list of all modules available on the DCC and will tell you exactly which version of GATK to load. 

Then enter the following text into the terminal (once logged onto the cluster). You may need to change the specific version of GATK depending on what is offered at the time you're using this.

```
module load GATK/4.1.9.0
```
**Note:** You may get a message that you need to use the command `java -jar $GATK` to use GATK rather than just writing `gatk`. If you don't feel like typing all that out, there is an option to make an alias for the command. To do so, type in:

```
alias shortName="the full/custom command here"
```
Make sure you have 



**2. Samtools**

## Step 3: Construct Dictionary


Run this code in `code` directory. 
```
#!/bin/bash
#SBATCH -e slurm-%j.err
#SBATCH -o slurm-%j.out
#SBATCH -p common
#SBATCH --mem=1G

#this script takes a fasta genome reference file and produces a dictionary of contig names
#Uses a tool from Picard within GATK4 called CreateSequenceDictionary
#-R=reference_file.fasta
#-O=output_file_name.dict

java -jar $GATK CreateSequenceDictionary -R ../ref/dmel-all-chromosome-r6.41.fasta -O ../ref/dmel-all-chromosome-r6.41.dict
```


**Don't panic** if you get an info error (see below) that failed to detect whether we are running on Google Compute Engine, according to this [link](https://github.com/broadinstitute/gatk/issues/6875).
```
Sep 23, 2021 2:45:39 PM shaded.cloud_nio.com.google.auth.oauth2.ComputeEngineCredentials runningOnComputeEngine
INFO: Failed to detect whether we are running on Google Compute Engine.
```

## Step 4: Construct an index file

```
#!/bin/bash
#SBATCH -e slurm-%j.err
#SBATCH -o slurm-%j.out
#SBATCH -p common
#SBATCH --mem=1G

#this script takes a fasta genome reference file and makes a copy of it in index form
#Uses a tool from SAMtools called faidx
#because SAMtools is not in your directory, you will need to temporarily add it to your PATH so you can run it outside of the SAMtools directory

#PATH=$PATH:/opt/apps/rhel7/samtools-1.9/bin

samtools faidx ../ref/dmel-all-chromosome-r6.41.fasta -o ../ref/test.fai

```

## Step 5: Make an image 
```
#!/bin/bash
#SBATCH -e slurm-%j.err
#SBATCH -o slurm-%j.out
#SBATCH -p common
#SBATCH --mem=1G

#This script creates an index image file (?) of the reference sequence
#It's required for any BWA tools within GATK, like BwaSpark
#where -I-input file (reference), -O-output file (assumed same name, but with .img extension)


java -jar $GATK BwaMemIndexImageCreator -I ../ref/dmel-all-chromosome-r6.41.fasta -O ../ref/dmel-all-chromosome-r6.41.fasta.img

```
Took somewhat longer to run (2-3 minutes(?))