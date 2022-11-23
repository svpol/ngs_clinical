# Overview

This repo presents a bioinformatic pipeline for NGS data clinical analysis. It starts with a FASTQ file and ends with SNPs annotation. Performed with command-line interpreter tools.

For now, the example pipeline is presented for a single-end read only.

**Disclaimer**

* It is supposed that reference sequences are taken from hg38.
* This is just an example, any other suitable software can be used for these purposes.

# Repo structure

* singe_end_pipeline.md - the pipeline for NGS data analysis.
* sample_data directory:

  * sample.fastq - the sequence to analyse, reads from human chromosome 22
  * reference_seq.txt - the link to reference human chromosome of hg38.
  * conclusion.txt - medical information about the resuls of the sample sequence analysis.
