## asv from owc 2018 linking to MAGs3211 genomes


##file

```
#/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/

16S sequences from dram
#how to extract 16S from DRAM??


Asv sequences from OWC wetland 2018 sequences

/home/projects/Wetlands/2018_sampling/16S_analysis/combined_summer_16S/may-sept_merged_rep_seqs
(qiime2-2019.10) [liupf@zenith combined_summer_16S]$ qiime tools export --input-path may-sept_merged_rep_seqs.qza --output-path may-sept_merged_rep_seqs

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
