## asv from owc 2018 linking to MAGs3211 genomes


##file

```
16S sequences from dram



Asv sequences from OWC wetland 2018 sequences
```

##
1) using mapping, 100% identity bbmap.sh minid=1

2) usearch global ==> search_exact

usearch -usearch_global otu20.fa -db gold.fa -strand plus -id 0.99 -alnout otu20.aln -notrunclabels

#Search for one (default) or a few high-identity hits to a database using the USEARCH algorithm. Alignments are global. To get more than one hit, increase -maxaccepts (see accept options).

An identity threshold must be specified using the -id option. If full-length, exact matches are required, then it is better to use search_exact than usearch_global with -id 1.0.

#references
```
https://github.com/torognes/vsearch/issues/295
https://github.com/torognes/vsearch


https://www.drive5.com/usearch/manual8.1/upp_tut_misop_mock.html
http://drive5.com/usearch/manual/cmd_usearch_global.html
http://drive5.com/usearch/manual/cmd_search_exact.html

```
