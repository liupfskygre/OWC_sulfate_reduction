##ttrA

```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir ttrA_tree
cd ttrA_tree

#seqs list
grep -i 'ttrA' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >ttrA_gene_list.txt
cat ttrA_gene_list.txt|sort|uniq|wc -l
#67 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n ttrA_gene_list.txt>ttrA_gene_aa.faa
#67  grep -c '>' ttrA_gene_aa.faa 


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n ttrA_gene_list.txt>ttrA_gene_nt.fna
#67; grep -c '>' ttrA_gene_nt.fna

```
