##soxB seqs from MAGs

##
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir soxB_tree
cd soxB_tree
#seqs list
grep -i 'soxB' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >soxB_gene_list.txt
cat soxB_gene_list.txt|sort|uniq|wc -l
#249 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n soxB_gene_list.txt>soxB_gene_aa.faa
#249  grep -c '>' soxB_gene_aa.faa 


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n soxB_gene_list.txt>soxB_gene_nt.fna
#248; grep -c '>' soxB_gene_nt.fna 
```

##soxC
```
#seqs list
grep -i 'soxC' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >soxC_gene_list.txt
cat soxC_gene_list.txt|sort|uniq|wc -l
#180 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n soxC_gene_list.txt>soxC_gene_aa.faa
#180  grep -c '>' soxC_gene_aa.faa 
sed -e 's/ #.*$//g' soxC_gene_aa.faa>soxC_gene_aa_fixheader.faa


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n soxC_gene_list.txt>soxC_gene_nt.fna
#180; grep -c '>' soxC_gene_nt.fna 
sed -e 's/ #.*$//g' soxC_gene_nt.fna>soxC_gene_nt_fixheader.fna

```

##soxA
```
#seqs list
grep -i 'soxA' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >soxA_gene_list.txt
cat soxA_gene_list.txt|sort|uniq|wc -l
#822 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n soxA_gene_list.txt>soxA_gene_aa.faa
#822  grep -c '>' soxA_gene_aa.faa 
sed -e 's/ #.*$//g' soxA_gene_aa.faa>soxA_gene_aa_fixheader.faa


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n soxA_gene_list.txt>soxA_gene_nt.fna
#822; grep -c '>' soxA_gene_nt.fna 
sed -e 's/ #.*$//g' soxA_gene_nt.fna>soxA_gene_nt_fixheader.fna
```
