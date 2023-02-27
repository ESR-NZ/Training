#### Author: Rhys White
#### Affiliations: Institute of Environmental Science and Research - ESR

# Kraken2

## Introduction

Kraken2 is a tool used for the taxonomic classification of sequence reads. It uses k-mer-based approaches to match sequence reads against a reference database of known genomes, allowing it to quickly and accurately classify reads into taxonomic groups.

This tutorial will cover the basics for installing Kraken2, building a reference database, and using it to classify sequence reads.

The manual for Kraken2 can be found here: [https://github.com/DerrickWood/kraken2/blob/master/docs/MANUAL.markdown](https://github.com/DerrickWood/kraken2/blob/master/docs/MANUAL.markdown)


## Installation

Before we begin, we must ensure that Kraken2 is installed on our system. If you don't have Kraken2 installed, you can download it from the official Kraken2 GitHub repository [https://github.com/DerrickWood/kraken2](https://github.com/DerrickWood/kraken2). To install it, open a terminal window and follow these steps:

```bash
# Download the Kraken2 source code from the official GitHub repository:
git clone https://github.com/DerrickWood/kraken2.git

# Navigate to the Kraken2 directory:
cd kraken2

# Build and install Kraken2:
./install_kraken2.sh /path/to/install
```

Note that you will need administrative privileges to install Kraken2 system-wide.


## Downloading a reference database

In addition to Kraken2, we must have a Kraken2 reference genome database. Kraken2 supports several databases, including RefSeq, GenBank, and custom databases. In this tutorial, we will use the MiniKraken2 database, which is a small subset of the full Kraken2 database. This is useful for testing and development purposes, as it requires fewer computational resources.

To download and install the MiniKraken2_v2_8GB database, follow these steps:

```bash
# Download the MiniKraken2 database from the official GitHub repository:
wget https://ccb.jhu.edu/software/kraken2/dl/minikraken2_v2_8GB.tgz

# Uncompress the database:
tar -xzf minikraken2_v2_8GB.tgz

# Set the database path as an environment variable:
export KRAKEN2_DB_PATH=/path/to/minikraken2_v2_8GB
```


## Build the downloaded Kraken2 reference database

Before we can use Kraken2 to classify our example sequences, we need to build the Kraken2 database. To build the database, we will use the `kraken2-build` command.

```bash
kraken2-build --standard --threads 8 --db $DBNAME
```

This will download NCBI taxonomic information, and the complete genomes in RefSeq for the bacterial, archaeal, and viral domains, along with the human genome and a collection of known vectors (UniVec_Core). Replace "$DBNAME" above with your preferred database name/location. Please note that the database will use approximately 100 GB of disk space during creation, most of which are reference sequences or taxonomy mapping information that can be removed after the build.

```bash
kraken2-build --download-taxonomy --threads 8 --db $DBNAME
kraken2-build --download-library bacteria --threads 8 --db $DBNAME
kraken2-build --build --threads 8 --db $DBNAME
```


## Classifying reads

Now that we have built the Kraken2 database, we can use Kraken2 to classify our example sequences. To classify the sequences, we will use the kraken2 command.

```bash
kraken2 --db /path/to/database/$DBNAME --threads 8 --memory-mapping --gzip-compressed --output kraken2_output --use-names --report kraken2_report --paired /path/to/test_data/paired-end/reads_1.fastq.gz /path/to/test_data/paired-end/reads_2.fastq.gz
```

This will run Kraken2 on the paired-end input sequences `reads_1.fastq.gz` and `reads_2.fastq.gz` using the Kraken2 database we downloaded earlier. The `--output` flag specifies the output file where Kraken2 will save the classified sequences. The `--report` flag specifies the report file where Kraken2 will save a summary of the classification results. Kraken 2 can handle gzip compressed files as input by specifying `--gzip-compressed` flag. The `--threads` flag specifies the number of CPU threads to use for classification (parallel processing). The output will be written to a file called "kraken2_output" and a report file called "kraken2_report".


## Conclusion

This tutorial covered the basics of using Kraken2 to classify sequence-read data. We built a Kraken2 database, classified example sequences, and analysed the results. With Kraken2, we can quickly and accurately classify large sequence datasets.
