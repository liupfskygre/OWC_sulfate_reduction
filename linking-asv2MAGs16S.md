## asv from owc 2018 linking to MAGs3211 genomes


##file

```
#/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/

16S sequences from dram
#how to extract 16S from DRAM??

#try on this 
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/OWC_subtractive_megahit_Surface_metabat.983_DRAMOUT

#cat rrnas.tsv
#scaffold	fasta	begin	end	strand	type	e-value	note
$1 $3 $4

DRAM.py strainer -i rrnas_16S.tsv -f scaffolds.fna -o contigs_w_16.fasta
grep -e '16S' rrnas.tsv |cut -f1,3,4 -d$'\t' > rrnas_16S.tsv

/home/projects/Wetlands/All_genomes/scripts/trim_scaffold_by_pos2.py -i rrnas_16S.tsv -f contigs_w_16.fasta -o rRNA16s.fasta


Asv sequences from OWC wetland 2018 sequences

/home/projects/Wetlands/2018_sampling/16S_analysis/combined_summer_16S/may-sept_merged_rep_seqs
(qiime2-2019.10) [liupf@zenith combined_summer_16S]$ qiime tools export --input-path may-sept_merged_rep_seqs.qza --output-path may-sept_merged_rep_seqs

```

##
```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes
First do this:
grep 16S all_bins_combined_rrnas_3211db.tsv | cut -f 1,3,4 > just_16S_rrna_info.tsv
Then get the ids to pull the sequences:
cut -f 1 just_16S_rrna_info.tsv > a
I used a different script, but you should be able to use pullseq to do this:
pull_sequence.py -i a all_bins_combined_3211db_scaffolds.fna > just_16S_scaffolds.fna
Check that you got them all:
wc -l a  
grep -c ">" just_16S_scaffolds.fna
Not the same:
sort a | uniq | wc -l
yes they are, looks like we had duplicate contigs in the rrnas file - probably split rRNA
rm a 
Trim the sequences:
python /home/projects/Wetlands/All_genomes/scripts/trim_scaffold_by_pos2.py -f just_16S_scaffolds.fna -i just_16S_rrna_info.tsv -o just_16S_TRIMMED_genes.fna

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
