# How to run jobs in parallel

We realized there are a lot of ways to run jobs in parallel on the cluster. Right now, we know three. At the end of this document we have examples of all three, but the first are just basic templates. Note that option #3 (Slurm job array) is the prefered method by the DCC (and probably most cluster systems) because rather than scheduling 100s or 1000s of jobs at once, it sends it one job that dispatches other jobs at a controlled rate. 

## 1) Use two .sh files 

#### Parent .sh

```
#!/bin/bash 	

for A in {1,2,3}; do

sbatch shell_file.sh $A

sleep 1

done
```

#### Daughter .sh

```
#!/bin/bash
#SBATCH --output=output/script-%j.out 
#SBATCH --mem=20G
#SBATCH -p scavenger
#SBATCH --cpus-per-task=1 
#SBATCH --job-name=jobname-%j

A=$1

YOUR COMMAND SCRIPT HERE (using A)
```

## 2) Use one shell script using WRAP

```
#!/bin/bash

for A in {1,2,3}; do

sbatch --wrap="YOUR COMMAND HERE (using A)"

done
```

## 3) Use one shell script using SLURM ARRAY

```
#!/bin/bash
#SBATCH --output=output/script-%j.out 
#SBATCH --mem=20G
#SBATCH -p scavenger
#SBATCH --cpus-per-task=1 
#SBATCH --job-name=jobname-%j
#SBATCH -a 1,2,3
A=${SLURM_ARRAY_TASK_ID}

YOUR COMMAND SCRIPT HERE (using A)
```

## EXAMPLES

## 1) Use two .sh files 

#### Parent .sh (loops_slim_job.sh)

```
#!/bin/bash 			
pwd; hostname; date
for a in 0 0.0001 0.0002 0.0004 0.001
do  	
for i in {1..20}
do 
	sbatch slim_job.sh $i $a
echo "$i"
sleep 1

done
done
```
#### Daughter .sh (slim_job.sh)

```
#!/bin/bash
#SBATCH --output=output/script-%j.out 
#SBATCH --mem=60G
#SBATCH -p scavenger
#SBATCH --cpus-per-task=1 
#SBATCH --job-name=jobname-%j

i=$1
a=$2

slim -d seed=$i -d pb=$a full_model_cluster.slim | tail -n +20 | awk -v run=$i '{print $0" "run}' >> data/output_prop_balancedequals$a.txt
```

## 2) Use one shell script using WRAP

```
#!/bin/bash

#this script aligns .fastq reads to your reference genome

module load BWA/0.7.17

for LL in {19,30,34,45,59,73}; do

sbatch -J ${LL}_alignment-%j --mem=10G -o outs/${LL}_alignment-%j.out -e errs/${LL}_alignment-%j.err --mail-type=ALL --mail-user=sbm34@duke.edu
--wrap= -t 4 "bwa mem ../ref/dmel-all-chromosome-r6.41.fasta ../data/2021_09_21_fastq/L${LL}_R1_001.fastq ../data/2021_09_21_fastq/L${LL}_R2_001.fastq > ../data/2021_09_21_fastq/aligned_sams/aligned_L${LL}.sam"

done

```
## 3) Use one shell script using SLURM ARRAY

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
