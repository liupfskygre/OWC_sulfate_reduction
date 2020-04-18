## phsA tree from MAGs

```
d /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir phsA_tree
cd phsA_tree
#seqs list
grep -i 'phsA' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >phsA_gene_list.txt
cat phsA_gene_list.txt|sort|uniq|wc -l
#822 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n phsA_gene_list.txt>phsA_gene_aa.faa
#822  grep -c '>' phsA_gene_aa.faa 


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n phsA_gene_list.txt>phsA_gene_nt.fna
#822; grep -c '>' phsA_gene_nt.fna 


```
