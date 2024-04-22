---
title: "QC_Genomic_Analysis"
output: html_document
date: "2024-02-06"
---
## Contents: 
- [**Downloading sequences from Genohub to the HPC**](#download)  
- [**Renaming files for dDocent**](#rename)
- [**Quality control with fastp**](#qc)
- [**Using dDocent**](#dD)
    - [**Renaming files for dDocent**](#rename)
    - [**Running dDocent**](#run)
- [**Troubleshooting**](#trouble)



## <a name="download"></a> **Downloading sequences from Genohub to the HPC**

Connecting to the HPC Andromeda 
```{bash}
ssh -l "your username" ssh3.hac.uri.edu
```

Make a new directory for the sequences 
```{bash}
mkdir QC_Genomic_Data
```

## Data Transfer

This project is eligible for 3 months of free and secure unlimited data storage on Genohub. We have allocated a dedicated S3 bucket in the Amazon Web Services (AWS) cloud. Here is the access information unique to this project:

Bucket name: genohub2422942
For AWS Access Key ID, enter: AKIAXFEWT4A6V2SANYJM   
For AWS Secret Access Key, enter: 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   
For Default Region Mame enter us-east-1
Leave Default output format blank (None)

## Method 2: Using the AWS Command Line Interface (Mac, Windows or Linux)


### Step 1: Install the AWS CLI

Load AWS CLI module on the Andromeda cluster (URI's cluster) to transfer the files

```{bash}
module load awscli/2.0.55-GCCcore-9.3.0-Python-3.8.2 
```

### Step 2: Configure AWS CLI

In a terminal window on HPC run this command

```{bash}
aws configure

```

For AWS Access Key ID, enter: AKIAXFEWT4A6V2SANYJM   
For AWS Secret Access Key, enter: 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   
For Default Region Mame enter us-east-1
Leave Default output format blank (None)


### Step 3: Access the project bucket to upload or download data

Viewing bucket contents
To see the contents of the project bucket:

```{bash}
aws s3 ls s3://genohub2422942 --recursive
```

#### Make a dir for the sequences only

```{bash}
mkdir sequences_genohub2422942

cd sequences_genohub2422942
```

#### Make a script a to download data by submitting a job

```{bash}
nano download_sequences_AWS_genohub2422942.sh
```

```{bash}
#!/bin/bash
#SBATCH -t 200:00:00
#SBATCH --nodes=1 --ntasks-per-node=20
#SBATCH --mail-type=BEGIN  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=END  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=FAIL  --mail-user=willow_dunster@uri.edu
# Set AWS credentials as environment variables
export AWS_ACCESS_KEY_ID="AKIAXFEWT4A6V2SANYJM"
export AWS_SECRET_ACCESS_KEY="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"
cd $SLURM_SUBMIT_DIR
module load awscli/2.0.55-GCCcore-9.3.0-Python-3.8.2
aws s3 sync s3://genohub2422942 .

```

Submit batch job 
```{bash}
sbatch download_sequences_AWS_genohub2422942.sh
#### Submitted batch job 294213
```

Count number of files that downloaded
```{bash}
ls sequences_genohub2422942/*fastq.gz | wc -l
```

## <a name="qc"></a> **Quality control with fastp**
[fastp explained](https://github.com/OpenGene/fastp)

```{bash}
cd /data/pradalab/QC_Genomic_Data

nano genohub2422942_fastp_QC.sh
```

```{bash}
#!/bin/bash
#SBATCH -t 48:00:00
#SBATCH --nodes=1 --ntasks-per-node=20
#SBATCH --mail-type=BEGIN  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=END  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=FAIL  --mail-user=willow_dunster@uri.edu
cd $SLURM_SUBMIT_DIR
module load fastp/0.19.7-foss-2018b
for r1_file in sequences_genohub2422942/*R1*.fastq.gz
do r2_file="${r1_file/_R1_/_R2_}" 
fastp -i $r1_file -I $r2_file -o ${r1_file/.fastq.gz/}.fq.gz -O ${r2_file/.fastq.gz/}.fq.gz --detect_adapter_for_pe --correction -h ${r1_file/_R1_001.fastq.gz/}.fastp.html -j ${r1_file/_R1_001.fastq.gz/}.fastp.json
done

```
Submitted batch job 294216


### Make a folder for FASTQ files after QC

```{bash}

mkdir qc_reads

cd qc_reads
```

### Move QC files to qc_reads


```{bash}
cd /data/pradalab/mgomez/Project_2583431_CPT_2023/qc_reads

mv ../sequences_Project_2583431/*.html .
mv ../sequences_Project_2583431/*.json .
mv ../sequences_Project_2583431/*.fq.gz .
```

### Aggregate results from bioinformatics analyses across many samples into a [single report](https://multiqc.info/)

Login to ssh -l (username) ssh3.hac.uri.edu

```{bash}
module load MultiQC/1.9-intel-2020a-Python-3.8.2

```

Run the multiqc by typing 


```{bash}
multiqc .
```

Rename the report 
```{bash}
mv multiqc_report.html multiqc_report_genohub2422942.html
```

Download this to local to open the html file
Need to go to another terminal window (DONE ON LOCAL COMPUTER NOT ON THE CLUSTER)

```{bash eval=FALSE}
#On local computer
pwd
cd /Users/willo/Documents/GitHub/QC_WGS_23
scp -r willow_dunster@ssh3.hac.uri.edu:/data/pradalab/QC_Genomic_Data/qc_reads/multiqc_report_genohub2422942.html .

```

## <a name="dD"></a> **Using dDocent**
dDocent [webpage](http://www.ddocent.com/why/) and [github](https://github.com/jpuritz/dDocent)

Move files into a test folder

nano move_test.sh
```{bash}
#!/bin/bash
#SBATCH -t 48:00:00
#SBATCH --nodes=1 --ntasks-per-node=20
#SBATCH --mail-type=BEGIN  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=END  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=FAIL  --mail-user=willow_dunster@uri.edu
cd $SLURM_SUBMIT_DIR
cp sequences_genohub2422942/FL* ddocent_test/

```
Submitted batch job 300758

We sequenced out data with Genohub with Dual End Illumina Adapters from the Nextera XT kit. Genohub demultiplexes the data, so I skipped the demultiplexing step in the dDocent workflow

Matias figured out that dDocent is already installed in Andromeda. He searched for modules in Andromeda with
```{bash}
module load d
```
which outprints the possible packages that start with d available in Andromeda

Because it is already isntalled on Andromeda, I can just laod it as a module instead of a conda environment 
```{bash} 
module load ddocent/2.7.8-foss-2018b 

dDocent #to activate the program 
```

### <a name="rename"></a> **Renaming files for dDocent**

Jon provides a [renaming script](https://github.com/jpuritz/dDocent/blob/master/scripts/Rename_SequenceFiles.sh) on the dDocent github. 

Currently my files are named with location-sample# and other genohub identifiers. Forward is denoted with R1 and reverse with R2. Here is an example file name
```
FL-9_S29_R2_001.fastq.gz 
```
To run dDocent sample names must be in this format 
```
location_sample-id. 
```

The dDocent renaming script requires a tab deliminated file with the old sequence name and the new name you want to give each sequence.

Make a list of current sample names to import to excel
```{bash}
cd /data/pradalab/QC_Genomic_Data/ddocent_test
ls *fastq.gz > list_of_fqfiles.txt
```
Copy and paste the above txt file into an [excel file]() and input a new name to adhear to dDocent requirements

Copy the tab deliminated names file from local to Andromeda 
```{bash}
#on local 
cd /Users/willo/Documents/GitHub/QC_WGS_23/Scripts

scp -i ~/.ssh/id_rsa /Users/willo/Documents/GitHub/QC_WGS_23/Scripts/rename.tsv willow_dunster@ssh3.hac.uri.edu:/data/pradalab/QC_Genomic_Data/ddocent_test
```

Check to make sure the file uploaded to Andromeda, then log into the HPC and create a file for the renaming script 
```{bash}
#!/bin/bash
#SBATCH -t 24:00:00
#SBATCH --nodes=1 --ntasks-per-node=1
#SBATCH --export=NONE
#SBATCH --mem=100GB
#SBATCH --mail-type=BEGIN  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=END  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=FAIL  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-user=willow_dunster@uri.edu #your email to send notifications
#SBATCH --account=pradalab                  
#SBATCH --error="script_error_rename" #if your job fails, the error report will be put in this file
#SBATCH --output="output_script_rename" #once your job is completed, any final job report comments will be put in this file

export LC_ALL=en_US.UTF-8

#This script can quickly rename filess within the dDocent naming convention. It needs to be passed a tab delimited
#file with the old name in one column and the new name in the other
#Example#
#PopA_001  NewPop_001
#
# This will renmae PopA_001.F.fq.gz and PopA_001.R.fq.gz to NewPop_001.F.fq.gz and NewPop_001.R.fq.gz

if [ -z "$1" ]
then
echo "No file with old names and new names specified."
echo "Correct usage: Rename_for_dDocent.sh namesfile"
exit 1
else
NAMES=( `cut -f2  $1 `)
BARCODES=( `cut -f1 $1 `)
LEN=( `wc -l $1 `)
LEN=$(($LEN - 1))

echo ${NAMES[0]}
echo ${BARCODES[0]}

for ((i = 0; i <= $LEN; i++));
do
mv ${BARCODES[$i]}_R1_001.fastq.gz ${NAMES[$i]}.F.fq.gz
mv ${BARCODES[$i]}_R2_001.fastq.gz ${NAMES[$i]}.R.fq.gz &>/dev/null
done
fi
```
Submitted batch job 302979


### <a name="run"></a> **Running dDocent**

Create a config file to run dDocent in a non interactive mode

```{bash}
nano config_file.txt
```

```
Number of Processors
24
Maximum Memory
0
Trimming
yes
Assembly?
yes
Type_of_Assembly
PE
Clustering_Similarity%
0.86
Minimum within individual coverage level to include a read for assembly (K1)
2
Minimum number of individuals a read must be present in to include for assembly (K2)
2
Mapping_Reads?
yes
Mapping_Match_Value
1
Mapping_MisMatch_Value
3
Mapping_GapOpen_Penalty
5
Calling_SNPs?
yes
Email
willow_dunster@uri.edu
```

Create a file to run dDocent and submit it as a batch job

```
nano run_ddocent.sbatch
```

```
#!/bin/bash
#SBATCH -t 48:00:00
#SBATCH --nodes=1 --ntasks-per-node=20
#SBATCH --mail-type=BEGIN  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=END  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=FAIL  --mail-user=willow_dunster@uri.edu
cd $SLURM_SUBMIT_DIR

module load ddocent/2.7.8-foss-2018b

dDocent config_file.txt
```



Submitted batch job 303626

## <a name="trouble"></a> **Troubleshooting**

### Cannot find trimmed reads
- Submitted batch job 303002
- When i first submitted a script to Run dDocent I had no selected for trimming reads in the config file. I changed this to yes and the program ran 





## Using dDocent to blast against mt-DNA ref genome 
Move files downloaded from NCBI to local to the HPC
```
scp -r sequence.fasta willow_dunster@ssh3.hac.uri.edu:/data/pradalab

```

Copy and paste 3 files from each group into a new folder for mt-DNA test run 
```
cp BVI-20_S19*.fq.gz BVI-8_S5*fq.gz FL-10_S30*fq.gz FL-5_S17*fq.gz FL-9_S29*fq.gz NI-15_S22*fq.gz NI-26_S53*fq.gz NI-4_S48*fq.gz PR-F1-47_S31*fq.gz PR-F1-53_S67*fq.gz PR-F2-7_S70*fq.gz ../mt-DNA_test/
```
Make a list of current sample names to import to excel
```{bash}
cd /data/pradalab/QC_Genomic_Data/mt-DNA_test

ls *fastq.gz > list_of_fqfiles.txt
```
Copy and paste the above txt file into an [excel file]() and input a new name to adhear to dDocent requirements

Copy the tab deliminated names file from local to Andromeda 
```{bash}
#on local 
cd /Users/willo/Documents/GitHub/QC_WGS_23/Scripts

scp -i ~/.ssh/id_rsa /Users/willo/Documents/GitHub/QC_WGS_23/Scripts/rename-mt-DNA-test.tsv willow_dunster@ssh3.hac.uri.edu:/data/pradalab/QC_Genomic_Data/mt-DNA_test
```

Take renaming script from JP

nano Rename_for_dDocent.sh
```{bash}
#!/bin/bash
#SBATCH -t 24:00:00
#SBATCH --nodes=1 --ntasks-per-node=1
#SBATCH --export=NONE
#SBATCH --mem=100GB
#SBATCH --mail-type=BEGIN  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=END  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=FAIL  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-user=willow_dunster@uri.edu #your email to send notifications
#SBATCH --account=pradalab                  
#SBATCH --error="script_error_rename" #if your job fails, the error report will be put in this file
#SBATCH --output="output_script_rename" #once your job is completed, any final job report comments will be put in this file

export LC_ALL=en_US.UTF-8

#This script can quickly rename filess within the dDocent naming convention. It needs to be passed a tab delimited
#file with the old name in one column and the new name in the other
#Example#
#PopA_001  NewPop_001
#
# This will renmae PopA_001.F.fq.gz and PopA_001.R.fq.gz to NewPop_001.F.fq.gz and NewPop_001.R.fq.gz

if [ -z "$1" ]
then
echo "No file with old names and new names specified."
echo "Correct usage: Rename_for_dDocent.sh namesfile"
exit 1
else
NAMES=( `cut -f2  $1 `)
BARCODES=( `cut -f1 $1 `)
LEN=( `wc -l $1 `)
LEN=$(($LEN - 1))

echo ${NAMES[0]}
echo ${BARCODES[0]}

for ((i = 0; i <= $LEN; i++));
do
mv ${BARCODES[$i]}_R1_001.fq.gz ${NAMES[$i]}.F.fq.gz
mv ${BARCODES[$i]}_R2_001.fq.gz ${NAMES[$i]}.R.fq.gz &>/dev/null
done
fi
```
Submit batch job to rename files

```{bash}
sbatch Rename_for_dDocent.sh rename-mt-DNA-test.tsv
```
Submitted batch job 309347

Now I need to submit a batch job to run dDocent 

Create a config file to run dDocent in a non interactive mode

```{bash}
nano config_file.txt
```
We have a reference genome so we assembly=no 
```
Number of Processors
24
Maximum Memory
0
Trimming
yes
Assembly?
no
Type_of_Assembly
PE
Clustering_Similarity%
0.86
Minimum within individual coverage level to include a read for assembly (K1)
2
Minimum number of individuals a read must be present in to include for assembly (K2)
2
Mapping_Reads?
yes
Mapping_Match_Value
1
Mapping_MisMatch_Value
3
Mapping_GapOpen_Penalty
5
Calling_SNPs?
yes
Email
willow_dunster@uri.edu
```

Create a file to run dDocent and submit it as a batch job

```
nano run_ddocent.sbatch
```

```
#!/bin/bash
#SBATCH -t 48:00:00
#SBATCH --nodes=1 --ntasks-per-node=20
#SBATCH --mail-type=BEGIN  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=END  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=FAIL  --mail-user=willow_dunster@uri.edu
cd $SLURM_SUBMIT_DIR

module load ddocent/2.7.8-foss-2018b

dDocent config_file.txt
```
Submitted batch job 309350 
for this job, I had my reference named "sequence.fasta" but dDocent only recognizes it as "reference.fasta" so I renamed the files and ran it again 

Submitted batch job 309353
I canceled this job


The above didn't seem to be going anywhere so I submitted again which idk if thats problematic 
Submitted batch job 309355


tried troubleshooting from google group 
```{bash}
rm reference.fasta.*

module load SAMtools/1.9-foss-2018b
module load BWA/0.7.17-foss-2018b

samtools faidx reference.fasta
bwa index reference.fasta

```
Then I re-ran dDocent

Submitted batch job 309881

This is the error message I get

```{bash}
Creating alignment intervals
parallel: Warning: The first three values end in CR-NL. Consider using -d "\r\n"

Using FreeBayes to call SNPs
ls: cannot access 'mapped.*.bed': No such file or directory
sh: /dev/tty: No such device or address
0% 0:0=0s                                                                       
rm: cannot remove 'mapped.*.bed': No such file or directory
could not open "raw.*.vcf" exiting
mv: cannot stat 'raw.*.vcf': No such file or directory
```


In interactive mode: 

Troubleshooting w/ JP -- He suggested that there is an issue with creating the vcf file because dDocent is trying to split up jobs between nodes and I only have 1 chromosome for this set because the mt-DNA is so small for these snails. He suggested I do the rest of the workflow manually. This has to be done in interactive mode becasue the main branch has time/power restrictions on it. 


This line of code: 
- Loads FreeBayes
- Specifies an input BAM file - cat-RRG.bam
- Specifies the output file name -v raw.mtdna.vcf
- Specifies the reference genome to align the BAM file to -f reference.fasta
- Sets the min base quality to -m 5
- Sets the min mapping quality -q 5
- Sets the phred-scaled gap extension penalty -E 3
- Sets the min entropy of homopolymer repeats --min-repeat-entropy 1
- Variant sites only -V
- Specifies file with population definitions --populations popmap
- Sets num haplotypes -n 10
- Sets fraction of a sample a variant must be observed in -F 0.1
- Sets the ploidy -p 1

```{bash}
interactive 
module load freebayes/1.3.6-foss-2021b-R-4.1.2
freebayes -b cat-RRG.bam -v raw.mtdna.vcf -f reference.fasta -m 5 -q 5 -E 3 --min-repeat-entropy 1 -V --populations popmap -n 10 -F 0.1 -p 1
```


Filtering: 
```{bash}
interactive
module load VCFtools/0.1.16-foss-2018b-Perl-5.28.0

#Filtering SNP genotyped in 50% 0f indv, min quality of 30, and minor allele count of 3 ######

vcftools --gzvcf raw.mtdna.vcf --max-missing 0.5 --mac 3 --minQ 30 --recode --recode-INFO-all --out raw.g5mac3

```

the above code output: 

```{bash}
Warning: Expected at least 2 parts in FORMAT entry: ID=GL,Number=G,Type=Float,Description="Genotype Likelihood, log10-scaled likelihoods of the data given the called genotype for each possible genotype generated from the reference and alternate alleles given the sample ploidy">
After filtering, kept 12 out of 12 Individuals
Outputting VCF file...
After filtering, kept 130 out of a possible 383 Sites
Run Time = 0.00 seconds
```

Recode any genotype with less than 3 reads 
```{bash}
vcftools --vcf raw.g5mac3.recode.vcf --minDP 3 --recode --recode-INFO-all --out raw.g5mac3dp3
```

Filter out indv that did not sequence well 
```{bash}
vcftools --vcf raw.g5mac3dp3.recode.vcf --missing-indv
```


```
cat out.imiss

INDV	N_DATA	N_GENOTYPES_FILTERED	N_MISS	F_MISS
BVI_14.S130	0	1	0.00769231
BVI_20.S130	0	0	0
BVI_8.S2130	0	0	0
FL_10.S3130	0	1	0.00769231
FL_5.S17130	0	0	0
FL_9.S29130	0	0	0
NI_15.S2130	0	13	0.1
NI_26.S5130	0	0	0
NI_4.S48130	0	13	0.1
PR_47.F11301    0	0	0
PR_53.F11307    0	0	0
PR_7.F2.S70	130	0	0	0
[willow_dunster@n066 mt-DNA_test]$ 
```

Filter out individuals with more then 50% data missing -- Note there is only 10% missing in 2 samples so nothing will be filtered out 

```{bash}
#Identify indv to remove
gawk '$5 > 0.5' out.imiss | cut -f1 > lowDP.indv

#remove indv 
vcftools --vcf raw.g5mac3dp3.recode.vcf --remove lowDP.indv --recode --recode-INFO-all --out raw.g5mac3dplm
```

Filter by mean depth of genotypes -- 95% genotyype call rate, 
```{bash}
vcftools --vcf raw.g5mac3dplm.recode.vcf --max-missing 0.95 --maf 0.05 --recode --recode-INFO-all --out DP3g95maf05 --min-meanDP 20
```

My popmapp is incorrect for these samples ***** NOTE i need to fix this in the renaming script for when I do the whole dataset *****
Here I took a new popmap and copied it to the cluster
```
scp -i ~/.ssh/id_rsa /Users/willo/Documents/GitHub/QC_WGS_23/popmap2.tsv willow_dunster@ssh3.hac.uri.edu:/data/pradalab/QC_Genomic_Data/mt-DNA_test
```

Creating lists for each location
```
gawk '$2 == "BVI"' popmap2.tsv > 1.keep && gawk '$2 == "FL"' popmap2.tsv > 2.keep && gawk '$2 == "NI"' popmap2.tsv > 3.keep && gawk '$2 == "PR"' popmap2.tsv > 4.keep
```

# Trynig actual dDocent with all data 

move fq.gz files into a new directory ddocent_full_run

```{bash}
$ mkdir ddocent_full_run
$ nano move_test.sh

#!/bin/bash
#SBATCH -t 48:00:00
#SBATCH --nodes=1 --ntasks-per-node=20
#SBATCH --mail-type=BEGIN  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=END  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=FAIL  --mail-user=willow_dunster@uri.edu
cd $SLURM_SUBMIT_DIR
cp qc_reads/*fq.gz  ddocent_full_run/
```

Curl the renaming script for dDocent 
```{bash}
$ curl -L -O https://github.com/jpuritz/dDocent/raw/master/Rename_for_dDocent.sh
$ cat Rename_for_dDocent.sh

#!/usr/bin/env bash

if [ -z "$1" ] #Checks to see if there was a barcodefile input
then
echo "No file with barcodes and sample names specified."
echo "Correct usage: Rename_for_dDocent.sh barcodefile"
exit 1
else #if a file was input the code procedes 
NAMES=( `cut -f1  $1 `) #cuts the first column in the barcodes file and assigns to array NAMES
BARCODES=( `cut -f2 $1 `) #cuts the second column in the barcodes file and assigns to array BARCODES
LEN=( `wc -l $1 `) #counts number of lines in barcodesfile
LEN=$(($LEN - 1)) #subtracts 1 from number of lines in barcodesfile

echo ${NAMES[0]} #print first line of NAMES
echo ${BARCODES[0]} #print first line of BARCODES

for ((i = 0; i <= $LEN; i++)); #for loop that iterates over the length of LEN
do
if [ -f sample_${BARCODES[$i]}.2.fq.gz ]; then
mv sample_${BARCODES[$i]}.1.fq.gz ${NAMES[$i]}.F.fq.gz
mv sample_${BARCODES[$i]}.2.fq.gz ${NAMES[$i]}.R.fq.gz
else
mv sample_${BARCODES[$i]}.fq.gz ${NAMES[$i]}.F.fq.gz
fi
done
```
The renaming script needs a file with Names (what you want the file to be called) and Barcodes (what the file is currently called).
SAMPLE NAME = BVI-3_S44_R1*
BARCODE = BVI-3_S44
NAME = BVI_3

Create a txt file with Names and Barcodes for all samples 
```{bash}
$ ls *fq.gz > list_of_fqfiles.txt #list all fq.gz files and print into a new file
$ grep "R1" list_of_fqfiles.txt | sed 's/_R1_.*//' > Barcodes.txt #search for R1 in file, and substitutes anything following R1 by nothing (// left blank)

$ wc -l Barcodes.txt #count lines in file 
58

#I then manually edited PR samples because they had a super funky naming scheme. I did this so I could easily get Names
$awk -F '[-_]' '{print $1 "_" $2}' Barcodes.txt > Names.txt #

$ grep "R1" list_of_fqfiles.txt | sed 's/_R1_.*//' > Barcodes.txt # Revert the Barcodes back to the og

#I then edited the two txt files to add a column header of "Names" and "Barcodes"

paste Names.txt Barcodes.txt > rename_ddocent.txt #paste the two txt files together into one file seperated by a tab

```

Because my file naming scheme is different, I need to edit Rename_for_dDocent.sh

```{bash}
$nano Rename_for_dDocent_edit.sh
#!/usr/bin/env bash

if [ -z "$1" ]; then
    echo "No file with barcodes and sample names specified."
    echo "Correct usage: Rename_for_dDocent.sh barcodefile"
    exit 1
fi

NAMES=($(cut -f1 "$1"))
BARCODES=($(cut -f2 "$1"))
LEN=$(($(wc -l < "$1") - 1))

echo ${NAMES[0]}
echo ${BARCODES[0]}
echo "Number of records: $LEN"

for ((i = 0; i <= $LEN; i++)); do
    mv "${BARCODES[$i]}_R1_001.fq.gz" "${NAMES[$i]}.F.fq.gz" &>/dev/null
    mv "${BARCODES[$i]}_R2_001.fq.gz" "${NAMES[$i]}.R.fq.gz" &>/dev/null
done
```

Run the script -- Submitted batch job 312006
```{bash}
$ sbatch Rename_for_dDocent_edit.sh rename_ddocent.txt 
```

Time to run dDocent: 
Create a config file to run dDocent in a non interactive mode

```{bash}
$ nano config_file.txt

Number of Processors
24
Maximum Memory
0
Trimming
yes
Assembly?
yes
Type_of_Assembly
PE
Clustering_Similarity%
0.90
Minimum within individual coverage level to include a read for assembly (K1)
2
Minimum number of individuals a read must be present in to include for assembly (K2)
2
Mapping_Reads?
yes
Mapping_Match_Value
1
Mapping_MisMatch_Value
3
Mapping_GapOpen_Penalty
5
Calling_SNPs?
yes
Email
willow_dunster@uri.edu
```
Create a batch script to run dDocent
```
$ nano run_ddocent.sbatch

#!/bin/bash
#SBATCH -t 48:00:00
#SBATCH --nodes=1 --ntasks-per-node=20
#SBATCH --mail-type=BEGIN  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=END  --mail-user=willow_dunster@uri.edu
#SBATCH --mail-type=FAIL  --mail-user=willow_dunster@uri.edu
cd $SLURM_SUBMIT_DIR

module load ddocent/2.7.8-foss-2018b

dDocent config_file.txt
```
Submitted batch job 312007
```{bash}
sbatch run_ddocent.sbatch
```

WAIT FOR dDocent to run 