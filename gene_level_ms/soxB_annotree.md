##

##
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/soxB_annotree_gene_tree
#soxB
cat soxB_arc_annotree_hits.csv soxB_bac_annotree_hits.csv > soxB_ba_ar_anntree_hits.csv


#soxB, search by tigrfam, 
awk -F ',' '{print ">"$4"___"$3"\n"$5}' soxB_ba_ar_anntree_hits.csv > soxB_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' soxB_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' soxB_ba_ar_anntree_hits.fasta

```


```
grep -c '>' soxB_ba_ar_anntree_hits.fasta

grep 'K17224' soxB_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > soxB_user.out.top_positive_hits.txt
pullseq -i soxB_ba_ar_anntree_hits.fasta -n soxB_user.out.top_positive_hits.txt > soxB_ba_ar_anntree_pos_hits.fasta

grep -c 'K17224' soxB_user.out.top.txt
grep -c '>' soxB_ba_ar_anntree_pos_hits.fasta


```
