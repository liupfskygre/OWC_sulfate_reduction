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
