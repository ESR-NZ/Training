#### Author: Rhys White
#### Affiliations: Institute of Environmental Science and Research - ESR

# Flye

## Introduction

Flye is a genome assembler that uses long reads to produce genome assemblies. 

This tutorial will cover the basics of using Flye to assemble a genome. We will use a set of example reads generated from a bacterial genome.

The manual for Flye can be found here: [https://github.com/fenderglass/Flye/blob/flye/docs/USAGE.md](https://github.com/fenderglass/Flye/blob/flye/docs/USAGE.md)

## Prerequisites

Before we begin, we must ensure that Flye is installed on our system. If you don't have Flye installed, you can download it from the official Flye GitHub repository. To install it, open a terminal window and follow these steps:

```bash
git clone https://github.com/fenderglass/Flye
cd Flye
python setup.py install
```

In addition to Flye, we must have a set of example reads. We can download example reads from the Flye GitHub repository [https://github.com/fenderglass/Flye/blob/flye/docs/USAGE.md](https://github.com/fenderglass/Flye/blob/flye/docs/USAGE.md). The dataset was originally released by the Loman lab:

```bash
# E. coli Oxford Nanopore Technologies data
wget https://zenodo.org/record/1172816/files/Loman_E.coli_MAP006-1_2D_50x.fasta
```


## Run Flye

Now that we have the example reads, we can run Flye to assemble the genome. To run Flye, we will use the flye command.

```bash
flye --nano-raw Loman_E.coli_MAP006-1_2D_50x.fasta --out-dir out_Flye_assembly --threads 4
```

This will assemble the genome using the example reads and write the output to the out_Flye_assembly directory.


## Analyse the Flye output

Finally, we can analyse the Flye output using tools such as QUAST (QUality ASsessment Tool) to evaluate the genome assembly quality.

The manual for QUAST can be found here: [https://github.com/ablab/quast](https://github.com/ablab/quast)

QUAST is a popular software tool used to assess the quality of genome assemblies. It can produce various valuable metrics, including N50, L50, and other statistics that can help you determine the quality of your assembly. Before we begin, we must ensure that QUAST is installed on our system. If you don't have QUAST installed, you can download it from the official QUAST GitHub repository [https://github.com/ablab/quast](https://github.com/ablab/quast). To install it, open a terminal window and follow these steps:

```bash
# Download the QUAST source code from the official GitHub repository:
git clone https://github.com/ablab/quast.git
```

To run QUAST, we will use the `quast.py` command.

```bash
quast.py out_Flye_assembly/assembly.fasta -o quast_output
```

This will generate a report summarising the quality of the genome assembly.


## Conclusion

In this tutorial, we covered the basics of using Flye to assemble a genome. We prepared example reads, ran Flye to assemble the genome, and analysed the output using tools such as QUAST. With Flye, we can generate de novo genome assemblies using long read sequence data.
