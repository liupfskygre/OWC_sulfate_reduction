## phsA tree from MAGs


## update
```
#phsA
../pfam_KO_to_seqs_OWC3211.sh phsA K08352 na_na na_na 569
#756

```

## tree
```
#from annotree KO search
#phsA_annotree_hits 

cd /home/projects/Wetlands/sulfur_cycling_analysis/phsA_tree

cat phsA_bac_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,8 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > phsA_annotree_hits.fasta

#reference
cat phsA_annotree_hits.fasta phsA_4_tree.faa >phsA_4_tree_w_ref.fasta

mafft --auto phsA_4_tree_w_ref.fasta > phsA_4_tree_w_ref_align.fasta

muscle -in phsA_4_tree_w_ref_align.fasta -out phsA_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in phsA_4_tree_w_ref_align_refine.fasta -out phsA_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <phsA_4_tree_w_ref_align_refine_trim.fasta> phsA_4_raw.fasttree

run_treeshrink.py  -t phsA_4_raw.fasttree -m per-gene -o phsA_treeshrink_pergene -a phsA_4_tree_w_ref_align_refine_trim.fasta

```

#conserved residue checking
```
cat phsA_4_tree.faa uniprot_cluster_P37600.fasta > phsA_4_tree_uniprot_cluster_P37600.fasta

/Users/pengfeiliu/software/mafft-mac/mafftdir/bin/mafft phsA_4_tree_uniprot_cluster_P37600.fasta >phsA_4_tree_uniprot_cluster_P37600_mafft.fasta

#~700 sequences left

phsA_4_tree_final.fasta

grep '>' phsA_4_tree_final.fasta |sed 's/>//g' - >phsA_4_tree_gene.txt 

```

## 

```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir phsA_tree
cd phsA_tree
#seqs list
grep -i 'phsA' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >phsA_gene_list.txt
cat phsA_gene_list.txt|sort|uniq|wc -l
#822 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n phsA_gene_list.txt>phsA_gene_aa.faa
#822  grep -c '>' phsA_gene_aa.faa 
sed -e 's/ #.*$//g' phsA_gene_aa.faa>phsA_gene_aa_fixheader.faa


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n phsA_gene_list.txt>phsA_gene_nt.fna
#822; grep -c '>' phsA_gene_nt.fna 
sed -e 's/ #.*$//g' phsA_gene_nt.fna>phsA_gene_nt_fixheader.fna

#eggnong confirmation
```
