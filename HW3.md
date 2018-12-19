# Summarize a genome assembly

## Download most current assembly

```
wget ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/fasta/dmel-all-chromosome-r6.24.fasta.gz
```

## File integrity

Download the check sums text file to check the integrity of the downloaded file

```
wget ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/fasta/md5sum.txt
```

Check the md5sum against the md5sum.txt file

```
md5sum -c md5sum.txt

dmel-all-chromosome-r6.24.fasta.gz: OK

```

## Calculate number of nucleotides, number of N's and total number of sequences

```
module load jje/kent
module load jje/jjeutils

faSize dmel-all-chromosome-r6.24.fasta.gz
43726002 bases (1152978 N's 142573024 real 142573024 upper 0 lower) in 1870 sequences in 1 files
Total size: mean 76858.8 sd 1382100.2 min 544 (211000022279089) max 32079331 (3R) median 1577
N count: mean 616.6 sd 6960.7
U count: mean 76242.3 sd 1379508.4
L count: mean 0.0 sd 0.0
%0.00 masked total, %0.00 masked real
```

There are 43,726,002 nucleotides. 
There are 1152978 N's. 
There are 1870 sequences.  

### Looks like you left out the leading 1 in the # of nt's.

# Summarize an annotation files

## Download the gtf file

I am downloading the gtf file and checksum file in a separate directory I created in my workspace.

```
wget ftp://ftp.flybase.net/genomes/Drosophila_melanogaster/current/gtf/dmel-all-r6.24.gtf.gz
```

## File integrity

```
md5sum -c md5sum.txt
dmel-all-r6.24.gtf.gz: OK

```

## Summary report

Pull out the feature list from the gtf file

```
awk '{print $3}' dmel-all-r6.24.gtf > featurelist.txt
```

## Sort the feature list by category and count how many featuers per category

```
sort featurelist.txt \
| uniq -c \
| sort -rn \
| nl
     1	 187315 exon
     2	 161014 CDS
     3	  46339 5UTR
     4	  33358 3UTR
     5	  30591 start_codon
     6	  30533 stop_codon
     7	  30507 mRNA
     8	  17772 gene
     9	   2961 ncRNA
    10	    485 miRNA
    11	    334 pseudogene
    12	    312 tRNA
    13	    299 snoRNA
    14	    262 pre_miRNA
    15	    115 rRNA
    16	     32 snRNA
```

As a pipeline to not write an extra file of the feature list on its own.

```
awk '{print $3}' dmel-all-r6.24.gtf \
| sort \
| uniq -c \
| sort -rn \
| nl
     1	 187315 exon
     2	 161014 CDS
     3	  46339 5UTR
     4	  33358 3UTR
     5	  30591 start_codon
     6	  30533 stop_codon
     7	  30507 mRNA
     8	  17772 gene
     9	   2961 ncRNA
    10	    485 miRNA
    11	    334 pseudogene
    12	    312 tRNA
    13	    299 snoRNA
    14	    262 pre_miRNA
    15	    115 rRNA
    16	     32 snRNA
```

## Total number of genes per chromosome arm

To get columns with only the features and the chromosome out of the gtf file

```
awk '{print $1,$3}' dmel-all-r6.24.gtf > chrfeature.txt
```

Regular expressions and sorting to get number of genes on chromosome arms (X, Y, 2L, 2R, 3L, 3R, 4):

```
grep '[XY234][LR]\?' chrfeature.txt \
| grep 'gene' \
| sort \
| uniq -c \
| sort -rn \
| nl

     1	   4202 3R gene
     2	   3628 2R gene
     3	   3501 2L gene
     4	   3464 3L gene
     5	   2676 X gene
     6	    113 Y gene
     7	    111 4 gene
     8	     73 X pseudogene
     9	     62 Y pseudogene
    10	     53 2R pseudogene
    11	     49 2L pseudogene
    12	     40 3R pseudogene
    13	     38 3L pseudogene
    14	     10 4 pseudogene
    15	      2 211000022280494 gene
    16	      1 211000022280703 pseudogene
    17	      1 211000022280703 gene
    18	      1 211000022280481 gene
    19	      1 211000022280347 gene
    20	      1 211000022280341 pseudogene
    21	      1 211000022280341 gene
    22	      1 211000022280328 gene
    23	      1 211000022279681 gene
    24	      1 211000022279392 gene
    25	      1 211000022279264 gene
    26	      1 211000022279188 gene
    27	      1 211000022279165 gene
    28	      1 211000022278760 gene
    29	      1 211000022278449 pseudogene
    30	      1 211000022278449 gene
    31	      1 211000022278436 gene
    32	      1 211000022278279 pseudogene
    33	      1 211000022278279 gene

```

There are: </br>
4202 genes on 3R </br>
3628 genes on 2R </br>
3501 genes on 2L </br>
3464 genes on 3L </br>
2676 genes on X </br>
113 genes on Y </br>
111 genes on 4 </br>

### Comments on "Summarize an annotation file"

Excellent.
