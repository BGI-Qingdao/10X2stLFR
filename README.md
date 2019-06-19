# 10X2stLFR

This page is telling you how to covert 10X raw reads into stLFR reads format.

## one way to do it 

### step1 , run longranger basic

* what is longranger basic.

    it takes FASTQ files from longranger mkfastq and performs basic barcode processing including error correction, barcode white-listing, and attaching barcodes to reads.

* office document url of longranger.

    https://support.10xgenomics.com/genome-exome/software/pipelines/latest/what-is-long-ranger

* where to download longranger.

    https://support.10xgenomics.com/genome-exome/software/downloads/latest

* how to run longranger baisc
```
    YOUR-LONGRANGER-PATH/longranger basic --id=YOUR-iD --fastqs=YOUR-RAW-READS-PATH
```

### step 2 , covert reads head line from 10X format into stLFR reads format

* for read1 , do : 
```
## some command
```

* for read2 , do :
```
## some command
```
## data example

* 10X raw reads
```
reads
```
* 10X reads after barcode processing
```
reads
```
* stLFR reads
```
reads
```
