# Spring School Bioinformatics and Population Genomics - Leukerbad 29 May - 2 June 2016
---------------------------------------

# Practical: RNA-seq analysis for population genomics

Julien Roux

## Schedule
* Wednesday 1 June, 16:45 to 17:45: from raw sequencing data to gene-level read counts.
* Thursday 2 June, 13:45 to 15:30: clustering and differential expression analysis.

## Introduction

The aim of this practical is to introduce you to the recent, efficient and accurate tools to perform gene expression analysis for population genomics studies. RNA-seq performed on the Illumina platform is now a mature technology (first papers published in 2008), but there are still hurdles for its analysis. Mapping is long, it generates large BAM files to are incovenient to manipulate, reads mapping to multiple location are often just discarded, gene coverage is inequal due to biases during library preparation steps, etc. There have been a few recent methodological developments that are real game-changers for the analysis and interpretation of RNA-seq data and that we will introduce in this practical. 

For the analyses of this practical, we will make use of data stored in the `~/data/rnaseq/` folder in the virtual machine. When you download or generate data by yourself, it will be convenient to add them to this folder too.

## The biological question and the experiment

We will be reanalyzing RNA-seq data generated in the lab of Bart Deplancke, published last year in the following paper: 

Bou Sleiman MS, Osman D, Massouras A, Hoffmann AA, Lemaitre B and Deplancke B. Genetic, molecular and physiological basis of variation in Drosophila gut immunocompetence. Nature Communications. 2015;6:7829. <http://www.nature.com/ncomms/2015/150727/ncomms8829/full/ncomms8829.html>
A PDF of the paper and the supplementary data are located in the `~/data/papers/` folder (`ncomms8829*` files). 

Bart introduced very nicely the motivations of this study during his talk on Tuesday. Briefly, they aimed at studying how genetic variation in *Drosophila melanogaster* impacts the molecular and cellular processes that constitute gut immunocompetence. They performed RNA-seq on 16 gut samples comprising four susceptible and four resistant DGRP lines in the unchallenged condition and 4h after *Pseudomonas entomophila* infection. We are thus faced with an experimental design with three factors: DGRP lines, infection susceptibility and infection status. For simplicity, we will ignore the DGRP line, and consider the four susceptibility and the four resistant lines as biological replicates.

## The data you will need for mapping

### RNA-seq reads
The RNA-seq data are deposited on the GEO database at the following link: <http://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE59411>. If your are not familiar wiht GEO, please have a look the experiment and samples webpages. In particular, these include links to the raw sequencing data, the processed sequencing data in form of log2(RPKM) values for each gene in each sample (but this is not compulsory for submission), and some metadata allowing to know what experimental conditions the samples correspond to, the protocols used, etc. The raw data are downloadable from the FTP of the SRA database in the `.sra` format that you need to convert to `.fastq` format using the SRA toolkit, which is quite long.

![Tip](elemental-tip.png)
Tip: All GEO experiments are also mirrored in european equivalent, the ENA database. There the raw data are available directly in `.fastq` format. This can save you a lot of time!

For this practical, the `.fastq` files were previously downloaded on <http://www.ebi.ac.uk/ena/data/view/SRP044339> and added to your VM data folder (`SRR*.fastq.gz files`).

Please have a look at the first lines of one of these file:
```sh
less ~/data/rnaseq/SRR1515104.fastq.gz
```
![Question](round-help-button.png)
Identify the lines corresponding to one read, and identify the role of each line. This wikipedia article is useful: <https://en.wikipedia.org/wiki/FASTQ_format>

It is essential to verify that the quality of the reads you will analyze is good. The `FastQC` tool is widely used for this purpose. As it takes some time to run, each `.fastq` file was processed in advance. The fastQC results can be found in the `~/data/rnaseq/FASTQC` folder. 
![Question](round-help-button.png)
Open a few `.html` results files from FastQC and make sure you understand the controls performed.

### A reference genome and its annotation
![Question](round-help-button.png)
Download the *D. melanogaster* reference genome from the database Ensembl: <http://www.ensembl.org/index.html>. To be sure to understand which version is needed (repeat-masked, soft-masked, toplevel, etc), it is a good practice to look at the `README.txt` files located in folders of the Ensembl FTP.

![Question](round-help-button.png)
Download also the *D. melanogaster* annotation in `GTF` format from Ensembl

### A transcriptome index for Kallisto pseudo-mapping
We will assign reads to transcript using the tool `Kallisto`. The online documentation is available at <https://pachterlab.github.io/kallisto/manual.html>. 

Using the GTF and genome files, we first need to create a fasta file including the sequences of all annotated transcripts. This is done using the `gffread` utility part of the `Cufflinks` package:
```sh
gunzip ~/data/rnaseq/Drosophila_melanogaster.BDGP6.84.gtf.gz
gunzip ~/data/rnaseq/Drosophila_melanogaster.BDGP6.dna_sm.toplevel.fa.gz
gffread ~/data/rnaseq/Drosophila_melanogaster.BDGP6.84.gtf -g ~/data/rnaseq/Drosophila_melanogaster.BDGP6.dna_sm.toplevel.fa -w ~/data/rnaseq/Drosophila_melanogaster.BDGP6.transcriptome.fa
```
We can then launch the creation of the Kallisto index:
```sh
kallisto index -i ~/data/rnaseq/Drosophila_melanogaster.BDGP6.transcriptome.idx ~/data/rnaseq/Drosophila_melanogaster.BDGP6.transcriptome.fa

![Question](round-help-button.png)
Is the default kmer-size appropriate? In which case would it be useful to reduce it?

## "Mapping" the data


##################################
* TO DO: add to github
* TO DO: prepare short presentation of: 
  * kallisto. Fast + accurate + need deal
  * DTU/DE/DTE. DE confounded by DTU
  * limma-voom on TPM, etc
  * pbs: missing genes? missing isoforms? should be better to get better annotation first usign RNA-seq dataset (cufflinks, trinity)

![Question](round-help-button.png)
![Tip](elemental-tip.png)