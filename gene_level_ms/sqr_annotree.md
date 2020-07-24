##

#sqr_annotree
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/sqr_annotree_gene_tree

sqr_bac_annotree_hits.csv

#soxB, search by tigrfam, 
awk -F ',' '{print ">"$4"___"$3"\n"$8}' sqr_bac_annotree_hits.csv > sqr_ba_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' sqr_ba_anntree_hits.fasta

sed -i -e '/sequence/d' sqr_ba_anntree_hits.fasta


```

```
grep -c '>' sqr_ba_anntree_hits.fasta

grep 'K17218' sqr_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > sqr_user.out.top_positive_hits.txt
pullseq -i sqr_ba_anntree_hits.fasta -n sqr_user.out.top_positive_hits.txt > sqr_ba_anntree_pos_hits.fasta

grep -c 'K17218' sqr_user.out.top.txt
grep -c '>' sqr_ba_anntree_pos_hits.fasta

#6642
```
