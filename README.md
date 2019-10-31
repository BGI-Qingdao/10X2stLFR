# 10X2stLFR

This page is telling you how to covert 10X raw reads into stLFR reads format.

This project is still within developing.

## one way to do it 

### step1 , run longranger basic

* what is longranger basic.

    it takes FASTQ files from longranger mkfastq and performs basic barcode processing including error correction, barcode white-listing, and attaching barcodes to reads.

* official document url of longranger.

    https://support.10xgenomics.com/genome-exome/software/pipelines/latest/what-is-long-ranger

* where to download longranger.

    https://support.10xgenomics.com/genome-exome/software/downloads/latest

* how to run longranger baisc
```
    YOUR-LONGRANGER-PATH/longranger basic --id=YOUR-ID --fastqs=YOUR-RAW-READS-PATH
```
* where is the output file

```
YOUR-RUN-PATH/YOUR-ID/outs/barcoded.fastq.gz
```

### step 2 , covert reads head line from 10X format into stLFR reads format

* command example : 
```
gzip -dc barcoded.fastq.gz | awk  'BEGIN{id=2;}{if((NR)%4==1) {if(id==1){id=2;}else{id=1;};if(NF>1){bc=$2;}else{bc="0_0_0";} printf("%s#%s/%d\n",$1,bc,id) >> "stlfr_read."id".fastq"} else {printf("%s\n",$1)>>"stlfr_read."id".fastq"}  }'
```

## data example

* 10X raw reads

```
@E00247:267:HMVT3CCXX:3:1101:9942:2258 1:N:0:0
GGAATAACATGGGAACCAGGTCACACACACACACCACACACACTATACATATACATACATAATAAACACATTATATATACACACCACATACACCATACATACACCACACACTACACATAACACACACATAATGCCACATACACACACCACA
+
<AFFFKKKKKAKKKKKK<KKKKKKKKKKKKKKKKKKKKKKKKK7FFKKKKKKKKK<FFKFKKKK,A,AK7A,7,KAFFKKKKKKKKKKFFKK,<FFFKK<F<AFKKKFKKKFFFK77,,7FKAKAKFKFKK,,7,<,F<<,A<F<<<FKF7
```
* 10X reads after barcode processing

```
@E00247:267:HMVT3CCXX:3:1101:9942:2258 BX:Z:GGAATAACATGGGAAC-1
CACACACACACCACACACACTATACATATACATACATAATAAACACATTATATATACACACCACATACACCATACATACACCACACACTACACATAACACACACATAATGCCACATACACACACCACA
+
KKKKKKKKKKKKKKKKKKKK7FFKKKKKKKKK<FFKFKKKK,A,AK7A,7,KAFFKKKKKKKKKKFFKK,<FFFKK<F<AFKKKFKKKFFFK77,,7FKAKAKFKFKK,,7,<,F<<,A<F<<<FKF7
```

* stLFR reads

```
@E00247:267:HMVT3CCXX:3:1101:9942:2258#BX:Z:GGAATAACATGGGAAC-1/1
CACACACACACCACACACACTATACATATACATACATAATAAACACATTATATATACACACCACATACACCATACATACACCACACACTACACATAACACACACATAATGCCACATACACACACCACA
+
KKKKKKKKKKKKKKKKKKKK7FFKKKKKKKKK<FFKFKKKK,A,AK7A,7,KAFFKKKKKKKKKKFFKK,<FFFKK<F<AFKKKFKKKFFFK77,,7FKAKAKFKFKK,,7,<,F<<,A<F<<<FKF7
```

## How to convert back into barcoded.fq

* format switch back into 10X style 

```
awk '{ if((NR)%4==1){split($1,a,"/") ;c=a[1]; split(c,b,"#"); if(b[2] == "0_0_0"){print b[1]; }else{printf("%s %s\n",b[1], b[2]);}} else { print $0 } }'  stlfr_read.1.fastq > 10X.1.fastq

awk '{ if((NR)%4==1){split($1,a,"/") ;c=a[1]; split(c,b,"#"); if(b[2] == "0_0_0"){print b[1]; }else{printf("%s %s\n",b[1], b[2]);}} else { print $0 } }'  stlfr_read.2.fastq > 10X.2.fastq
```

* merge r1 and r1 into barcoded.fastq

```
./merge.pl 10X.1.fastq 10X.2.fastq | gzip -c >barcoded.fastq.gz
```
