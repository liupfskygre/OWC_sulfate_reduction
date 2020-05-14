## asv from owc 2018 linking to MAGs3211 genomes




## based on all combined file by Adrienne
```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes

#First do this:
grep '16S' all_bins_combined_rrnas_3211db.tsv | cut -f 1,3,4 > just_16S_rrna_info.tsv

#just_16S_rrna_info.tsv, 301 lines

#Then get the ids to pull the sequences:
cut -f 1 just_16S_rrna_info.tsv > a
#I used a different script, but you should be able to use pullseq to do this:
pull_sequence.py -i a all_bins_combined_3211db_scaffolds.fna > just_16S_scaffolds.fna
#pullseq -n a -i all_bins_combined_3211db_scaffolds.fna>just_16S_scaffolds.fna


#Check that you got them all:
wc -l a   #294
grep -c ">" just_16S_scaffolds.fna #301

#Not the same:
sort a | uniq | wc -l   #294


#repeated 
Aug_M2_C1_D5_30Gb_idba_metabat.80_Aug_M2_C1_D5_30Gb_idba_metabat_scaffold_630
M3C4D3_v1_idba.127_M3C4D3_v1_idba_scaffold_3183
Nov_Plant_rebin_metabat.2_Nov_Plant_rebin_metabat_Nov_Plant_scaffold_4383
O3C3D3_DDIG_MN.1090_O3C3D3_DDIG_MN_k121_24246102
O3C3D4_DDIG_MN.607_O3C3D4_DDIG_MN_k121_1721028
O3D3D3_DDIG_megahit.356_O3D3D3_DDIG_megahit_k121_4428038
OWC_substrative_co_megahit_Deep_metabat.1272_OWC_substrative_co_megahit_Deep_metabat_k121_46235138


#yes they are, looks like we had duplicate contigs in the rrnas file - probably split rRNA

rm a  

#Trim the sequences:
python /home/projects/Wetlands/All_genomes/scripts/trim_scaffold_by_pos2.py -f just_16S_scaffolds.fna -i just_16S_rrna_info.tsv -o just_16S_TRIMMED_genes.fna

#and only the second part is kept. 

for line in $(cat split_16s_list.txt) 
do 
grep "${line}" just_16S_rrna_info.tsv |head -1 >> split_16s_missed_info.txt
done

pullseq -n split_16s_list.txt -i just_16S_scaffolds.fna > just_16S_scaffolds_add.fna
sed -i -e "s/\t.*$//" just_16S_scaffolds_add.fna

python /home/projects/Wetlands/All_genomes/scripts/trim_scaffold_by_pos2.py -f just_16S_scaffolds_add.fna -i split_16s_missed_info.txt -o just_16S_TRIMMED_genes_recovery.fna

cat just_16S_TRIMMED_genes_recovery.fna just_16S_TRIMMED_genes.fna > DB3211_16S_301seq.fna

grep -c '>' DB3211_16S_301seq.fna
#301

cp DB3211_16S_301seq.fna /home/projects/Wetlands/sulfur_cycling_analysis/rRNA16S_DB3211_ASV
```

#short summary

```
both slurm_get16S_from_DRAM.sh and Adrienne's way worked, but 
the scripts from Adrienne only print only the second part 16s if there is split 16S on one contigs
```

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

## link 16s asv with 16S in MAGs

```

cd /home/projects/Wetlands/2018_sampling/16S_analysis/combined_summer_16S/may-sept_merged_rep_seqs
cp dna-sequences.fasta /home/projects/Wetlands/sulfur_cycling_analysis/rRNA16S_DB3211_ASV/ASV_OWC2018May_Sep.fna

cd /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes
cp DB3211_16S_301seq.fna /home/projects/Wetlands/sulfur_cycling_analysis/rRNA16S_DB3211_ASV 

wkdir 
cd /home/projects/Wetlands/sulfur_cycling_analysis/rRNA16S_DB3211_ASV

#usearch global
usearch -usearch_global ASV_OWC2018May_Sep.fna -db DB3211_16S_301seq.fna -strand both -id 0.99 -alnout otu_try.aln -notrunclabels -query_cov 0.99 -userout otu_try_userout.txt -maxaccepts 100000 -userfields query+target+id+alnlen+mism+opens+qlo+qhi+tlo+thi+evalue+bits
sed -i "1i query\ttarget\tid\talnlen\tmism\topens\tqlo\tqhi\ttlo\tthi\tevalue\tbits" otu_try_userout.txt 

-mincols 200  #0.4%, the same as -query_cov 0.99 #0.4% , #432 sequences



usearch -usearch_global ASV_OWC2018May_Sep.fna -db DB3211_16S_301seq.fna -strand both -id 0.999 -alnout otu_try_999.aln -notrunclabels -query_cov 0.999 -userout otu_try_userout_999.txt -maxaccepts 100000 -userfields query+target+id+alnlen+mism+opens+qlo+qhi+tlo+thi+evalue+bits
sed -i "1i query\ttarget\tid\talnlen\tmism\topens\tqlo\tqhi\ttlo\tthi\tevalue\tbits" otu_try_userout_999.txt

#89 seques

#>100bp alignment , id=0.999
usearch -usearch_global ASV_OWC2018May_Sep.fna -db DB3211_16S_301seq.fna -strand both -id 0.999 -alnout otu_try_999_s100.aln -notrunclabels -mincols 100 -userout otu_try_userout_999_s100.txt -maxaccepts 100000 -userfields query+target+id+alnlen+mism+opens+qlo+qhi+tlo+thi+evalue+bits

sed -i "1i query\ttarget\tid\talnlen\tmism\topens\tqlo\tqhi\ttlo\tthi\tevalue\tbits" otu_try_userout_999_s100.txt
#also 89

```

