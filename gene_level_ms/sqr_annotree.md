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
