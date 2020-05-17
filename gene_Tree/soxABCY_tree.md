## 

## update
```
#soxA
../pfam_KO_to_seqs_OWC3211.sh soxA na_na K17222 TIGR04484 159
#361

#soxB
../pfam_KO_to_seqs_OWC3211.sh soxB na_na K17224 TIGR04486 417
#
#soxC
../pfam_KO_to_seqs_OWC3211.sh soxC na_na K17225 TIGR04555 306
182

#soxY
../pfam_KO_to_seqs_OWC3211.sh soxY PF13501 K17226 TIGR04488 112
506

```

## soxA tree
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/soxB_tree

cat soxA_bac_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,5 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > soxA_annotree_hits.fasta

cd-hit -i soxA_annotree_hits.fasta -o soxA_annotree_hits_09.fasta

pullseq -i soxA_annotree_hits_09.fasta -m 159 > soxA_annotree_hits_09_length.fasta

#reference
cat soxA_annotree_hits_09_length.fasta soxA_4_tree.faa >soxA_4_tree_w_ref.fasta

mafft --auto soxA_4_tree_w_ref.fasta > soxA_4_tree_w_ref_align.fasta

muscle -in soxA_4_tree_w_ref_align.fasta -out soxA_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in soxA_4_tree_w_ref_align_refine.fasta -out soxA_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <soxA_4_tree_w_ref_align_refine_trim.fasta> soxA_4_raw.fasttree

run_treeshrink.py  -t soxA_4_raw.fasttree -m per-gene -o soxA_treeshrink_pergene -a soxA_4_tree_w_ref_align_refine_trim.fasta
```


## soxB tree
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/soxB_tree

cat soxB_bac_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,5 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > soxB_annotree_hits.fasta

cd-hit -i soxB_annotree_hits.fasta -o soxB_annotree_hits_09.fasta

pullseq -i soxB_annotree_hits_09.fasta -m 417 > soxB_annotree_hits_09_length.fasta

#reference
cat soxB_annotree_hits_09_length.fasta soxB_4_tree.faa >soxB_4_tree_w_ref.fasta

mafft --auto soxB_4_tree_w_ref.fasta > soxB_4_tree_w_ref_align.fasta

muscle -in soxB_4_tree_w_ref_align.fasta -out soxB_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in soxB_4_tree_w_ref_align_refine.fasta -out soxB_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <soxB_4_tree_w_ref_align_refine_trim.fasta> soxB_4_raw.fasttree

run_treeshrink.py  -t soxB_4_raw.fasttree -m per-gene -o soxB_treeshrink_pergene -a soxB_4_tree_w_ref_align_refine_trim.fasta
```

## soxC tree
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/soxB_tree

cat soxC_bac_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,5 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > soxC_annotree_hits.fasta

cd-hit -i soxC_annotree_hits.fasta -o soxC_annotree_hits_09.fasta

pullseq -i soxC_annotree_hits_09.fasta -m 306 > soxC_annotree_hits_09_length.fasta

#reference
cat soxC_annotree_hits_09_length.fasta soxC_4_tree.faa >soxC_4_tree_w_ref.fasta

mafft --auto soxC_4_tree_w_ref.fasta > soxC_4_tree_w_ref_align.fasta

muscle -in soxC_4_tree_w_ref_align.fasta -out soxC_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in soxC_4_tree_w_ref_align_refine.fasta -out soxC_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <soxC_4_tree_w_ref_align_refine_trim.fasta> soxC_4_raw.fasttree

run_treeshrink.py  -t soxC_4_raw.fasttree -m per-gene -o soxC_treeshrink_pergene -a soxC_4_tree_w_ref_align_refine_trim.fasta
```


## soxY tree
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/soxB_tree

cat soxY_Bac_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,5 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > soxY_annotree_hits.fasta

cd-hit -i soxY_annotree_hits.fasta -o soxY_annotree_hits_09.fasta

pullseq -i soxY_annotree_hits_09.fasta -m 112 > soxY_annotree_hits_09_length.fasta

#reference
cat soxY_annotree_hits_09_length.fasta soxY_4_tree.faa >soxY_4_tree_w_ref.fasta

mafft --auto soxY_4_tree_w_ref.fasta > soxY_4_tree_w_ref_align.fasta

muscle -in soxY_4_tree_w_ref_align.fasta -out soxY_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in soxY_4_tree_w_ref_align_refine.fasta -out soxY_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <soxY_4_tree_w_ref_align_refine_trim.fasta> soxY_4_raw.fasttree

run_treeshrink.py  -t soxY_4_raw.fasttree -m per-gene -o soxY_treeshrink_pergene -a soxY_4_tree_w_ref_align_refine_trim.fasta
```


## soxB seqs from MAGs

##
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir soxB_tree
cd soxB_tree
#seqs list
grep -i 'soxB' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >soxB_gene_list.txt
cat soxB_gene_list.txt|sort|uniq|wc -l
#249 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n soxB_gene_list.txt>soxB_gene_aa.faa
#249  grep -c '>' soxB_gene_aa.faa 


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n soxB_gene_list.txt>soxB_gene_nt.fna
#248; grep -c '>' soxB_gene_nt.fna 
```

##soxC
```
#seqs list
grep -i 'soxC' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >soxC_gene_list.txt
cat soxC_gene_list.txt|sort|uniq|wc -l
#180 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n soxC_gene_list.txt>soxC_gene_aa.faa
#180  grep -c '>' soxC_gene_aa.faa 
sed -e 's/ #.*$//g' soxC_gene_aa.faa>soxC_gene_aa_fixheader.faa


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n soxC_gene_list.txt>soxC_gene_nt.fna
#180; grep -c '>' soxC_gene_nt.fna 
sed -e 's/ #.*$//g' soxC_gene_nt.fna>soxC_gene_nt_fixheader.fna

```

##soxA
```
#seqs list
grep -i 'soxA' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >soxA_gene_list.txt
cat soxA_gene_list.txt|sort|uniq|wc -l
#822 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n soxA_gene_list.txt>soxA_gene_aa.faa
#822  grep -c '>' soxA_gene_aa.faa 
sed -e 's/ #.*$//g' soxA_gene_aa.faa>soxA_gene_aa_fixheader.faa


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n soxA_gene_list.txt>soxA_gene_nt.fna
#822; grep -c '>' soxA_gene_nt.fna 
sed -e 's/ #.*$//g' soxA_gene_nt.fna>soxA_gene_nt_fixheader.fna
```
