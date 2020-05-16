# dsrAB tree from MAGs


#purpose, to validate the search of hmm
#to separate rdsr and odsr

#wkdir
```
/home/projects/Wetlands/sulfur_cycling_analysis/dsrABD_tree

```

#all gene list from HMM search
```
/home/projects/Wetlands/sulfur_cycling_analysis/MAGs_Pro_Con_wide_w_profile_name.txt

```

##all gene list from DRAMOUT
```
# https://github.com/liupfskygre/OWC_sulfate_reduction/blob/master/DRAM_annotation_MAGs.md
/home/projects/Wetlands/sulfur_cycling_analysis/DRAM_OUT/sulfur_cycling_KO_HMM_gene_annotations_3211db.tsv

```

## sequences db
```
/home/projects/Wetlands/sulfur_cycling_analysis/all_3211_genes_DRAM.fna


/home/projects/Wetlands/sulfur_cycling_analysis/all_3211_genes_DRAM_aa.faa

for line in $(cat hmm_header.txt);do  pullseq -i /home/projects/Wetlands/sulfur_cycling_analysis/all_3211_genes_DRAM_aa.faa
 -g ${line} >> xxx.faa; done


for line in $(cat hmm_header.txt);do  pullseq -i /home/projects/Wetlands/sulfur_cycling_analysis/all_3211_genes_DRAM.fna -g ${line} >> xxx.fna; done
```

## dsrABD scripts
```
#dsrB scripts
../pfam_KO_to_seqs_OWC3211.sh dsrB na_na TIGR02064 K11181 256

#dsrA scripts
../pfam_KO_to_seqs_OWC3211.sh dsrA na_na TIGR02061 K11180 302

#dsrD
../pfam_KO_to_seqs_OWC3211.sh dsrD PF08697 na_na na_na 48

```

# reference for dsrAB
```
# 1292 Full_length_seq_dsrAB_header.txt, nearly full length seqs from Pelican 2016 and Muller 2015, EMI and ISME J

Full_length_seq_dsrAB_ref.faa
sed -e 's/ \+.*$/_Reference/g' Full_length_seq_dsrAB_ref.faa > Full_length_seq_dsrAB_ref_fixH.faa




```


## alignment 
```
#dsrA
mafft --auto --add dsrA_4_tree.faa --thread 6 Full_length_seq_dsrAB_ref_fixH.faa >dsrA_4_tree_alignment.faa

#--maxiterate 0 --thread -1 --keeplength --reorder 

#dsrB 
mafft --auto --add dsrB_4_tree.faa --thread 6 Full_length_seq_dsrAB_ref_fixH.faa >dsrB_4_tree_alignment.faa

#mafft --maxiterate 0 --add dsrB_4_tree.faa --thread 6 --keeplength Full_length_seq_dsrAB_ref_fixH.faa >dsrB_4_tree_alignment.faa

```









# old log #

## pullseq,

```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir dsrABD_tree
#seqs list
cd dsrABD_tree

# HMM hits
grep -i 'dsrB' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >tmp.txt

sed -i -e 's/$/\$/1' tmp.txt
grep -E -f tmp.txt ../all_bins_combined_annotations_3211db_ID.txt > hmm_header.txt 

## DRAMout hits
grep -E -i 'dsrB|TIGR02064|K11181' /home/projects/Wetlands/sulfur_cycling_analysis/DRAM_OUT/sulfur_cycling_KO_HMM_gene_annotations_3211db.tsv|cut -f1 -d$'\t' |sort|uniq >DRAM_header.txt 

cat hmm_header.txt DRAM_header.txt > hmm_DRAM_header.txt
cat hmm_DRAM_header.txt |sort|uniq > hmm_DRAM_header_uniq.txt

#get faa seqs
pullseq -i /home/projects/Wetlands/sulfur_cycling_analysis/all_3211_genes_DRAM_aa.faa -n hmm_DRAM_header_uniq.txt > dsrB_hmm_DRAM.faa

#get fna seqs
pullseq -i /home/projects/Wetlands/sulfur_cycling_analysis/all_3211_genes_DRAM.fna -n hmm_DRAM_header_uniq.txt > dsrB_hmm_DRAM.fna

sed -e 's/ .*$//g' dsrB_hmm_DRAM.faa >dsrB_fixH.faa
sed -i -e 's/\*$//g' dsrB_fixH.faa
rm dsrB_hmm_DRAM.faa
#
sed -e 's/ .*$//g' dsrB_hmm_DRAM.fna >dsrB_fixH.fna
sed -i -e 's/\*$//g' dsrB_fixH.fna
rm dsrB_hmm_DRAM.fna

rm hmm_DRAM_header.txt
rm DRAM_header.txt
rm hmm_header.txt

pullseq -i dsrB_fixH.faa -m 256 > dsrB_4_tree.faa
```



## dsrA 

## pullseq 
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir dsrABD_tree
#seqs list
grep -i 'dsrA' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >dsrA_gene_list.txt
cat dsrA_gene_list.txt|sort|uniq|wc -l
#439 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n dsrA_gene_list.txt>dsrA_gene_aa.faa
# 439 
sed -e 's/ #.*$//g' dsrA_gene_aa.faa >dsrA_gene_aa_fixheader.faa


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n dsrA_gene_list.txt>dsrA_gene_nt.fna
#437
sed -e 's/ #.*$//g' dsrA_gene_nt.fna >dsrA_gene_nt_fixheader.fna

```


## dsrD 

## pullseq 
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir dsrABD_tree
#seqs list
grep -i 'dsrD' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >dsrD_gene_list.txt
cat dsrD_gene_list.txt|sort|uniq|wc -l
#91


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n dsrD_gene_list.txt>dsrD_gene_aa.faa
# 91 

#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n dsrD_gene_list.txt>dsrD_gene_nt.fna
#91

sed -e 's/ #.*$//g' dsrD_gene_aa.faa >dsrD_gene_aa_fixheader.faa
sed -e 's/ #.*$//g' dsrD_gene_nt.fna >dsrD_gene_nt_fixheader.fna

```

## reference sequences preparation

**dsrD**
```
#1)Annotree to download reference seqs

cutoff: E= 1e-10 (0.0000000001); PF08679

#output from annotree
annotree_hits_dsrD.csv

cat annotree_hits_dsrD.csv|cut -f3,4,6 -d',' > annotree_hits_dsrD_sel.csv
awk -F ',' '{print ">" $2 "\n" $3}' annotree_hits_dsrD_sel.csv >annotree_hits_dsrD_sel.fa

#reference
cat annotree_hits_dsrD_sel.fa dsrD_gene_aa_fixheader.faa >dsrD_aa_cat.faa

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto dsrD_aa_cat.faa > dsrD_aa_cat_align.faa

#trimAl v1.4.rev22 build[2015-05-21]
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in dsrD_aa_cat_align.faa -out dsrD_aa_cat_trim.faa

#remove 95% gaps in UGENE

#IQ-TREE multicore version 1.6.10 for Mac OS X 64-bit built Feb 19 2019
iqtree -s dsrD_aa_cat_trim.faa  -nt 4 -pre dsrD_iqtree -alrt 1000 -bb 1000 

```



#

#write email to Karthik Anantharaman (karthik@bact.wisc.edu) to have the reference of dsrAB alignments, dsrD

#concatenate alignment when possible Geneious/seqkit when possible

