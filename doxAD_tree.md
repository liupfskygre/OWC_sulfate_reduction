##

##
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir doxD_tree
cd doxD_tree
#seqs list
```
grep -i 'doxD' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >doxD_gene_list.txt
cat doxD_gene_list.txt|sort|uniq|wc -l
#758 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n doxD_gene_list.txt>doxD_gene_aa.faa
#758  grep -c '>' doxD_gene_aa.faa 


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n doxD_gene_list.txt>doxD_gene_nt.fna
#758; grep -c '>' doxD_gene_nt.fna
```
