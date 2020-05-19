##soeABC

```

../pfam_KO_to_seqs_OWC3211.sh soeA na_na K21307 na_na 580
##D3RNN8, 967 aa, 60% of the typical length==580
# 214
```

##
```
#soeA_annotree_hits 

cd /home/projects/Wetlands/sulfur_cycling_analysis/soeA_tree

cat soeA_bac_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,8 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > soeA_annotree_hits.fasta

#reference
cat soeA_annotree_hits.fasta soeA_4_tree.faa >soeA_4_tree_w_ref.fasta

mafft --auto soeA_4_tree_w_ref.fasta > soeA_4_tree_w_ref_align.fasta

muscle -in soeA_4_tree_w_ref_align.fasta -out soeA_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in soeA_4_tree_w_ref_align_refine.fasta -out soeA_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <soeA_4_tree_w_ref_align_refine_trim.fasta> soeA_4_raw.fasttree

run_treeshrink.py  -t soeA_4_raw.fasttree -m per-gene -o soeA_treeshrink_pergene -a soeA_4_tree_w_ref_align_refine_trim.fasta
```

##
```
grep '>' soeA_4_tree.faa |sed 's/>//g' - > soeA_4_tree_gene.txt 
214 


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
