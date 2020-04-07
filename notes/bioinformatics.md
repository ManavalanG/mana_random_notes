- [Helpful resources](#helpful-resources)
  - [One-liners](#one-liners)
- [fastq](#fastq)
  - [Downsample fastq](#downsample-fastq)
    - [BBMap's reformat.sh](#bbmaps-reformatsh)
  - [fastq base count](#fastq-base-count)
  - [Extract fastq reads by id](#extract-fastq-reads-by-id)
  - [Filter fastq by sequence length](#filter-fastq-by-sequence-length)
  - [fastq format validation](#fastq-format-validation)
- [fasta](#fasta)
  - [Filter by header](#filter-by-header)
- [vcf](#vcf)
  - [bcftools](#bcftools)
    - [Commands available](#commands-available)
    - [Examples](#examples)


# Helpful resources

## One-liners

1. https://github.com/stephenturner/oneliners
2. https://github.com/crazyhottommy/bioinformatics-one-liners


# fastq

## Downsample fastq

### BBMap's reformat.sh

- Downsampling using total number of bases.

    ```sh
    reformat.sh in1={input.illumina_r1} \\
                in2={input.illumina_r2} \\
                out1={params.unzipped_r1} \\
                out2={params.unzipped_r2} \\
                samplebasestarget=$BASECOUNT \\
                sampleseed={params.seed_random}
    ```


## fastq base count

- https://www.biostars.org/p/78043/

- Using BBMap's reformat.sh

    ```bash
    reformat.sh in={input} \\
            2> {output}
    ```


## Extract fastq reads by id

```sh
seqtk subseq in.fq name.lst > out.fq
```

[Source](https://www.biostars.org/p/45356/#45357)


## Filter fastq by sequence length

Script `filter_fasta_by_seq_length.pl` from [ampli-tools](https://github.com/timkahlke/ampli-tools).

```sh
# usage example
FILT_SIZE=10000
filter_fasta_by_seq_length.pl  \
        -i <IN_FASTA> \
        -o <FILTERED_FASTA> \
        -a ${FILT_SIZE}
```


## fastq format validation

Tool [FastQValidator](https://genome.sph.umich.edu/wiki/FastQValidator)

Note:
Installation of release version `0.1.1a` resulted in error, and it was resolved based on [solution here](https://vcru.wisc.edu/simonlab/bioinformatics/programs/install/fastqvalidator.htm). In short, `libStatGen` supplied was out of date, and was obtained directly from its source.

```sh
rm libStatGen -r
git clone git://github.com/statgen/libStatGen.git
make
```

# fasta

## Filter by header

- Using `bioawk`

    ```sh
    # filter header ID containing substring "OCS"
    bioawk -c fastx '{if ($name ~ /OCS/) {print ">"$name " " $comment;print $seq}}' fname.fasta

    # Pipe to "fold -120" to wrap sequence. Note: This would also wrap header line though.
    ```


# vcf

## bcftools

[bcftools](https://samtools.github.io/bcftools/bcftools.html) — utilities for variant calling and manipulating VCFs and BCFs. This tool can handle several filtering/querying/etc.. processes dealing with vcf data.

### Commands available

* annotate .. edit VCF files, add or remove annotations
* call .. SNP/indel calling (former "view")
* cnv .. Copy Number Variation caller
* concat .. concatenate VCF/BCF files from the same set of samples
* consensus .. create consensus sequence by applying VCF variants
* convert .. convert VCF/BCF to other formats and back
* csq .. haplotype aware consequence caller
* filter .. filter VCF/BCF files using fixed thresholds
* gtcheck .. check sample concordance, detect sample swaps and contamination
* index .. index VCF/BCF
* isec .. intersections of VCF/BCF files
* merge .. merge VCF/BCF files files from non-overlapping sample sets
* mpileup .. multi-way pileup producing genotype likelihoods
* norm .. normalize indels
* plugin .. run user-defined plugin
* polysomy .. detect contaminations and whole-chromosome aberrations
* query .. transform VCF/BCF into user-defined formats
* reheader .. modify VCF/BCF header, change sample names
* roh .. identify runs of homo/auto-zygosity
* sort .. sort VCF/BCF files
* stats .. produce VCF/BCF stats (former vcfcheck)
* view .. subset, filter and convert VCF and BCF files


### Examples

```sh
# Extract calls based on INFO values. Multiple conditions (and/or) can be used. Example below from Duphold.
$ bcftools view -i '(SVTYPE = "DEL" & FMT/DHFFC[0] < 0.7) | (SVTYPE = "DUP" & FMT/DHBFC[0] > 1.3)' SV_CALLS.vcf

# Extract allele frequency at each position
$ bcftools query -f '%CHROM %POS %AF\n' file.bcf | head -3
1 13380 7.69515e-05
1 16071 0.000123122
1 16141 0.000138513

# Extract a particular tag from INFO column
$ bcftools query -f '%CHROM\t%POS\t%INFO/SVTYPE\n' tp-call.vcf | head
10	829773	DEL
10	1092494	DEL
10	1741456	DEL
```


## Chromosome mappings


This repository contains chromosome/contig name mappings between UCSC <-> Ensembl <-> Gencode for a variety of genomes:

https://github.com/dpryan79/ChromosomeMappings
