- [Helpful resources](#helpful-resources)
    - [One-liners](#one-liners)
- [fastq](#fastq)
    - [Downsample fastq](#downsample-fastq)
        - [BBMap's reformat.sh](#bbmaps-reformatsh)
    - [fastq base count](#fastq-base-count)
    - [Extract fastq reads by id](#extract-fastq-reads-by-id)
    - [Filter fastq by sequence length](#filter-fastq-by-sequence-length)
    - [fastq format validation](#fastq-format-validation)


# Helpful resources

## One-liners

1.  https://github.com/stephenturner/oneliners
2.  https://github.com/crazyhottommy/bioinformatics-one-liners


# fastq 

## Downsample fastq

### BBMap's reformat.sh

* Downsampling using total number of bases. 

    ```
    reformat.sh in1={input.illumina_r1} \\
                in2={input.illumina_r2} \\
                out1={params.unzipped_r1} \\
                out2={params.unzipped_r2} \\
                samplebasestarget=$BASECOUNT \\
                sampleseed={params.seed_random}
    ```


## fastq base count 

* https://www.biostars.org/p/78043/  

* Using BBMap's reformat.sh

    ```
    reformat.sh in={input} \\
            2> {output}
    ```


## Extract fastq reads by id
    
    ```
    seqtk subseq in.fq name.lst > out.fq
    ```
   
 [Source](https://www.biostars.org/p/45356/#45357)
    

## Filter fastq by sequence length
Script `filter_fasta_by_seq_length.pl` from [ampli-tools](https://github.com/timkahlke/ampli-tools).

    ```
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

```
rm libStatGen -r
git clone git://github.com/statgen/libStatGen.git
make
```
