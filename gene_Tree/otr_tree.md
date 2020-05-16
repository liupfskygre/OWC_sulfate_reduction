## update hmm + DRAMOUT

```
cd /home/projects/Wetlands/sulfur_cycling_analysis/

#otr, otr	TIGR04315	375	325	440	330	

../pfam_KO_to_seqs_OWC3211.sh otr TIGR04315 na_na na_na 330
#326
```


#otr
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir otr_tree
cd otr_tree

#seqs list
grep -w -i 'otr' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >otr_gene_list.txt
cat otr_gene_list.txt|sort|uniq|wc -l
# 326


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n otr_gene_list.txt>otr_gene_aa.faa
#326  grep -c '>' otr_gene_aa.faa 
sed -e 's/ #.*$//g' otr_gene_aa.faa >otr_gene_aa_fixheader.faa


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n otr_gene_list.txt>otr_gene_nt.fna
#325; grep -c '>' otr_gene_nt.fna
sed -e 's/ #.*$//g' otr_gene_nt.fna > otr_gene_nt_fixheader.fna
```

#
```
#reference sequences

#cdd search
seqkit replace -p '(.+)$' -r '{kv}' -k alias.txt otr_gene_aa_fixheader.faa >otr_gene_aa_fixheader_alias.faa

#filtering out false positive sequences
grep -i -E 'TIGR04315|octaheme_Shew|cl37419' otr_hitdata.txt > otr_hitdata_positive.txt 
cat otr_hitdata_positive.txt |cut -f1 -d$'\t' |sed -e 's/\Q.*>//g'|sort|uniq>otr_hitdata_positive_id.txt
#all positive, 326

```
