#### Author: Rhys White ([@RhysTWhite](https://twitter.com/RhysTWhite))
#### Affiliations: Institute of Environmental Science and Research - ESR

# Advanced Genomic Insights Workshop Week 07:<br> A tutorial on assembling a genome using long-read sequence data

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
To fully benefit from these exercises, it is essential that you manually _type_ each exercise. Please avoid the temptation to copy and paste, as these exercises aim to train your hands, brain, and mind in the fundamental skills of reading, writing, and comprehending code. By copying and pasting, you would be depriving yourself of the valuable learning experience these lessons provide.

Before we begin, you should have a basic understanding of genomics and the concept of DNA sequencing. You should also be familiar with the command line and have access to a Unix-based operating system (such as Linux or macOS).

Prior completion of the week 05 sequence data [quality control tutorial](https://github.com/ESR-NZ/Training/tree/main/Advanced_Genomics_Workshop/Week_05) is needed. We will analyse the same sequence data in that tutorial for time efficiency.








ONT sequence data Assembly


Notes
medaka_consensus -d .fasta -i ONT.fastq.gz -o medaka_test
