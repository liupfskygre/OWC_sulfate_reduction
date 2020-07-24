##


##
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrD_w_annotree_gene_tree

awk -F ',' '{print ">"$2"___"$1"\n"$3}' annotree_hits_dsrD_sel.csv > annotree_hits_dsrD_sel.fasta
#CDD search verified


```
