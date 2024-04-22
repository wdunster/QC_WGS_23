---
title: "QC_Genomic_Analysis"
output: html_document
date: "2024-02-06"
---
Contents: 
- [**Downloading sequences from Genohub to the HPC**](#download)    


## <a name="download"></a> **Downloading sequences from Genohub to the HPC**

Connecting to the HPC Andromeda 
```{bash}
ssh -l "your username" ssh3.hac.uri.edu
```

Make a new directory for the sequences 
```{bash}
mkdir QC_Genomic_Data
```

# Data Transfer

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

# Make a dir for the sequences only

```{bash}
mkdir sequences_genohub2422942

cd sequences_genohub2422942
```

#### Making a script a to download data by submitting a job

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

### Quality control with fastp https://github.com/OpenGene/fastp

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


#### Make a folder for FASTQ files after QC with fastp 

```{bash}

mkdir qc_reads

cd qc_reads
```

#### Move QC files to qc_reads


```{bash}
cd /data/pradalab/mgomez/Project_2583431_CPT_2023/qc_reads

mv ../sequences_Project_2583431/*.html .
mv ../sequences_Project_2583431/*.json .
mv ../sequences_Project_2583431/*.fq.gz .
```

#### Aggregate results from bioinformatics analyses across many samples into a single repor https://multiqc.info/

#### Login to ssh -l (username) ssh3.hac.uri.edu

```{bash}
module load MultiQC/1.9-intel-2020a-Python-3.8.2

```

#### Run the multiqc just by typing 


```{bash}
multiqc .
```

Rename report 
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

#Now I want to use DDocent to build a pseduogenome and identify snps


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



Load the Miniconda environment 
```{bash}
module load Miniconda3/4.9.2
```

Add biconda channels 
```{bash}
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
```

Create a dDocent conda environment 
```{bash}
conda create -n ddocent_env ddocent
```
Output: 
Collecting package metadata (current_repodata.json): done
Solving environment: failed with repodata from current_repodata.json, will retry with next repodata source.
Collecting package metadata (repodata.json): done
Solving environment: / 

I tried submitting as a batch job because above work was taking +1hr to run 
```{bash}
nano create_conda.sh
```

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
#SBATCH --error="script_error_create_conda" #if your job fails, the error report will be put in this file
#SBATCH --output="output_script_create_conda" #once your job is completed, any final job report comments will be put in this file

source /usr/share/Modules/init/sh # load the module function

module load Miniconda3/4.9.2

conda create -n ddocent_env ddocent
```
Submitted batch job 302883
Submitted batch job 302885 added x3 config lines to above script 