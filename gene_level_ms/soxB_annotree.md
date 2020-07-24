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
