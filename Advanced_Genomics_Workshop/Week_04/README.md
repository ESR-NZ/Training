#### Author: Rhys White ([@RhysTWhite](https://twitter.com/RhysTWhite))
#### Affiliations: Institute of Environmental Science and Research - ESR

# Advanced Genomic Insights Workshop Week 04: a tutorial on sequence data quality control

Table of contents
=================

<!--ts-->
   * [Prerequisites](#prerequisites)
   * [Introduction](#introduction)
   * [Let's get started...](#lets-get-started)
      * [Analysing Quality of Sequence Read Data with FastQC](#analysing-quality-of-sequence-read-data-with-fastqc)
      * [Sequence Data Processing with Trimmomatic: Quality Filtering and Adapter Removal](#sequence-data-processing-with-trimmomatic-quality-filtering-and-adapter-removal)
      * [Conquering Taxonomic Classification with Kraken2](#conquering-taxonomic-classification-with-kraken2)
      
      
   * [Why have I learnt this?](#why-have-i-learnt-this)
   * [Final notes](#final-notes)
   * [Data availability](#data-availability)
<!--te-->

## Prerequisites
To fully benefit from these exercises, it is essential that you manually _type_ each exercises. Please avoid the temptation to copy and paste, as the purpose behind these exercises is to train your hands, brain, and mind in the fundamental skills of reading, writing, and comprehending code. By copying and pasting, you would be depriving yourself of the valuable learning experience these lessons provide.

Before we begin, you should have a basic understanding of genomics and the concept of DNA sequencing. You should also be familiar with the command line and have access to a Unix-based operating system (such as Linux or macOS).

## Introduction

In the world of genomics and bioinformatics, it is crucial to ensure the quality of the sequencing data before proceeding with any downstream analyses. In this tutorial, you will receive a step-by-step guide on utilising three 'gold-standard' tools to guarantee that your Illumina sequencing data is of the highest quality before beginning your analyses.

## Let's get started...

##### Getting the data
Note: The data is ~40 MB. To download the data, please run these commands:

```bash
# Download the file with forward paired-end reads:
wget https://raw.githubusercontent.com/ESR-NZ/Training/main/Rhys_Toolbox/01_Quality_control/Corella_11_Sub_1.fastq.gz
```

```bash
# Download the file with reverse paired-end reads:
wget https://raw.githubusercontent.com/ESR-NZ/Training/main/Rhys_Toolbox/01_Quality_control/Corella_11_Sub_2.fastq.gz
```

You have been given two `fastq.gz` files. Each file is a compressed text file containing DNA sequence data and corresponding quality scores generated by Illumina sequencing machines. The `${SAMPLE}_1.fastq.gz` and `${SAMPLE}_2.fastq.gz` files are paired-end Illumina sequencing data files representing forward and reverse reads, respectively, from the same DNA fragment.

To begin, create a directory called `quality_control` and move both files into it. Afterwards, navigate to the `quality_control` folder:


```bash
# Make a directory called quality_control:
mkdir quality_control
```

```bash
# Move all files ending in .fastq.gz into the quality_control directory:
mv /path/to/*.fastq.gz quality_control/.
```

```bash
# Change directory to your quality_control directory
cd quality_control
```

To confirm that both files are present in your current working directory, check for their existence using the following command:

```bash
# Directory listing
ls
```

### Analysing Quality of Sequence Read Data with FastQC

`FastQC` is a quality control tool for high throughput sequence data, which provides an intuitive interface and in-depth analyses of the sequence read data, enabling the detection of several common issues, such as adapter contamination, sequence quality, GC content, and overrepresented sequences. `FastQC` supports multiple file formats, including FASTQ, BAM, and SAM. 

You can find the `FastQC` manual and examples of good and bad Illumina sequencing data by following this link: [https://www.bioinformatics.babraham.ac.uk/projects/fastqc/](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/).

#### Running FastQC

Once you are in the directory containing your FASTQ files, you can run `FastQC` on the command line using the following syntax:

````
fastqc [options] ${SAMPLE}_1.fastq.gz ${SAMPLE}_2.fastq.gz 
````

Replace `${SAMPLE}_1.fastq.gz` and `${SAMPLE}_2.fastq.gz` with the names of your FASTQ files.

Some standard options you may want to include are:

`-o` or `--outdir` to specify the output directory where the `FastQC` reports will be saved.<br>
`-t` or `--threads` to specify the number of threads (CPU cores) to use for analysis.<br>

For example, to run `FastQC` on two paired-end FASTQ files with 4 threads and save the reports in a directory named `fastqc_reports`, the command would be:

```
fastqc -t 4 -o fastqc_reports ${SAMPLE}_1.fastq.gz ${SAMPLE}_2.fastq.gz
```

#### Viewing the FastQC reports

After `FastQC` has finished running, navigate to the output directory specified in the previous step to view the reports. Each FASTQ file will have its own report, which can be opened in a web browser by clicking on the HTML file.

The `FastQC` reports provide information about the quality of the sequencing data, including metrics such as per-base quality scores, GC content, and sequence length distribution. The reports can be used to identify potential issues with the sequencing data and to guide downstream analysis.

**Questions**<br>
**(i) How many reads does each of these fastq files contain?**<br>
**(ii) Are these single-end or paired-end reads?**<br>
**(iii) How long is each of the reads?**<br>

### Sequence Data Processing with Trimmomatic: Quality Filtering and Adapter Removal

`Trimmomatic` is a popular and powerful tool for trimming raw sequence reads to remove low-quality bases and adapter sequences, which can negatively impact downstream analysis, such as genome assembly and variant calling. In this section, you will use `Trimmomatic` on the command line to process Illumina sequence read data. By the end of this section, you will be able to use `Trimmomatic` to preprocess your Illumina read data and improve the accuracy and reliability of your downstream analysis.

You can find the `Trimmomatic` manual by following this link: [https://github.com/usadellab/Trimmomatic](https://github.com/usadellab/Trimmomatic).

Remember to cite the paper:

&emsp; Bolger AM, Lohse M, Usadel B. Trimmomatic: a flexible trimmer for Illumina sequence data. _Bioinformatics_ 2014;30:2114-2120 doi: [10.1093/bioinformatics/btu170](https://doi.org/10.1093/bioinformatics/btu170)

##### Getting the adapter fasta file

Disclaimer: Illumina adapter and other technical sequences are copyrighted by Illumina, but the developers of `Trimmomatic` have been granted permission to distribute them with `Trimmomatic`.
 
To download the adapter fasta file, please run these commands:

```bash
# Download the file with forward paired-end reads:
wget https://raw.githubusercontent.com/ESR-NZ/Training/main/Rhys_Toolbox/01_Quality_control/trimmomatic.fa
```

#### Prepare your input files

`Trimmomatic` requires input files in FASTQ format. Therefore, ensure your input files are in the correct format and your current directory.

#### Determine the quality score cutoff

`Trimmomatic` uses a sliding window approach to determine the quality of each base in a read. The quality score cutoff is the minimum average quality score required for a base to be kept in a read. For example, a typical quality score cutoff is 20, corresponding to a 1% error rate.

#### Running Trimmomatic

Once you are in the directory containing your FASTQ files, you can run `Trimmomatic` on the command line using the following syntax:

````
trimmomatic PE -threads ${CPUS} -trimlog ${SAMPLE}_trim.log ${SAMPLE}_1.fastq.gz ${SAMPLE}_2.fastq.gz ${SAMPLE}_trimmed_1.fastq.gz ${SAMPLE}_unpaired_1.fastq.gz ${SAMPLE}_trimmed_2.fastq.gz ${SAMPLE}_unpaired_2.fastq.gz ILLUMINACLIP:trimmomatic.fa:1:30:11 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
````

Replace `${SAMPLE}_1.fastq.gz` and `${SAMPLE}_2.fastq.gz` with the names of your input files.

Replace `${SAMPLE}_trimmed_1.fastq.gz`, `${SAMPLE}_unpaired_1.fastq.gz`, `${SAMPLE}_trimmed_2.fastq.gz`, and `${SAMPLE}_unpaired_2.fastq.gz` with the names you want for your output files.

The options following the adapter file are explained below:

```
ILLUMINACLIP:adapters.fa:1:30:11    specifies the adapter file, the number of mismatched bases allowed when searching for adapter sequences, the palindrome clip threshold, and the simple clip threshold.
LEADING:3 and TRAILING:3            specify the quality score cutoff for the first and last bases of a read, respectively.
SLIDINGWINDOW:4:15                  specifies the window size and average quality score cutoff for the sliding window approach.
MINLEN:36                           specifies the minimum length of a read to be kept after trimming.
```

Depending on the size of your input files, `Trimmomatic` may take some time to complete.

Once `Trimmomatic` has finished running, you should see your output files in the same directory as your input files.

I recommend running the trimmed reads through `FastQC` to compare them with the raw reads.

**Questions**<br>
**(i) How many reads does each of these fastq files contain?**<br>
**(ii) How long is each of the reads?**<br>
**(iii) How do these metrics differ from the raw reads?**<br>

### Conquering Taxonomic Classification with Kraken2

`Kraken2` is a tool used for the taxonomic classification of sequence reads. It uses k-mer-based approaches to match sequence reads against a reference database of known genomes, allowing it to quickly and accurately classify reads into taxonomic groups.

This section will cover the basics for using `Kraken2` to classify sequence reads.

The manual for `Kraken2` can be found here: [https://github.com/DerrickWood/kraken2/blob/master/docs/MANUAL.markdown](https://github.com/DerrickWood/kraken2/blob/master/docs/MANUAL.markdown)

Remember to cite the paper:

&emsp; Wood DE, Salzberg SL. Kraken: ultrafast metagenomic sequence classification using exact alignments. _Genome Biology_ 2014;15:R46 doi: [10.1186/gb-2014-15-3-r46](https://doi.org/10.1186/gb-2014-15-3-r46)

#### Classifying your reads

We can use `Kraken2` to classify our example sequences. To classify the sequences, we will use the following `Kraken2` command:

```bash
kraken2 --db /path/to/database/$DBNAME --threads ${CPUS} --memory-mapping --gzip-compressed --output kraken2_output --use-names --report ${SAMPLE}_kraken2_report --paired /path/to/trimmed_reads/${SAMPLE}_trimmed_1.fastq.gz /path/to/trimmed_reads/${SAMPLE}_trimmed_2.fastq.gz
```

Replace `${SAMPLE}` with the name of your sample. I.e., `Corella_11`

This will run `Kraken2` on the paired-end input sequences `${SAMPLE}_trimmed_1.fastq.gz` and `${SAMPLE}_trimmed_2.fastq.gz` using your designated `Kraken2` database. 

The `--output` flag specifies the output file where `Kraken2` will save the classified sequences. The `--report` flag specifies the report file where `Kraken2` will save a summary of the classification results. `Kraken2` can handle gzip compressed files as input by specifying `--gzip-compressed` flag. The `--threads` flag specifies the number of CPU threads to use for classification (parallel processing). The output will be written to a file called "Corella_11_kraken2_output" and a report file called "Corella_11_kraken2_report".

**Question**<br>
**(i) Based on your analysis, which bacterial species do the majority of the reads belong to?**<br>

## Why have I learnt this?

Tools like `FastQC`, `Trimmomatic`, and `Kraken2` are essential for analysing sequence data before downstream analysis. `FastQC` provides a detailed quality control report, identifying potential issues with data and allowing researchers to decide whether to perform additional quality control steps. `Trimmomatic` removes low-quality reads, adapter sequences, and contaminants to improve the accuracy of downstream analysis. `Kraken2` classifies reads based on their taxonomic origin, helping researchers understand the microbial community present in the sample and identify potential sources of error. Using these tools before downstream analysis can improve the quality and accuracy of the data, leading to more informed decisions and accurate results.

## Whakamihi! You did it!

That's it for this tutorial on sequence data quality control! You have investigated the quality of your sequence read data using `FastQC`, successfully used `Trimmomatic` to remove low-quality bases and Illumina adaptor sequences from your reads,  and employed `Kraken2` for the taxonomic classification of your DNA sequences. If you have any further questions, feel free to ask.

###### Final notes

<sup> This tutorial uses example paired-end short-read sequencing data from our phylogenomic study of *Chlamydia psittaci* sequence type (ST)24. I invite you to read our publication if you find this data intriguing: <sup>


<sub> &emsp; **White RT**, Anstey SI, Kasimov V, Jenkins C, Devlin J, El-Hage C, Pannekoek Y, Legione AR, Jelocnik M. One clone to rule them all: Culture-independent genomics of *Chlamydia psittaci* from equine and avian hosts in Australia. *Microbial Genomics* 2022;8:000888 doi: [https://doi.org/10.1099/mgen.0.000888](https://doi.org/10.1099/mgen.0.000888)<sub> 

###### Data availability

<sub> Sequence read data has been submitted to the National Center for Biotechnology Information (NCBI) Sequence Read Archive (SRA) under BioProject [PRJNA798154](https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA798154) <sub>

<sub> Raw Illumina sequence read data have been deposited to the NCBI SRA under the accession numbers [SRR17649252 to SRR17649264](https://www.ncbi.nlm.nih.gov/sra/?term=https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA798154) <sub>

<sub> In addition, the high-quality assembly has been deposited to the NCBI GenBank database under the accession numbers [JAKGCA000000000 to JAKGCM000000000](https://www.ncbi.nlm.nih.gov/sra/?term=https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA798154) <sub> 