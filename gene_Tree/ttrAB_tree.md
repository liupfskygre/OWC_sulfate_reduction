##ttrA

##
```
../pfam_KO_to_seqs_OWC3211.sh ttrA na_na K08357 na_na 612

```

## ttrA tree
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/ttrA_tree

cat ttrA_bac_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,8 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > ttrA_annotree_hits.fasta

cd-hit -i ttrA_annotree_hits.fasta -o ttrA_annotree_hits_09.fasta

pullseq -i ttrA_annotree_hits_09.fasta -m 612 > ttrA_annotree_hits_09_length.fasta

#reference
cat ttrA_annotree_hits_09_length.fasta ttrA_4_tree.faa >ttrA_4_tree_w_ref.fasta

mafft --auto ttrA_4_tree_w_ref.fasta > ttrA_4_tree_w_ref_align.fasta

muscle -in ttrA_4_tree_w_ref_align.fasta -out ttrA_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in ttrA_4_tree_w_ref_align_refine.fasta -out ttrA_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <ttrA_4_tree_w_ref_align_refine_trim.fasta> ttrA_4_raw.fasttree

run_treeshrink.py  -t ttrA_4_raw.fasttree -m per-gene -o ttrA_treeshrink_pergene -a ttrA_4_tree_w_ref_align_refine_trim.fasta
```


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


##

cd /home/projects/Wetlands/sulfur_cycling_analysis/
#seqs list
grep -i 'ttrB' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >ttrB_gene_list.txt
cat ttrB_gene_list.txt|sort|uniq|wc -l
#204 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n ttrB_gene_list.txt>ttrB_gene_aa.faa
#204  grep -c '>' ttrB_gene_aa.faa 


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n ttrB_gene_list.txt>ttrB_gene_nt.fna
#204; grep -c '>' ttrB_gene_nt.fna

```

