##



##
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/aprAB_w_annotree_tree

#aprA
cat aprA_arc_annotree_hits.csv aprA_bac_annotree_hits.csv > aprA_ba_ar_anntree_hits.csv

#aprB 
cat aprB_arc_annotree_hits.csv aprB_bac_annotree_hits.csv > aprB_ba_ar_anntree_hits.csv

#aprA
awk -F ',' '{print ">"$4"___"$3"\n"$5}' aprA_ba_ar_anntree_hits.csv > aprA_ba_ar_anntree_hits.fasta

awk -F ',' '{print ">"$4"___"$3"\n"$5}' aprB_ba_ar_anntree_hits.csv > aprB_ba_ar_anntree_hits.fasta

sed -i -e '/gtdbId___geneId/d' aprA_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' aprA_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' aprB_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' aprB_ba_ar_anntree_hits.fasta
```

#submit to GhostKAAS
```
#https://www.kegg.jp/ghostkoala/
#one per time
#download
==> Annotation data: user_ko.txt
==> Taxonomy data: user.out.top.txt ==>this one is the tax


https://www.ncbi.nlm.nih.gov/Structure/bwrpsb/bwrpsb.cgi

```

#filter positive hits from ghostkoala analysis
```
#aprA
grep -c '>' aprA_ba_ar_anntree_hits.fasta #1104
grep 'K00394' aprA_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > aprA_user.out.top_positive_hits.txt
pullseq -i aprA_ba_ar_anntree_hits.fasta -n aprA_user.out.top_positive_hits.txt > aprA_ba_ar_anntree_pos_hits.fasta

#aprB
grep -c '>' aprB_ba_ar_anntree_hits.fasta
grep 'K00395' aprB_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > aprB_user.out.top_positive_hits.txt
pullseq -i aprB_ba_ar_anntree_hits.fasta -n aprB_user.out.top_positive_hits.txt > aprB_ba_ar_anntree_pos_hits.fasta
```
