##

## sdo tree from MAGs

```
#sdo
../pfam_KO_to_seqs_OWC3211.sh sdo K17725 na_na na_na 184

#415
```
#with caution

```
#sdo_annotree_hits 

cd /home/projects/Wetlands/sulfur_cycling_analysis/sdo_tree

cat sdo_bac_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,8 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > sdo_annotree_hits.fasta

#reference
cat sdo_annotree_hits.fasta sdo_4_tree.faa >sdo_4_tree_w_ref.fasta

mafft --auto sdo_4_tree_w_ref.fasta > sdo_4_tree_w_ref_align.fasta

muscle -in sdo_4_tree_w_ref_align.fasta -out sdo_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in sdo_4_tree_w_ref_align_refine.fasta -out sdo_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <sdo_4_tree_w_ref_align_refine_trim.fasta> sdo_4_raw.fasttree

run_treeshrink.py  -t sdo_4_raw.fasttree -m per-gene -o sdo_treeshrink_pergene -a sdo_4_tree_w_ref_align_refine_trim.fasta



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
