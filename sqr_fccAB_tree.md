##

##sqr seqs from MAGs
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir sqr_tree
cd sqr_tree
#seqs list
grep -i 'sqr' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >sqr_gene_list.txt
cat sqr_gene_list.txt|sort|uniq|wc -l
#1518 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n sqr_gene_list.txt>sqr_gene_aa.faa
#1518;  grep -c '>' sqr_gene_aa.faa 


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n sqr_gene_list.txt>sqr_gene_nt.fna
#1517; grep -c '>' sqr_gene_nt.fna 
```
