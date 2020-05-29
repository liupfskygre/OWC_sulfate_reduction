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


#add this three into vhoACT
K14068, K14069, K14070 # all zero


##fix header
head -1 /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt> header

cat header OWC2014-2018_Bac1712_H2_cycle_TPM.txt > OWC2014-2018_Bac1712_H2_cycle_TPM_h.txt


##add tax info to TPM values


python /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/joint_htseq_output.py OWC2014-2018_Bac1712_H2_cycle_TPM_h.txt ../marker_gene_confirmation_final/MAGs1761_w_sulfur_Gene_annotation_fix.txt > OWC2014-2018_Bac1712_H2ase_cycle_TPM_w_annotation.txt

awk -F '\t' '{print NF; exit}' OWC2014-2018_Bac1712_H2ase_cycle_TPM_w_annotation.txt

cat  OWC2014-2018_Bac1712_H2ase_cycle_TPM_w_annotation.txt|cut -f1-128,129,153 -d$'\t'>OWC2014-2018_Bac1712_H2ase_cycle_TPM_w_tax.txt

```
