#### Author: Rhys White ([@RhysTWhite](https://twitter.com/RhysTWhite))
#### Affiliations: Institute of Environmental Science and Research - ESR

# Rhyscover the Secrets of Evolutionary Trees: a tutorial on phylogenetics

Table of contents
=================

<!--ts-->
   * [Prerequisites](#prerequisites)
   * [Introduction](#introduction)
   * [Let's get started...](#lets-get-started)
      * [Read _De Novo_ Assembly](#read-de-novo-assembly)
      * [_De novo_ assembly of Illumina sequence reads with Velvet](#de-novo-assembly-of-illumina-sequence-reads-with-velvet)
      * [_De novo_ assembly of Illumina sequence reads with SPAdes](#de-novo-assembly-of-illumina-sequence-reads-with-spades)
      * [Analyse the draft assembly metrics](#analyse-the-draft-assembly-metrics)      
      * [Conquering Taxonomic Classification with Kraken2](#conquering-taxonomic-classification-with-kraken2)  
   * [Why have I learnt this?](#why-have-i-learnt-this)
   * [Final notes](#final-notes)
   * [Data availability](#data-availability)
<!--te-->

## Prerequisites
To fully benefit from these exercises, it is essential that you manually _type_ each exercises. Please avoid the temptation to copy and paste, as the purpose behind these exercises is to train your hands, brain, and mind in the fundamental skills of reading, writing, and comprehending code. By copying and pasting, you would be depriving yourself of the valuable learning experience these lessons provide.

Before we begin, you should have a basic understanding of genomics and the concept of DNA sequencing. You should also be familiar with the command line and have access to a Unix-based operating system (such as Linux or macOS).

## Introduction

Welcome to the world of phylogenetics, where we explore the intricate relationships and evolutionary history of organisms. Phylogenetics allows us to reconstruct the Tree of Life, providing insights into how species are connected and evolve. In this tutorial, we will embark on an exciting journey to understand the principles, methods, and tools used in phylogenetics.

![Phylogeny_explained](https://github.com/ESR-NZ/Training/assets/28285670/03c8c84b-de28-4dab-88b5-d742c0940874)
**Figure. Phylogenetic relationships of _Chlamydia psittaci_ sequence type (ST)24 strains**. Image is adapted from [White et al. 2023](https://doi.org/10.1016/j.vetmic.2023.109704)

## Let's get started...

##### Getting the data

Note: The data is ~2 KB. To download the data, please run this command:

```bash
# Download the FASTA file containing sequence data for cytochrome b:
wget https://github.com/ESR-NZ/Training/blob/main/Rhys_Toolbox/03_Phylogenetics/CYTB.fasta
```

To begin, create a directory called `phylogenetics` and move the `CYTB.fasta` file into it. Afterwards, navigate to the `phylogenetics` folder:


```bash
# Make a directory called quality_control:
mkdir phylogenetics
```

```bash
# Move the `CYTB.fasta` file into the phylogenetics directory:
mv /path/to/CYTB.fasta phylogenetics/.
```

```bash
# Change directory to your phylogenetics directory
cd phylogenetics
```

To confirm that both files are present in your current working directory, check for their existence using the following command:

```bash
# Directory listing
ls
```

**The sequence data for an unknown cytochrome b (hint: class Mammalia and order Carnivora) is what we will be using today.**

### Identification and retrieval of closest homologues: cytochrome b loci analysis
In this tutorial, we will learn how to use the CytB loci to identify and download the 20 closest homologues with **full** mitochondrial sequences to a unknown species. We will then construct a table that includes relevant information such as GenBank IDs, species names, and the level of homology.

##### Step 1: Searching for homologous sequences to our query
- Access the National Center for Biotechnology Information (NCBI) Standard Nucleotide BLAST database, which can be found [here](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastn&BLAST_SPEC=GeoBlast&PAGE_TYPE=BlastSearch)
- Upload your unknown Mammalia CytB sequence as the query sequence for a BLASTn search 
- In this particular case, we will refrain from conducting a search within the conventional nucleotide collection (nr/nt). Instead, our focus will be on searching the RefSeq Genome database (refseq_genomes)
- Set the search parameters to focus on the class Mammalia (taxid: 40674) and order Carnivora (taxid: 33554)
- Performing a somewhat similar sequences (BLASTn) search
- Retrieve the 20 closest homologues with **full** mitochondrial sequences to your unknown Mammalia CytB sequence
- Generate and complete a table, like below:

|  | GenBank Accession Number | Nucleotide Sequence Homology (%) | Total Score | Query Coverage (%) |
| :---         |          ---: |          ---: |          ---: |          ---: |
| Species 1 |           |           |           |           |
| Species 2 |           |           |           |           |
| Species 3 |           |           |           |           |
| Species 4 |           |           |           |           |
| Species 5 |           |           |           |           |
| and so on... |           |           |           |           |

- Don't forget to download the FASTA (aligned sequences) for your 20 closest homologues

### Read _De Novo_ Assembly

The next step is assembling the high-quality reads into contigs using a _de novo_ assembler. Many _de novo_ assemblers are available, but today we will focus on Velvet ([Zerbino et al. 2008](https://doi.org/10.1101/gr.074492.107)) and SPAdes ([Bankevich et al. 2012](https://doi.org/10.1089/cmb.2012.0021)). These assemblers use different algorithms and heuristics to assemble the reads, so it is essential to test and compare the performance of multiple assemblers on your specific dataset. We will evaluate how varying k-mer lengths can impact the assembly process.

Remember to cite the papers:

&emsp; Bankevich A, Nurk S, Antipov D, Gurevich AA, Dvorkin M, Kulikov AS, _et al_. SPAdes: a new genome assembly algorithm and its applications to single-cell sequencing. _Journal of Computational Biology_ 2012;19:455-477 doi: [10.1089/cmb.2012.0021](https://doi.org/10.1089/cmb.2012.0021) 

&emsp; Zerbino DR, Birney E. Velvet: algorithms for _de novo_ short read assembly using de Bruijn graphs. _Genome Research_ 2008;18:821-829 doi: [10.1101/gr.074492.107](https://doi.org/10.1101/gr.074492.107)

### _De novo_ assembly of Illumina sequence reads with Velvet

`velvet` operates in two fundamental stages:

(a) `velveth`: This step involves the intake of sequence data, storing all k-mers, and generating a hash table.
- Input: sequence reads in various formats such as fasta, fastq, fasta.gz, fastq.gz, sam, bam, etc.
- Output: Sequences and Roadmaps.

(b) `velvetg`: Here, de Bruijn graphs are constructed and manipulated to assemble the data into contigs.
- Input: Sequences and Roadmaps (from `velveth`)
- Output: An output directory containing contigs in FASTA format, as well as statistics and log files.

You can find the `velvet` manual by following this link: [https://github.com/dzerbino/velvet/blob/master/Manual.pdf](https://github.com/dzerbino/velvet/blob/master/Manual.pdf).

##### velveth

To start the _de novo_ assembly process, use the `velvet` command-line tool `velveth` to build the hash table. The syntax of the command is as follows:

```
# Run velveth:
velveth <output_directory> <k-mer_length> -fastq -shortPaired ${SAMPLE}_trimmed_1.fastq.gz ${SAMPLE}_trimmed_2.fastq.gz
```

In this command, replace `<output_directory>` with the name of the directory where you want to save the output files, `<k-mer_length>` with the desired k-mer length for assembly, and `${SAMPLE}_1.fastq.gz` and `${SAMPLE}_2.fastq.gz` with the names of your FASTQ input files.

For example, to create a hash table with a k-mer length of 31 for Illumina paired-end reads `Corella_11_Sub_trimmed_1.fastq.gz` and `Corella_11_Sub_trimmed_2.fastq.gz` and save the output files in a directory named "velvet31_output", you can use the following command:

```
# Run velveth:
velveth velvet31_output 31 -fastq -shortPaired Corella_11_Sub_trimmed_1.fastq.gz Corella_11_Sub_trimmed_2.fastq.gz
```

The selection of the k-mer length to use is specified while running velveth. 
<Rhys To-do: guidance on choosing the appropriate k-mer length>

##### velvetg

Next, construct the contigs using the `velvet` command-line tool `velvetg`. The syntax of the command is as follows:

```
# Run velvetg:
velvetg <output_directory> -min_contig_lgth <min_contig_length>
```

In this command, replace `<output_directory>` with the name of the directory where the hash table output files were saved, and `<min_contig_length>` with the desired minimum length for the contigs.

For example, to construct contigs with a minimum length of 100 bp from the hash table output files in the `velvet31_output` directory, you can use the following command:

```
# Run velvetg:
velvetg velvet31_output -min_contig_lgth 100 -ins_length 300 -exp_cov 40
```

Now, we are going to perform sequence assembly with `velvet` using varying k-mer lengths of 59, 69, 79, and 109.

### Questions

**After generating the `contigs.fa`, `stats.txt`, and Log files, extract the following information about the assembly:**<br>
**(a) N50 length**<br>
**(b) maximum contig length**<br>
**(c) total length of all contigs**<br>
**(d) percentage of reads assembled into contigs**<br>
**(e) number of contigs in the contigs.fa file**<br>
**(f) maximum average coverage for a contig**<br>
**(g) average coverage across all contigs**<br>
**If any of the results appear unusual, consider why this might be the case.**<br>

Record the following information from the Log file and complete the table below:
  
  
  
|  | _k_ = 59 | _k_ = 69 | _k_ = 79 | _k_ = 109 |
| :---         |          ---: |          ---: |          ---: |          ---: |
| N50 |           |           |           |           |
| Max. contig length |           |           |           |           |
| Total contig lengths |           |           |           |           |
| % of reads that are assembled |           |           |           |           |

**Using the table you created, which k-mer length resulted in the following:**<br>
**(a) a shorter N50**<br>
**(b) a longer N50**<br>
**(c) a longer total contig length**<br>
**(d) a lower percentage of assembled reads**<br>

**In your opinion, which of the two k-mer lengths produced a superior assembly based on your observations?**<br>

**Using the `contigs.fa` and `stats.txt` files from your best assembly, retrieve the following information:**<br>
**(a) total number of contigs in `contigs.fa`**<br>
**(b) maximum average coverage for a contig**<br>
**(c) total length of all contigs (an estimation of genome size)**<br>
**(d) average coverage across all contigs (an estimation of overall genome coverage)**<br>

The `contigs.fa` file from your best assembly will be utilised for other in upcoming tutorials. This file represents your draft genome assembly and can be renamed to a more relevant name, like `Corella_11_velvet_contigs.fasta`, and stored in a separate folder, if preferred. You can delete all other output directories containing different assemblies and maintain the directory of your best assembly. It is a beneficial habit to regularly remove any unnecessary files.

In conclusion, `velvet` is a powerful tool for _de novo_ assembly of Illumina sequence reads. By following the above steps, you can use `velvet` to assemble your sequence data and obtain a high-quality assembly.
  
### _De novo_ assembly of Illumina sequence reads with SPAdes

You can find the `spades` manual by following this link: [https://github.com/ablab/spades](https://github.com/ablab/spades).

To run `spades`, you need to specify the input files and parameters. The basic command to run `spades` is:

```
spades.py -o output_dir -1 reads_R1.fastq -2 reads_R2.fastq
```

In this command, `-o` specifies the output directory, `-1` and `-2` specify the paired-end reads, and `spades.py` is the SPAdes binary file. You can also specify additional parameters, such as the k-mer size (`-k`), the coverage cutoff for error correction (`--cov-cutoff`), and the number of threads (`-t`).

Now, we're going to run `spades` using our trimmed reads:

```
spades.py -t 4 -o output_dir -1 Corella_11_Sub_trimmed_1.fastq.gz -2 Corella_11_Sub_trimmed_2.fastq.gz -o Corella_11_Sub_spades_output
```

`spades` is another powerful tool for _de novo_ genome assembly. By following the above steps, you can use `spades` to assemble your sequence data and obtain a high-quality assembly.

## Analyse the draft assembly metrics

We can analyse the `velvet` and `spades` output using tools such as QUAST (QUality ASsessment Tool) to evaluate the genome assembly quality.

The manual for QUAST can be found here: [https://github.com/ablab/quast](https://github.com/ablab/quast)

QUAST is a popular software tool used to assess the quality of genome assemblies. It can produce various valuable metrics, including N50, L50, and other statistics that can help you determine the quality of your assembly.
  
To run QUAST, we will use the `quast.py` command.

```bash
quast.py /path/to/assembly/${SAMPLE}.fasta -o quast_${SAMPLE}_output
```

Replace `${SAMPLE}.fasta` with the names of your input files. This will generate a report summarising the quality of the genome assembly.

### Question
In your opinion, which of the two assembler tools (i.e., `velvet` and `spades`) produced a superior assembly based on your observations?**<br>
  
## Rhys To-do: CheckM
  
  
  ## Why have I learnt this?

You have learned to use `velvet` and `spades` as they are popular and widely used _de novo_ genome assembly tools that handle short-read sequence data. They provide different strategies for contig assembly, and it is helpful to be familiar with multiple assembly tools to compare the quality of assemblies generated by each.

`quast` and `checkm` are commonly used tools for assessing the quality of genome assemblies. `quast` allows you to compare the accuracy and completeness of different assemblies, while `checkm` assesses the completeness and contamination of genome assemblies by comparing the assembled contigs to a set of reference marker genes. These tools help ensure that the genome assembly is high quality and can be used for downstream analyses.

## Whakamihi! You did it!

That's it for this tutorial on _de novo_ genome assembly using short-read sequence data! You have assembled your short-read sequence data into a draft genome using `velvet` and `spades`, and have successfully used `quast` and `checkm` to assess the quality metrics of your genome assemblies. If you have any further questions, feel free to ask.
  
  ###### Final notes

<sup> This tutorial uses example paired-end short-read sequencing data from our phylogenomic study of *Chlamydia psittaci* sequence type (ST)24. I invite you to read our publication if you find this data intriguing: <sup>


<sub> &emsp; **White RT**, Anstey SI, Kasimov V, Jenkins C, Devlin J, El-Hage C, Pannekoek Y, Legione AR, Jelocnik M. One clone to rule them all: Culture-independent genomics of *Chlamydia psittaci* from equine and avian hosts in Australia. *Microbial Genomics* 2022;8:000888 doi: [https://doi.org/10.1099/mgen.0.000888](https://doi.org/10.1099/mgen.0.000888)<sub> 

###### Data availability

<sub> Sequence read data has been submitted to the National Center for Biotechnology Information (NCBI) Sequence Read Archive (SRA) under BioProject [PRJNA798154](https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA798154) <sub>

<sub> Raw Illumina sequence read data have been deposited to the NCBI SRA under the accession numbers [SRR17649252 to SRR17649264](https://www.ncbi.nlm.nih.gov/sra/?term=https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA798154) <sub>

<sub> In addition, the high-quality assembly has been deposited to the NCBI GenBank database under the accession numbers [JAKGCA000000000 to JAKGCM000000000](https://www.ncbi.nlm.nih.gov/sra/?term=https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA798154) <sub> 
