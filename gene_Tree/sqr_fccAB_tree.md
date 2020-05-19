##

```
#sqr
../pfam_KO_to_seqs_OWC3211.sh sqr na_na K17218 na_na 311
#1450


#fccB
../pfam_KO_to_seqs_OWC3211.sh fccB na_na K17229 na_na 258
#use 60% of typical fccB Q06530, 430*0.6 ==258
??? mixed with Pyridine nucleotide-disulfide oxidoreductase???
389
```

##tree of fccB
```

fccB_4_tree.faa

#ref from uniref
fccB_uniref100_q06530_id50.fasta
sed -e 's/ /replace/1' fccB_uniref100_q06530_id50.fasta >fccB_uniref.fasta
sed -i -e 's/replace.*$/_Ref/g' fccB_uniref.fasta


#from annotree KO search
#fccB_annotree_hits 


cat fccB_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,8 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > fccB_annotree_hits.fasta

#reference
cat fccB_annotree_hits.fasta fccB_uniref.fasta fccB_4_tree.faa >fccB_4_tree_w_ref.fasta

mafft --auto fccB_4_tree_w_ref.fasta > fccB_4_tree_w_ref_align.fasta

muscle -in fccB_4_tree_w_ref_align.fasta -out fccB_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in fccB_4_tree_w_ref_align_refine.fasta -out fccB_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <fccB_4_tree_w_ref_align_refine_trim.fasta> fccB_4_raw.fasttree

run_treeshrink.py  -t fccB_4_raw.fasttree -m per-gene -o fccB_treeshrink_pergene -a fccB_4_tree_w_ref_align_refine_trim.fasta
```

#fccB residue checking, based on the uniref ref
```
cat fccB_4_tree.faa fccB_Q06530_Ref.fasta q06530_fccB_Ref.fasta > fccB_4_tree_w_uni_Ref.faa
/Users/pengfeiliu/software/mafft-mac/mafftdir/bin/mafft fccB_4_tree_w_uni_Ref.faa > fccB_4_tree_w_uni_Ref_align.faa



fccB_4_tree_w_uni_Ref_align_removed.faa

grep '>' fccB_4_tree_final.fasta |sed 's/>//g' - > fccB_4_tree_gene.txt 

```


##tree of sqr
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/sqr_tree

cat sqr_bac_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,8 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > sqr_annotree_hits.fasta
cd-hit -i sqr_annotree_hits.fasta -o sqr_annotree_hits_09.fasta

pullseq -i sqr_annotree_hits_09.fasta -m 311 > sqr_annotree_hits_09_length.fasta

#reference
cat sqr_annotree_hits_09_length.fasta sqr_4_tree.faa >sqr_4_tree_w_ref.fasta

mafft --auto sqr_4_tree_w_ref.fasta > sqr_4_tree_w_ref_align.fasta

muscle -in sqr_4_tree_w_ref_align.fasta -out sqr_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in sqr_4_tree_w_ref_align_refine.fasta -out sqr_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <sqr_4_tree_w_ref_align_refine_trim.fasta> sqr_4_raw.fasttree

run_treeshrink.py  -t sqr_4_raw.fasttree -m per-gene -o sqr_treeshrink_pergene -a sqr_4_tree_w_ref_align_refine_trim.fasta
```

## sqr: recheck  the tree of based on eggnog reference, residue checking
```
sqr_4_tree_07SA8_ref_mafft_align.faa
muscle -in sqr_4_tree_07SA8_ref_mafft_align.faa -out sqr_4_tree_07SA8_ref_mafft_align_refine.faa

3C,
after removing sequences without 3 conserved redidues, 382 left

sqr_4_tree_07SA8_ref_mafft_align_remove.faa
sqr_4_tree_final.fasta

grep '>' sqr_4_tree_final.fasta |sed 's/>//g' - > sqr_4_tree_gene.txt 

```



##sqr seqs from MAGs
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir sqr_tree
cd sqr_tree
#seqs list
grep -i 'sqr' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >sqr_gene_list.txt
cat sqr_gene_list.txt|sort|uniq|wc -l
#1518 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n sqr_gene_list.txt>sqr_gene_aa.faa
#1518;  grep -c '>' sqr_gene_aa.faa 
sed -e 's/ #.*$//g' sqr_gene_aa.faa >sqr_gene_aa_fixheader.faa 

#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n sqr_gene_list.txt>sqr_gene_nt.fna
#1517; grep -c '>' sqr_gene_nt.fna 

sed -e 's/ #.*$//g' sqr_gene_nt.fna >sqr_gene_nt_fixheader.fna
```

##fccA
```
#seqs list
grep -i 'fccA' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >fccA_gene_list.txt
cat fccA_gene_list.txt|sort|uniq|wc -l
#560 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n fccA_gene_list.txt>fccA_gene_aa.faa
#560;  grep -c '>' fccA_gene_aa.faa 
sed -e 's/ #.*$//g' fccA_gene_aa.faa >fccA_gene_aa_fixheader.faa 

#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n fccA_gene_list.txt>fccA_gene_nt.fna
#559; grep -c '>' fccA_gene_nt.fna 
sed -e 's/ #.*$//g' fccA_gene_nt.fna>fccA_gene_nt_fixheader.fna

##fccB
#seqs list
grep -i 'fccB' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >fccB_gene_list.txt
cat fccB_gene_list.txt|sort|uniq|wc -l
#453 

#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n fccB_gene_list.txt>fccB_gene_aa.faa
#1518;  grep -c '>' fccB_gene_aa.faa 
sed -e 's/ #.*$//g' fccB_gene_aa.faa >fccB_gene_aa_fixheader.faa 


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n fccB_gene_list.txt>fccB_gene_nt.fna
#452; grep -c '>' fccB_gene_nt.fna 
sed -e 's/ #.*$//g' fccB_gene_nt.fna >fccB_gene_nt_fixheader.fna

```
