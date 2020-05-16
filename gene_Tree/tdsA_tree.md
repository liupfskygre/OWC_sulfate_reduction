##
```
../pfam_KO_to_seqs_OWC3211.sh tdsA na_na K19713 na_na 145
#218

```



##

##tdsA seqs from MAGs
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir tdsA_tree
cd tdsA_tree
#seqs list
grep -i 'tdsA' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >tdsA_gene_list.txt
cat tdsA_gene_list.txt|sort|uniq|wc -l
#304 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n tdsA_gene_list.txt>tdsA_gene_aa.faa
#304  grep -c '>' tdsA_gene_aa.faa 


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n tdsA_gene_list.txt>tdsA_gene_nt.fna
#304; grep -c '>' tdsA_gene_nt.fna
```
