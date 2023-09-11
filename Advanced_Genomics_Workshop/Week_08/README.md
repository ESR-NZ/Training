Phylogenetics


#### Author: Rhys White ([@RhysTWhite](https://twitter.com/RhysTWhite))
#### Affiliations: Institute of Environmental Science and Research - ESR

# Advanced Genomic Insights Workshop Week 08:<br> A tutorial on Multi-Locus Sequence Typing and Screeening Genome Assemblies for Antibiotic Resistance Genes

Table of contents
=================

<!--ts-->
   * [Prerequisites](#prerequisites)
   * [Introduction](#introduction)
   * [Let's get started...](#lets-get-started)
      * [Identifying MLST profiles in draft _E. coli_ assemblies using the tool mlst](#identifying-mlst-profiles-in-draft-e-coli-assemblies-using-the-tool-mlst)
      * [Identifying antibiotic resistance profiles in draft _E. coli_ assemblies using the tool abricate](#identifying-antibiotic-resistance-profiles-in-draft-E-coli-assemblies-using-the-tool-abricate)
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

You have been given five `fasta` files. Among these files, those beginning with the prefix `16AR1` represent draft genome assemblies, each containing contiguous sequences (contigs). 

ESR contributes to the national public health surveillance of antibiotic resistance among human pathogens ([https://www.esr.cri.nz/our-research/nga-kete/infectious-disease-intelligence/antimicrobial-resistance-amr/](https://www.esr.cri.nz/our-research/nga-kete/infectious-disease-intelligence/antimicrobial-resistance-amr/)) in Aotearoa New Zealand. Until 2005, diagnostic laboratories referred all ESBL-producing Enterobacterales (ESBL-E) isolates to ESR for confirmation. This continuous surveillance was discontinued in August 2005 and replaced with annual 31-day surveys, typically in August (available at [https://www.esr.cri.nz/our-research/nga-kete/infectious-disease-intelligence/antimicrobial-resistance-amr/](https://www.esr.cri.nz/our-research/nga-kete/infectious-disease-intelligence/antimicrobial-resistance-amr/)).

These four draft genome assemblies come from the 2016 survey:<br>

&emsp; **Heffernan H, Woodhouse R, Draper J, Ren X**. 2016 survey of extended-spectrum β-lactamase-producing Enterobacteriaceae. Porirua, New Zealand: Antimicrobial Reference Laboratory and Health Group, Institute of Environmental Science and Research Limited; 2018. Available from: [https://surv.esr.cri.nz/antimicrobial/esbl.php](https://surv.esr.cri.nz/antimicrobial/esbl.php) [Accessed 09 July 2021]

Additionally, there is a file named `MLST_alignment.fasta`, which is an alignment file comprising seven concatenated genes used for MLST.

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

### Identifying MLST profiles in draft _E. coli_ assemblies using the tool mlst

MLST is a powerful method for characterising bacterial strains based on the sequence variations in specific housekeeping genes. Torsten Seemann's tool named `mlst` simplifies identifying MLST profiles in bacterial genome assemblies.

You can find the `mlst` manual by following this link: [https://github.com/tseemann/mlst](https://github.com/tseemann/mlst)

#### Running mlst

Once you are in the directory containing your `fasta` files, you can run `mlst` on the command line using the following syntax:

```bash
mlst [options] ${SAMPLE}.fasta 
````

But! before you do, there are a couple of things we need to consider.
First, replace `${SAMPLE}.fasta` with the name of your `fasta` file.

Secondly, you can force a particular scheme (useful for reporting systems). Some standard options you may want to include are:

`--scheme` to designate your preferred MLST scheme.<br>

Third, if you are unsure about what schemes are available, you can generate a long list of MLST profiles available in this tool. To do this, you can include the `--longlist` option like this:

##### Available schemes
To see which PubMLST schemes are supported you can use the `--list` option like this:
```bash
mlst --list
```

```bash
# Output from mlst --list
abaumannii achromobacter aeromonas afumigatus cdifficile efaecium
hcinaedi hparasuis hpylori kpneumoniae leptospira
saureus xfastidiosa yersinia ypseudotuberculosis yruckeri
```

The command `mlst --longlist` is used to generate a comprehensive and detailed list of MLST profiles for different bacterial species or strains. This list includes information about the MLST schemes and the corresponding housekeeping genes used for each scheme.

The above list is shortened. You can get more details using mlst --longlist.

```bash
mlst --longlist
````

```bash
# Output from mlst --longlist
achromobacter     nusA       rpoB      eno       gltB      lepA       nuoL      nrdA
abaumannii        Oxf_gltA   Oxf_gyrB  Oxf_gdhB  Oxf_recA  Oxf_cpn60  Oxf_gpi   Oxf_rpoD
abaumannii_2      Pas_cpn60  Pas_fusA  Pas_gltA  Pas_pyrG  Pas_recA   Pas_rplB  Pas_rpoB
aeromonas         gyrB       groL      gltA      metG      ppsA       recA
aphagocytophilum  pheS       glyA      fumC      mdh       sucA       dnaN      atpA
arcobacter        aspA       atpA      glnA      gltA      glyA       pgm       tkt
afumigatus        ANX4       BGT1      CAT1      LIP       MAT1_2     SODB      ZRF2
bcereus           glp        gmk       ilv       pta       pur        pyc       tpi
```

There are two MLST schemes for _E. coli_ available: 
- The Achtman scheme, which is based on seven housekeeping genes (_adk_, _fumC_, _gyrB_, _icd_, _mdh_, _purA_, and _recA_) ([Wirth et al., 2006](www.doi.org/10.1111/j.1365-2958.2006.05172.x))
- The Pasteur Institute (Paris, France) scheme, which is based on eight housekeeping genes (_dinB_, _icdA_, _pabB_, _polB_, _putP_, _trpA_, _trpB_, and _uidA_) (Jaureguy et al., 2009](www.doi.org/10.1186/1471-2164-9-560))

**Both E. coli MLST schemes with allelic profiles are hosted on PubMLST ([Larsen et al., 2012](www.doi.org/10.1128/JCM.06094-11).**

Today, we will be using the Achtman scheme. Have a go at identifying the MLST profile for the 16AR1035 genome:

```bash
mlst --scheme ecoli_4 16AR1035_skesa.fasta >> 16AR1035_MLST.tsv
```

Each sample's MLST profile is displayed, including the sequence type (ST) and allele numbers for each housekeeping gene.
You can identify the MLST profiles for your _E. coli_ draft assemblies based on these results.

**Question**<br>
**(i) Based on your analysis, which MLST profile does each of the 2016 ESBL-producing _E. coli_ genomes belong to?**<br>

### Identifying antibiotic resistance profiles in draft _E. coli_ assemblies using the tool abricate

Next, I'll walk you through how to use a tool called `Abricate` and an antibiotic resistane gene database called `ResFinder` to identify antibiotic resistance genes in bacterial genomes.

#### Running abricate

To search for antibiotic resistance genes, use the following command:

```bash
abricate --db resfinder --minid 90 --mincov 60 ${SAMPLE}.fasta
```

Replace `${SAMPLE}.fasta` with the path and name of your bacterial genome in `fasta` format.

Abricate will generate a report in tab-separated values (TSV) format.<br>
The report will list the antibiotic resistance genes found in the input genome, their start and end positions, percent identity, and other information.<br>
You can use tools like Excel, LibreOffice Calc, or Python libraries like Pandas to visualize and analyze the results for a more comprehensive understanding.<br>

For more advanced options and settings, refer to the Abricate documentation: [https://github.com/tseemann/abricate](https://github.com/tseemann/abricate)

If you use ResFinder in your research, remember to cite the paper:

&emsp; Zankari E, Hasman H, Cosentino S, Vestergaard M, Rasmussen S, Lund O, Aarestrup FM, Larsen MV. Identification of acquired antimicrobial resistance genes. _Journal of Antimicrobial Chemotherapy_ 2012;67:2640-2644 doi: [https://doi.org/10.1093/jac/dks261](https://doi.org/10.1093/jac/dks261)

**Question**<br>
**(i) Based on your analysis, which antibiotic resistance genes does each of the 2016 ESBL-producing _E. coli_ genomes carry?**<br>


Rhys to do
phylogenetics



## Why have I learnt this?

-

## Whakamihi! You did it!
That's it for this tutorial on MLST and screeening genome assemblies for antibiotic resistance genes! You have investigated the MLST profile of assemblies using `mlst`, successfully used `abricate` to identify any antibiotic resistance genes present in your DNA sequences. If you have any further questions, feel free to ask.

###### Final notes

<sup> This tutorial uses example paired-end short-read sequencing data from our 2016 survey of extended-spectrum beta-lactamase-producing Enterobacterales. I invite you to read our report if you find this data intriguing: <sup>


<sub> &emsp; **Heffernan H, Woodhouse R, Draper J, Ren X**. 2016 survey of extended-spectrum β-lactamase-producing Enterobacteriaceae. Porirua, New Zealand: Antimicrobial Reference Laboratory and Health Group, Institute of Environmental Science and Research Limited; 2018. Available from: [https://www.esr.cri.nz/our-research/nga-kete/infectious-disease-intelligence/antimicrobial-resistance-amr/](https://www.esr.cri.nz/our-research/nga-kete/infectious-disease-intelligence/antimicrobial-resistance-amr/) [Accessed 09 July 2021]<sub> 

###### Data availability

<sub> Sequence read data has been submitted to the National Center for Biotechnology Information (NCBI) Sequence Read Archive (SRA) under BioProject [PRJNA531554](https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA531554) <sub>

<sub> Raw Illumina sequence read data have been deposited to the NCBI SRA under the accession numbers [SRR10286073, SRR10286105, SRR10286115, and SRR10286317](https://www.ncbi.nlm.nih.gov/sra/?term=https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA531554) <sub>
