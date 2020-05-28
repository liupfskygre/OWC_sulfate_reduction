## checking the Nitrogen cycling capacity of sulfur cycling MAGs

```
KEGG based filteing of genes to TPM changes of 
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir N_cycling

```

#KO number to get all gene list of N-cycling genes
```
grep -w -f N_cycling_KO.txt ../CAZY_metaT/combined_annotations_1712MAGs_bac.tsv|cut -f1 -d$'\t' > 1712MAGs_bac_N_cycling_gene.txt
```

#using gene list to get TPM values of those genes
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/N_cycling


grep -w -f 1712MAGs_bac_N_cycling_gene.txt /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt > OWC2014-2018_Bac1712_N_cycle_TPM.txt

#fix header
head -1 /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt> header 
cat header OWC2014-2018_Bac1712_N_cycle_TPM.txt > OWC2014-2018_Bac1712_N_cycle_TPM_h.txt

```

#summary TPM based on tax and functional categories
```

```
