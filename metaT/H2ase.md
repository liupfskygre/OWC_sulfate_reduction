## expression level of H2ase in 1712 MAGs 

## KO list of hydroogenase
```
cd /home/projects/Wetlands/sulfur_cycling_analysis
mkdir H2ase 
cd H2ase

nano H2ase_KO.txt

H2ase_KO.txt
```


#get H2ase coding genes of all MAGs, then TPM valuse across all samples
```
/home/projects/Wetlands/sulfur_cycling_analysis/H2ase

grep -w -f H2ase_KO.txt ../CAZY_metaT/combined_annotations_1712MAGs_bac.tsv|cut -f1 -d$'\t' > 1712MAGs_bac_H2_cycling_gene.txt

grep -w -f 1712MAGs_bac_H2_cycling_gene.txt /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt > OWC2014-2018_Bac1712_H2_cycle_TPM.txt


```
