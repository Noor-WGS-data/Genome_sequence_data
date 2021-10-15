# SnpEff and SnpSift
###### tags: `3. Miscellaneous`

[SnpEff (and SnpSift)](http://pcingola.github.io/SnpEff/) are useful software that will annotate and filter VCF files (and lots of other things, but I haven't used most of it's capabilities). Below we have taken a VCF file of chromosome 2 of *Drosophila melanogaster* and annotated it using SnpEff. We used SnpSift to filter the annotated file by location on the chromosome and by the predicted 'effect impact' (HIGH). 

## Step 1: Upload SnpEff to your home directory

### SnpEff
`wget https://snpeff.blob.core.windows.net/versions/snpEff_latest_core.zip`

`unzip snpEff_latest_core.zip`


## Step 2: Run SnpEff to create an annotated VCF


```
#!/bin/bash
#SBATCH -e errs/%a_SnpEff_chr2-%A.err 
#SBATCH -o outs/%a_SnpEff_chr2-%A.out 
#SBATCH --mem-per-cpu=20G 
#SBATCH -p scavenger
#SBATCH --mail-type=ALL --mail-user=sbm34@duke.edu

module load Java/1.8.0_60

java -Xmx8g -jar /work/sbm34/lethal/snpEff/snpEff.jar \
-c /work/sbm34/lethal/snpEff/snpEff.config Drosophila_melanogaster \
-o gatk \
/work/sbm34/lethal/L19/dmel_L19_chr2.vcf > /work/sbm34/lethal/L19/Dmel_L19_chr2.ann.vcf
```

## Step 3: Run SnpSift to filter your VCF (on location, variant 'impact')

```
#!/bin/bash
#SBATCH -e errs/L19_SnpSift_ed1725.err 
#SBATCH -o outs/L19_SnpSift_ed1725.out 
#SBATCH --mem-per-cpu=20G 
#SBATCH -p scavenger
#SBATCH --mail-type=ALL --mail-user=sbm34@duke.edu

module load Java/1.8.0_60

java -Xmx8g -jar /work/sbm34/lethal/snpEff/SnpSift.jar filter \
"( CHROM = '2R' ) & \
( POS > 7613924 ) & ( POS < 8156045 ) & \
((ANN[*].IMPACT = 'HIGH'))" \
/work/sbm34/lethal/L19/dmel_L19_chr2.ann.vcf > /work/sbm34/lethal/L19/dmel_L19_highimp_ed1725.vcf 
```

## Push VCF file to your home computer:

This isn't working yet: get the error: ssh: Could not resolve hostname dcc-slogin.oit.duke.edu: nodename nor servname provided, or not known

```
scp sbm34@dcc-slogin.oit.duke.edu:/work/sbm34/lethal/L19/dmel_L19_highimp_ed1725.vcf ~/Downloads
```




