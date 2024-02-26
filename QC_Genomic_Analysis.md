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