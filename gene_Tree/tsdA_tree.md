##
```
../pfam_KO_to_seqs_OWC3211.sh tdsA na_na K19713 na_na 145 # tdsA were used in the gene name, 
#218

```

## tdsA tree
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/tsdA_tree

cat tsdA_bac_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,8 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > tsdA_annotree_hits.fasta

cd-hit -i tsdA_annotree_hits.fasta -o tsdA_annotree_hits_09.fasta

pullseq -i tsdA_annotree_hits_09.fasta -m 218 > tsdA_annotree_hits_09_length.fasta

#reference
cat tsdA_annotree_hits_09_length.fasta tsdA_4_tree.faa >tsdA_4_tree_w_ref.fasta

mafft --auto tsdA_4_tree_w_ref.fasta > tsdA_4_tree_w_ref_align.fasta

muscle -in tsdA_4_tree_w_ref_align.fasta -out tsdA_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in tsdA_4_tree_w_ref_align_refine.fasta -out tsdA_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <tsdA_4_tree_w_ref_align_refine_trim.fasta> tsdA_4_raw.fasttree

run_treeshrink.py  -t tsdA_4_raw.fasttree -m per-gene -o tsdA_treeshrink_pergene -a tsdA_4_tree_w_ref_align_refine_trim.fasta
```

##residue
```
#D3RVD4
#76-79-80 C-C-H
#187-190-191 C-C-H
cat tsdA_D3RVD4.fasta tsdA_raw_alg.fasta tdsA_4_tree.faa> tdsA_4_tree_uniref.fasta
/Users/pengfeiliu/software/mafft-mac/mafftdir/bin/mafft tdsA_4_tree_uniref.fasta > tdsA_4_tree_uniref_mafft.fasta
#161 left
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
