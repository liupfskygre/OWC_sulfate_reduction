##soeABC

```

../pfam_KO_to_seqs_OWC3211.sh soeA na_na K21307 na_na 580
##D3RNN8, 967 aa, 60% of the typical length==580
# 214
```

```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir soeA_tree
cd soeA_tree

#seqs list
grep -i 'soeA' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >soeA_gene_list.txt
cat soeA_gene_list.txt|sort|uniq|wc -l
#312 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n soeA_gene_list.txt>soeA_gene_aa.faa
#312  grep -c '>' soeA_gene_aa.faa 
sed -e 's/ #.*$//g' soeA_gene_aa.faa >soeA_gene_aa_fixheader.faa


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n soeA_gene_list.txt>soeA_gene_nt.fna
#311; grep -c '>' soeA_gene_nt.fna
sed -e 's/ #.*$//g' soeA_gene_nt.fna > soeA_gene_nt_fixheader.fna

```
