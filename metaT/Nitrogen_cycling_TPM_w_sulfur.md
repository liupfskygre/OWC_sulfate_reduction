## checking the Nitrogen cycling capacity of sulfur cycing MAGs

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


```

#summary TPM based on tax and functional categories
```

```
