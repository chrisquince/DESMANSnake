# DESMANSnake

## Overview

STRONG resolves strain on assembly graphs by resolving variants on core COGs using co-occurrence across multiple samples.

## Installation

Clone repo:

```
git clone https://github.com/chrisquince/DESMANSnake.git
```

## Requirements

This Snakmake requires the following to be installed:

1. [snakemake](https://snakemake.readthedocs.io/en/stable/getting_started/installation.html): 
    


2. [bwa](https://github.com/lh3/bwa): Necessary for mapping reads onto contigs

    ```
    cd ~/repos
    git clone https://github.com/lh3/bwa.git
    cd bwa; make
    cp bwa ~/bin
    ```

3. [bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml): Used to get per sample base frequencies at each position


4. [samtools](http://www.htslib.org/download/): Utilities for processing mapped files. The version available through apt will *NOT* work instead...

    ```
    cd ~/repos
    wget https://github.com/samtools/samtools/releases/download/1.3.1/samtools-1.3.1.tar.bz2
    tar xvfj samtools-1.3.1.tar.bz2 
    cd samtools-1.3.1/ 
    sudo apt-get install libcurl4-openssl-dev libssl-dev
    ./configure --enable-plugins --enable-libcurl --with-plugin-path=$PWD/htslib-1.3.1
    make all plugins-htslib
    cp samtools ~/bin/  
    ```

5. [bedtools](http://bedtools.readthedocs.io/en/latest/): Utilities for working with read mappings

    ```
    sudo apt-get install bedtools
    ```

6. [prodigal](https://github.com/hyattpd/prodigal/releases/): Used for calling genes on contigs

    ```
    wget https://github.com/hyattpd/Prodigal/releases/download/v2.6.3/prodigal.linux 
    cp prodigal.linux ~/bin/prodigal
    chmod +rwx ~/bin/prodigal
    ```
    
7. [DESMAN](https://github.com/chrisquince/DESMAN.git): Needs to be installed in the path and scripts available at location ***dscripts*** in config file (see below).

8. [rpsblast](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download): Needs to be from BLAST 2.5.0+

9. Download the COG RPSBlast database:
```
wget https://desmantutorial.s3.climb.ac.uk/rpsblast_cog_db.tar.gz
tar -xvzf rpsblast_cog_db.tar.gz
```
Then set the location as ***cog_database*** in config.yaml.


## Structuring input

A data directory myData needs to created with N subdirectories labelled:


sample1,...,samplen,..., sampleN


One for each sample. In those forward and reverse reads must be present named:

samplen_R1.fastq.gz and samplen_R2.fastq.gz 

where n is the index of that directory.

The final folder in this directory should be called:

binning

This can contain any number of MAGs as subdirectories named:

Bin_n

Where n is any integer and within that there is a single fasta of contigs named:

Bin_n.fasta


## Example Folder

An example data folder is available here:

```
wget https://desmantutorial.s3.climb.ac.uk/DesmanExample.tar.gz
tar -cvzf DesmanExample.tar.gz
```

## Quick start

Run from within the DESMANSnake directory. Using the following command:

```
python3 ./start.py --config config.yaml output_dir --threads 32
```

To generate results in output_dir.

Optionally pass snakemake parameters e.g. '--dryrun'

## Config file

```
# ------ Resources ------ 
threads : 8 # single task nb threads
# ------ Assembly parameters ------ 
data: /pathTo/myData  # path to data folder
groups: ['*'] # specify a group of samples to resolve strains from this indicates one group using all samples
# ---- Annotation database -----
cog_database: /pathTo/rpsblast_cog_db/Cog # COG database 
# ---- Desman parameters ------
desman:
    dscripts: /pathTo/repos/DESMAN/scripts
    execution: 1
    nb_haplotypes: 10
    nb_repeat: 5
    min_cov: 1
```

## Output

Desman results in desman folder



