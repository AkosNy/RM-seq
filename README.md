# RM-seq: Resistance Mutation SEQuencing

Analysis bioinformatic pipeline for high-throughput assessment of resistance mutations. RM-seq is an amplicon-based, deep-sequencing technique using single molecule barcoding. We have adapted this method to identify and characterise antibiotic resistance mutation.

The complete description of the RM-seq workflow is available [here](https://genomemedicine.biomedcentral.com/articles/10.1186/s13073-018-0572-z)

## Is this the right tool for me?

1. To be able to us this pipeline you need to have sequenced amplicon library with molecular barcodes.
2. It only supports paired-end FASTQ reads (including .gz compressed fastq files).
3. It needs paired reads that are overlapping.
4. It needs a reference fasta sequences of the sequenced gene (DNA sequence).
5. It's written in Python3 and Perl.

## Installation

### Install RM-seq pipeline

```
pip3 install rmseq
```

### Dependencies

RM-seq has the following package dependencies:
* EMBOSS >= 6.6 for `clustalo`, `cons`, `getorf`, `diffseq`
* clustal-omega >= 1.2.1
* bwa >= 0.7.15
* samtools >= 1.3
* bedtools >= 2.26.0
* pear >= 0.9.10
* cd-hit >= 4.7
* trimmomatic >= 0.36
* seqtk >= 1.3-r106 (only if you subsample reads)
* python modules: `plumbum`, `Biopython`


If you are using the [OSX Brew](http://brew.sh/) or [LinuxBrew](http://linuxbrew.sh/) packaging system:

```
brew tap homebrew/science
brew tap tseemann/bioinformatics-linux
brew install parallel; parallel --citation # please write will cite
brew install bedtools
brew install EMBOSS
brew install clustal-omega
brew install bwa
brew install samtools
brew install pear
brew install cd-hit
brew install trimmomatic
brew install seqtk
pip3 install plumbum
pip3 install biopython
```

## Quick start

Do

    rmseq

### Help

    usage: rmseq [-h]  ...

    Run RM-seq pipeline.

    optional arguments:
      -h, --help  show this help message and exit

    Commands:

        run       Run the pipeline.
        version   Print version.
        check     Check pipeline dependencies
        test      Run the test data set.

### To check dependencies are installed

    rmseq check

### To run the test dataset

    rmseq test

### To run analysis pipeline, follow the steps in
   
	rmseq run -h
    usage: rmseq run [options]

    Run the pipeline

    positional arguments:
      R1                    Path to read pair 1
      R2                    Path to read pair 2
      refnuc                Reference gene that will be used for premapping
                            filtering (fasta).
      outdir                Output directory.

    optional arguments:
      -h, --help            show this help message and exit
      -d, --debug_on        Switch on debug mode.
      -f, --force           Force overwite of existing.
      -b BARLEN, --barlen BARLEN
                            Length of barcode (default 16)
      -m MINFREQ, --minfreq MINFREQ
                            Minimum barcode frequency to keep (default 5)
      -c CPUS, --cpus CPUS  Number of CPUs to use (default 72)
	  -t TRANSLATION, --translation TRANSLATION
      	                    Manually set the reading frame for translation (use 1,
                            2 or 3 - use getorf by default)
      -r MINSIZE, --minsize MINSIZE
                            Minimum ORF size in bp used when annotating variants
                            (default 200)
      -w WSIZE, --wsize WSIZE
                            Word-size option to pass to diffseq for comparison
                            with reference sequence (default 5)
      -s SUBSAMPLE, --subsample SUBSAMPLE
                            Only examine this many reads.
      -k, --keepfiles       Keep the intermediate files. Default is to remove
                            intermediate files

### To check the version

    rmseq version

## Outputs

RM-seq produces a tap-separated output file called amplicons.effect where each raw correspond to a consensus amplicon (a genetic variant in the sequenced population):

Column | Example | Description
-------|---------|------------
barcode | GACACAACTGAGATTA | sequence of the barcode
sample | Rifampicin1 | output folder name
prot_mutation | H481N | annotation of the amino acid change (Histidine residue 481 substituted by Asparagine)
prot_start |  481 | start coordinate of the mutation 
prot_end | 481 | end coordinate of the mutation
nuc_mutation | C1443G | annotation of the nucleotide change
nuc_start | 1443 | start coordinate of the nucleotide change
nuc_end | 1443 | end coordinate of the nucleotide change
prot | VRPPDKNNRFVGLYCTLV... | protein sequence of the consensus sequence
dna | GGTTAGACCACCCGATAA... | dna sequence of the consensus sequence
reference_barcode | CTGACACGTCCTGAAG | barcode of the identical consesnsus amplicon used for annotation

The other files produced by RM-seq are:

File name | Description
----------|------------
amplicons.barcodes | Table with the count of each barcode sequence
amplicons.fna | Multifasta file containing all the consensus nucleotide sequence (header of sequence is the barcode)
amplicons.faa | Multifasta file containing all the consensus protein sequence (header of sequence is the barcode)
amplicons.fna.cdhit | Multifasta file containing all the unique consensus nucleotide sequence (header of sequence is the barcode)
amplicons.faa.cdhit | Multifasta file containing all the unique consensus amino acid sequence (header of sequence is the barcode)


## Issues

Please report problems to the [Issues Page](https://github.com/rguerillot/RM-seq/issues).

## Authors

Romain Guerillot (github: rguerillot) | Torsten Seemann (github: tseemann) | Mark B Schultz (github: schultzm)

## Citation

If you use this tool please cite:
[Guérillot R et al. Comprehensive antibiotic-linked mutation assessment by Resistance Mutation Sequencing (RM-seq). 2018. doi:10.1101/257915.](https://genomemedicine.biomedcentral.com/articles/10.1186/s13073-018-0572-z)
