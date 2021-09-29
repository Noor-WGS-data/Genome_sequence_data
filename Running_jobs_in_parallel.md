# How to run jobs in parallel

We realized there are a lot of ways to run jobs in parallel on the cluster. Right now, we know three. At the end of this document we have examples of all three, but the first are just basic templates.

## 1) Use two .sh files 

#### Parent .sh

```
#!/bin/bash 	

for A in {1,2,3}; do

sbatch shell_file.sh $A

done
```

#### Daughter .sh

```
#!/bin/bash
#SBATCH --output=output/script-%j.out 
#SBATCH --mem=100G
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
#SBATCH --mem=100G
#SBATCH -p scavenger
#SBATCH --cpus-per-task=1 
#SBATCH --job-name=jobname-%j
#SBATCH -a 1,2,3
A=${SLURM_ARRAY_TASK_ID}

YOUR COMMAND SCRIPT HERE (using A)
```