## update hmm + DRAMOUT

```
cd /home/projects/Wetlands/sulfur_cycling_analysis/

#otr, otr	TIGR04315	375	325	440	330	

../pfam_KO_to_seqs_OWC3211.sh otr TIGR04315 na_na na_na 330
#326
```


## otr tree
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/otr_tree

cat otr_bac_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,5 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > otr_annotree_hits.fasta

cd-hit -i otr_annotree_hits.fasta -o otr_annotree_hits_09.fasta

pullseq -i otr_annotree_hits_09.fasta -m 330 > otr_annotree_hits_09_length.fasta

#reference
cat otr_annotree_hits_09_length.fasta otr_4_tree.faa >otr_4_tree_w_ref.fasta

mafft --auto otr_4_tree_w_ref.fasta > otr_4_tree_w_ref_align.fasta

muscle -in otr_4_tree_w_ref_align.fasta -out otr_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in otr_4_tree_w_ref_align_refine.fasta -out otr_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <otr_4_tree_w_ref_align_refine_trim.fasta> otr_4_raw.fasttree

run_treeshrink.py  -t otr_4_raw.fasttree -m per-gene -o otr_treeshrink_pergene -a otr_4_tree_w_ref_align_refine_trim.fasta
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
