# STRONG - Strain Resolution ON Graphs

## Overview

STRONG resolves strain on assembly graphs by resolving variants on core COGs using co-occurrence across multiple samples.

## Installation

Requires recursive cloning:

```
git clone https://github.com/chrisquince/DESMANSnake.git
```

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


## Quick start

Run from within the DESMANSnake directory. Using the following command:

```
python3 ./start.py --config config.yaml output_dir --threads 32
```

To generate results in output_dir.

Optionally pass snakemake parameters e.g. '--dryrun'

## Config file

```
# ------ Resssources ------ 
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

