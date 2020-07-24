##

##
```
#cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/fccB_annotree_gene_tree


fccB_bac_annotree_hits.csv #pfam 

awk -F ',' '{print ">"$4"___"$3"\n"$6}' fccB_bac_annotree_hits.csv > fccB_ba_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' fccB_ba_anntree_hits.fasta

sed -i -e '/sequence/d' fccB_ba_anntree_hits.fasta
```
