##

```
#
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/soeA_annotree_gene_tree

#soeA
cat soeA_arc_annotree_hits.csv soeA_bac_annotree_hits.csv > soeA_ba_ar_anntree_hits.csv


#soeA, search by KO, 
awk -F ',' '{print ">"$4"___"$3"\n"$8}' soeA_ba_ar_anntree_hits.csv > soeA_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' soeA_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' soeA_ba_ar_anntree_hits.fasta


```
#filter positive hits from ghostkoala analysis
```
grep -c '>' soeA_ba_ar_anntree_hits.fasta

grep 'K21307' soeA_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > soeA_user.out.top_positive_hits.txt
pullseq -i soeA_ba_ar_anntree_hits.fasta -n soeA_user.out.top_positive_hits.txt > soeA_ba_ar_anntree_pos_hits.fasta

grep -c 'K21307' soeA_user.out.top.txt
grep -c '>' soeA_ba_ar_anntree_pos_hits.fasta 
#1101
```
