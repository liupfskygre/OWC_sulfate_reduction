##

##

```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/phsA_annotree_gene_tree

#phsA
cat phsA_arc_annotree_hits.csv phsA_bac_annotree_hits.csv > phsA_ba_ar_anntree_hits.csv


#phsA
awk -F ',' '{print ">"$4"___"$3"\n"$8}' phsA_ba_ar_anntree_hits.csv > phsA_ba_ar_anntree_hits.fasta



sed -i -e '/gtdbId___geneId/d' phsA_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' phsA_ba_ar_anntree_hits.fasta


```

```
grep -c '>' phsA_ba_ar_anntree_hits.fasta

grep 'K08352' phsA_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > phsA_user.out.top_positive_hits.txt
pullseq -i phsA_ba_ar_anntree_hits.fasta -n phsA_user.out.top_positive_hits.txt > phsA_ba_ar_anntree_pos_hits.fasta

grep -c 'K08352' phsA_user.out.top.txt
grep -c '>' phsA_ba_ar_anntree_pos_hits.fasta
#1079
```
