# transcript-analysis-workflow-No-reference-genome
If the transcriptome data of the species you are analyzing does not have a corresponding reference genome, please refer to this workflow.

# 1. Installation

## 1.1 Install Docker

Since this workflow is based on Snakemake (workflow management) and Docker, Docker must be installed on your server first.

## 1.2 Launch the Container with the Workflow

Pull the Docker image:

```bash
docker pull siyinobelma/denovo-snake:1.1
```

Create and run the container in the background:
```bash
docker run -itd --name RNA_snakemake siyinobelma/denovo-snake:1.1 bash  # RNA_snakemake is the container name (can be customized as needed)
```

# Enter the container:

```
docker exec -it RNA_snakemake bash
```

# 2. Prepare Input Files
The workflow requires multiple input files: compressed sequencing files (.fastq.gz), workflow configuration file (.yaml), and sample information file (.csv).
|    File Type      |        Description              |
| :---------------: | :------------------------: |
| sequence.fastq.gz |   User-provided compressed sequencing files |
|    config.yaml    | Configuration file for customizing the workflow |
|    samples.txt    |       Sample information file      |

## 2.1 Input Files (.fastq.gz)
These are user-provided input files supporting paired-end sequencing data. Note: Ensure files are compressed in .gz format with the .fastq.gz suffix.

## 2.2 Configure the Workflow
Modify the sample information table (/home/configs/samples.txt):
|   endo   |   SRR7403459   |   /home/Rhododendron_01/fastp/SRR7403459-1.fastq.gz   |   /home/Rhododendron_01/fastp/SRR7403459-2.fastq.gz   |
| :------: | :------------: | :---------------------------------------------------: | :---------------------------------------------------: |
| **endo** | **SRR7403460** | **/home/Rhododendron_01/fastp/SRR7403460-1.fastq.gz** | **/home/Rhododendron_01/fastp/SRR7403460-2.fastq.gz** |
| **eco**  | **SRR7403463** | **/home/Rhododendron_01/fastp/SRR7403463-1.fastq.gz** | **/home/Rhododendron_01/fastp/SRR7403463-2.fastq.gz** |
| **endo** | **SRR7403464** | **/home/Rhododendron_01/fastp/SRR7403464-1.fastq.gz** | **/home/Rhododendron_01/fastp/SRR7403464-2.fastq.gz** |
| **eco**  | **SRR7403465** | **/home/Rhododendron_01/fastp/SRR7403465-1.fastq.gz** | **/home/Rhododendron_01/fastp/SRR7403465-2.fastq.gz** |
| **eco**  | **SRR7403466** | **/home/Rhododendron_01/fastp/SRR7403466-1.fastq.gz** | **/home/Rhododendron_01/fastp/SRR7403466-2.fastq.gz** |

#### Column 1: Sample treatment condition
#### Column 2: Sample ID
#### Columns 3-4: Quality-controlled sequencing data paths (formatted as {OUTPUTPATH}/fastp/{sampleID}-1.fastq.gz and {sampleID}-2.fastq.gz in config.yaml).

##### Note: Customize the workflow in /home/configs/config.yaml with the following variables:
```
PROJECT: Project name
SAMPLES: [List of sequencing file names]
INPUTPATH: Input directory path
OUTPUTPATH: Output directory path
THREAD: Number of threads for software execution
INFO: Sample information table path
TREATMENT: [List of sample treatment conditions]
SPECIES: Species name
```
# 3. Run the Workflow

```
# Run in the /home/run directory:
snakemake -s run.py -j 6          # -j specifies the number of concurrent jobs (adjust as needed)
# Run in the background:
nohup snakemake -s run.py -j 6 &
```

##### Note: Running run.py directly generates gene expression metrics (TPM, count matrix), differential gene analysis results, predicted transcript ORFs, and annotation reports from sequencing data.

# 4. Example Data
Example sequencing data source: Rhododendron delavayi var. delavayi (ID 476831) - BioProject - NCBI (nih.gov)
SRR IDs used: "SRR7403463", "SRR7403465", "SRR7403466", "SRR7403459", "SRR7403460", "SRR7403464"