##MAGs with ASV link
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/rRNA16S_DB3211_ASV

awk -F '\t' '{print NF; exit}' /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_annotations_3211db.tsv 
#28

cat /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_annotations_3211db.tsv | cut -f1,2,3,26,27,28 -d$'\t'> all_bins_combined_annotations_3211db_simple.tsv

awk -F '\t' '{print $0"\t"$1}' all_bins_combined_annotations_3211db_simple.tsv >tmp.tsv
rev tmp.tsv | sed -e 's/[1-9]\+_//' |rev  > all_bins_combined_annotations_3211db_simple.tsv 

#0.99 and -mincols 200; 433 sequences
cat otu_try_userout.txt|cut -f2 -d$'\t' >Contigs_w_linking_ASV.txt
sed -i -e 's/:.*$//g' Contigs_w_linking_ASV.txt
cat Contigs_w_linking_ASV.txt |sort|uniq >Contigs_w_linking_ASV_uniq.txt

#432 ASV linking to 109 Contigs (or MAGs)

grep -w -f Contigs_w_linking_ASV_uniq.txt all_bins_combined_annotations_3211db_simple.tsv > MAGs_w_linking_ASV.txt

cat MAGs_w_linking_ASV.txt |cut -f2,3,4,5,6,7 -d$'\t' |sort|uniq >MAGs_w_linking_ASV_uniq.txt


##89, coverage 0.999, id=0.999

cat otu_try_userout_999.txt|cut -f2 -d$'\t' >Contigs_w_linking_ASV.txt
sed -i -e 's/:.*$//g' Contigs_w_linking_ASV.txt
cat Contigs_w_linking_ASV.txt |sort|uniq >Contigs_w_linking_ASV_uniq.txt
#89

grep -w -f Contigs_w_linking_ASV_uniq.txt all_bins_combined_annotations_3211db_simple.tsv > MAGs_w_linking_ASV.txt

cat MAGs_w_linking_ASV.txt |cut -f2,3,4,5,6,7 -d$'\t' |sort|uniq >MAGs_w_linking_ASV_uniq_999.txt

#88
```



## references 

## link 16s asv with 16S in MAGs
```
1) using mapping, 100% identity bbmap.sh minid=1

2) usearch global ==> search_exact (partial 16s seqs in MAGs will fail?)
# -mincols	
# -id 

usearch -usearch_global otu20.fa -db gold.fa -strand plus -id 0.99 -alnout otu20.aln -notrunclabels

#Search for one (default) or a few high-identity hits to a database using the USEARCH algorithm. Alignments are global. To get more than one hit, increase -maxaccepts (see accept options).

An identity threshold must be specified using the -id option. If full-length, exact matches are required, then it is better to use search_exact than usearch_global with -id 1.0.

3), use blastn?? 
#Briefly, each ASV was searched using BLASTN, with an E value of 1e−10 and -num_alignments 100000. Hits greater than 99% nucleotide identity over at least 200 bp were retained for further analysis.
#controled by??
#length    Alignment length; 
#pident    Percentage of identical matches
```

#references
```
https://github.com/torognes/vsearch/issues/295
https://github.com/torognes/vsearch


https://www.drive5.com/usearch/manual8.1/upp_tut_misop_mock.html
http://drive5.com/usearch/manual/cmd_usearch_global.html
http://drive5.com/usearch/manual/cmd_search_exact.html

http://www.metagenomics.wiki/tools/blast/blastn-output-format-6
http://drive5.com/usearch/manual/accept_options.html
```
