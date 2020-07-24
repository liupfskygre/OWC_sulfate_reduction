##

#sdo
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/sdo_annotree_gene_tree

#sdo
cat sdo_arc_annotree_hits.csv sdo_bac_annotree_hits.csv > sdo_ba_ar_anntree_hits.csv


#sdo, search by KO, 
awk -F ',' '{print ">"$4"___"$3"\n"$8}' sdo_ba_ar_anntree_hits.csv > sdo_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' sdo_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' sdo_ba_ar_anntree_hits.fasta



```
