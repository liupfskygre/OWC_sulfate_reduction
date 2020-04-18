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
