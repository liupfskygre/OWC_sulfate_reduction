## 


## gene list
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Data_analysis_sulfur_cycling/Confirmed_marker_gene_hits
wc -l all_22_marker_gene_final.list
    5167 all_22_marker_gene_final.list


cat all_22_marker_gene_final.list |sort|uniq|wc -l
    5166 #there is one dsrAB gene fusion, others are great


```

##bin list
```
grep -w -f all_22_marker_gene_final.list  /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_annotations_3211db.tsv > all_22_marker_gene_final_dram_annotation.tsv 

```
