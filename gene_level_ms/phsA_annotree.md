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
