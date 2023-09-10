Phylogenetics


#### Author: Rhys White ([@RhysTWhite](https://twitter.com/RhysTWhite))
#### Affiliations: Institute of Environmental Science and Research - ESR

# Advanced Genomic Insights Workshop Week 08:<br> A tutorial on phylogenetics

Table of contents
=================

<!--ts-->
   * [Prerequisites](#prerequisites)
   * [Introduction](#introduction)
   * [Let's get started...](#lets-get-started)
      * [Read _de novo_ assembly](#read-de-novo-assembly)
      * [_De novo_ assembly of nanopore sequence reads with Flye](#de-novo-assembly-of-nanopore-sequence-reads-with-flye)
      * [Polishing an assembly generated from nanopore reads using racon](#polishing-an-assembly-generated-from-nanopore-reads-using-racon)
      * [Polishing an assembly generated from nanopore reads using medaka](#polishing-an-assembly-generated-from-nanopore-reads-using-medaka)
   * [Why have I learnt this?](#why-have-i-learnt-this)
   * [Final notes](#final-notes)
   * [Data availability](#data-availability)
<!--te-->

## Prerequisites
To fully benefit from these exercises, it is essential that you manually _type_ each exercise. Please avoid the temptation to copy and paste, as these exercises aim to train your hands, brain, and mind in the fundamental skills of reading, writing, and comprehending code. By copying and pasting, you would be depriving yourself of the valuable learning experience these lessons provide.

Before we begin, you should have a basic understanding of genomics and the concept of DNA sequencing. You should also be familiar with the command line and have access to a Unix-based operating system (such as Linux or macOS).

## Introduction

Multi-Locus Sequence Typing (MLST) is a valuable technique for characterising bacterial strains based on the sequence variations in specific housekeeping genes. Whole Genome Sequencing (WGS) provides a wealth of genomic data, enabling the detection of antibiotic resistance genes and the construction of phylogenetic trees. This tutorial will guide you through performing MLST, antibiotic resistance gene detection, and basic phylogenetics using WGS data.

## Let's get started...

##### Getting the data
Note: The data is ~20 MB. To download the data, please run these commands:

```bash
# Download the four assembly (Illumina) files that we will be working with today:
wget https://raw.githubusercontent.com/ESR-NZ/Training/main/Advanced_Genomics_Workshop/Week_08/16AR1035_skesa.fasta
wget https://raw.githubusercontent.com/ESR-NZ/Training/main/Advanced_Genomics_Workshop/Week_08/16AR1047_skesa.fasta
wget https://raw.githubusercontent.com/ESR-NZ/Training/main/Advanced_Genomics_Workshop/Week_08/16AR1138_skesa.fasta
wget https://raw.githubusercontent.com/ESR-NZ/Training/main/Advanced_Genomics_Workshop/Week_08/16AR1246_skesa.fasta
```

```bash
# Download the fasta alignment file containing 7 concatenated MLST genes:
wget https://raw.githubusercontent.com/ESR-NZ/Training/main/Advanced_Genomics_Workshop/Week_08/MLST_alignment.fasta
```

You have been given five `fasta` files. Among these files, those beginning with the prefix `16AR1` represent draft genome assemblies, each containing contiguous sequences (contigs). Additionally, there is a file named `MLST_alignment.fasta`, which is an alignment file comprising seven concatenated genes used for MLST.

To begin, create a directory called `assemblies` and move the four assembly files into it. Afterwards, navigate to the `assemblies` folder:

```bash
# Make a directory called assemblies:
mkdir assemblies
```

```bash
# Move all files ending starting with 16AR1 into the assemblies directory:
mv /path/to/16AR1* assemblies/.
```

```bash
# Change directory to your assemblies directory
cd assemblies
```

To confirm that all four files are present in your current working directory, check for their existence using the following command:

```bash
# Directory listing
ls
```















