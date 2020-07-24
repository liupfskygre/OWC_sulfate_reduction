#


#soxC
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/soxC_annotree_gene_tree

cat soxC_arc_annotree_hits.csv soxC_bac_annotree_hits.csv > soxC_ba_ar_anntree_hits.csv


#soxC, search by KO, 
awk -F ',' '{print ">"$4"___"$3"\n"$5}' soxC_ba_ar_anntree_hits.csv > soxC_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' soxC_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' soxC_ba_ar_anntree_hits.fasta
```

```
grep -c '>' soxC_ba_ar_anntree_hits.fasta

grep 'K17225' soxC_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > soxC_user.out.top_positive_hits.txt
pullseq -i soxC_ba_ar_anntree_hits.fasta -n soxC_user.out.top_positive_hits.txt > soxC_ba_ar_anntree_pos_hits.fasta

grep -c 'K17225' soxC_user.out.top.txt
grep -c '>' soxC_ba_ar_anntree_pos_hits.fasta
#1840
```
