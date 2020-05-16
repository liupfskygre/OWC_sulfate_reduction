##

## sdo tree from MAGs

```
#sdo
../pfam_KO_to_seqs_OWC3211.sh sdo K17725 na_na na_na 184

#415
```


```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir sdo_tree
cd sdo_tree
#seqs list
grep -i 'sdo' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >sdo_gene_list.txt
cat sdo_gene_list.txt|sort|uniq|wc -l
#428


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n sdo_gene_list.txt>sdo_gene_aa.faa
#428  grep -c '>' sdo_gene_aa.faa 
sed -e 's/ #.*$//g' sdo_gene_aa.faa>sdo_gene_aa_fixheader.faa


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n sdo_gene_list.txt>sdo_gene_nt.fna
#428; grep -c '>' sdo_gene_nt.fna 
sed -e 's/ #.*$//g' sdo_gene_nt.fna>sdo_gene_nt_fixheader.fna

#eggnong confirmation
```
