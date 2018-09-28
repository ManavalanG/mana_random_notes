- [Helpful resources](#helpful-resources)
    - [One-liners](#one-liners)
- [fastq](#fastq)
    - [Downsample fastq](#downsample-fastq)
        - [BBMap's reformat.sh](#bbmaps-reformatsh)
    - [fastq base count](#fastq-base-count)
    - [Extract fastq reads by id](#extract-fastq-reads-by-id)


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
    