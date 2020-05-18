##

##
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir doxD_tree
cd doxD_tree
#seqs list
```
grep -i 'doxD' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >doxD_gene_list.txt
cat doxD_gene_list.txt|sort|uniq|wc -l
#758 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n doxD_gene_list.txt>doxD_gene_aa.faa
#758  grep -c '>' doxD_gene_aa.faa 


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n doxD_gene_list.txt>doxD_gene_nt.fna
#758; grep -c '>' doxD_gene_nt.fna
```

```

../pfam_KO_to_seqs_OWC3211.sh doxD PF04173 K16937 na_na 125
#835

```

#conserved residue domain search
```
#use cdd search batch mode
rename header
seqkit replace -p '(.+)$' -r '{kv}' -k doxD_match_header.txt doxD_4_tree.faa >  doxD_4_tree_cdd.faa

Upload to CDD search

#residue is not clear yet. 
cat doxD_4_tree.faa UPI0002624EAE_w_CDD_pf04173.fasta> doxD_4_tree_UPI0002624EAE_w_CDD_pf04173.faa


/Users/pengfeiliu/software/mafft-mac/mafftdir/bin/mafft  doxD_4_tree_UPI0002624EAE_w_CDD_pf04173.faa > doxD_4_tree_UPI0002624EAE_w_CDD_pf04173_mafft.fasta

lcl|consensus
#6R-28L-37G-75E-79G-85G-90R-108-116E-141D-148K

#used
75E-108W-141D
```


##doxD, tree
```
/home/projects/Wetlands/sulfur_cycling_analysis/doxD_tree

sed -i -e 's/>\(.*\)/>\1_Ref/g' raw_alg.fasta

cat doxD_4_tree.faa raw_alg.fasta > doxD_4_tree_w_ref.fasta

muscle -in doxD_4_tree_w_ref.fasta -out doxD_4_tree_w_ref_muscle.fasta

trimal -automated1 -keepheader -in doxD_4_tree_w_ref_muscle.fasta -out doxD_4_tree_w_ref_muscle_trim.fasta

fasttree -gamma -lg -boot 1000 <doxD_4_tree_w_ref_muscle_trim.fasta> doxD_4_tree_w_ref_muscle_trim.fasttree

run_treeshrink.py  -t doxD_4_tree_w_ref_muscle_trim.fasttree -m per-gene -o doxD_treeshrink_pergene -a doxD_4_tree_w_ref_muscle_trim.fasta

#try mafft
mafft --maxiterate 1000 --globalpair --thread 6 doxD_4_tree_w_ref.fasta  > doxD_4_tree_w_mafft.fasta

trimal -automated1 -keepheader -in doxD_4_tree_w_mafft.fasta -out doxD_4_tree_w_ref_mafft_trim.fasta

fasttree -gamma -lg -boot 1000 <doxD_4_tree_w_ref_mafft_trim.fasta> doxD_4_tree_w_ref_mafft_trim.fasttree

run_treeshrink.py  -t doxD_4_tree_w_ref_mafft_trim.fasttree -m per-gene -o doxD_treeshrink_pergene_mafft -a doxD_4_tree_w_ref_mafft_trim.fasta


```
