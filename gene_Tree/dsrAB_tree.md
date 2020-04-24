# dsrAB tree from MAGs


#purpose, to validate the search of hmm
#to separate rdsr and odsr

#wkdir
```
/home/projects/Wetlands/sulfur_cycling_analysis/dsrABD_tree

```

#all gene list
```
/home/projects/Wetlands/sulfur_cycling_analysis/MAGs_Pro_Con_wide_w_profile_name.txt

```


## dsrB 

## pullseq 
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir dsrABD_tree
#seqs list
grep -i 'dsrB' MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >dsrB_gene_list715.txt
mv dsrB_gene_list715.txt dsrABD_tree
cat dsrB_gene_list715.txt|sort|uniq|wc -l
#363 



#all 3211 MAGs gene aa seqs: prodigal.faa
cd 
/home/projects/Wetlands/sulfur_cycling_analysis/sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa


#fna list? confusing about the file from Adrienne: all_wetlands_bins_combined.genes.fasta
cp /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/prodigal_genes_NT/all_wetlands_bins_combined.genes.fasta ./

cd dsrABD_tree
#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n dsrB_gene_list715.txt>dsrB_gene_aa.faa
# 363 
sed -e 's/ #.*$//g' dsrB_gene_aa.faa >dsrB_gene_aa_fixH.faa
sed -i -e 's/\*$//g' dsrB_gene_aa_fixH.faa

#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n dsrB_gene_list715.txt>dsrB_gene_nt.fna
#362
sed -e 's/ #.*$//g' dsrB_gene_nt.fna >dsrB_gene_nt_fixH.fna
#102_Metabat_MUD_2014_2015_coassembly_102_Metabat_MUD_2014_2015_coassembly.fa102_Metabat_MUD_2014_2015_coassembly_scaffold_3016_10 missing #
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

