## TPM valuess of sulfur cycling key genes

```
#check the expression level of 5168 confirmed sulfur cycling marker genes
 
 
/home/projects/Wetlands/sulfur_cycling_analysis/marker_gene_confirmation_final

  5168 all_22_marker_gene_final.list
grep -w -f /home/projects/Wetlands/sulfur_cycling_analysis/marker_gene_confirmation_final/all_22_marker_gene_final.list /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt > OWC2014-2018_DB3211_Sulfure_genes_TPM.txt

# add gene info and tax info to the end of file

```
