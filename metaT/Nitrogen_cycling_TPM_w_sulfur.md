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

#1761 genomes from w sulfur cycling genes
```
fix annotation
../marker_gene_confirmation_final/MAGs1761_w_sulfur_Gene_annotation.txt
head -1 MAGs1761_w_sulfur_Gene_annotation.txt > header.txt
grep -v 'fasta' MAGs1761_w_sulfur_Gene_annotation.txt  > tpm.txt 

#5786088 MAGs1761_w_sulfur_Gene_annotation.txt


cat header.txt tpm.txt > MAGs1761_w_sulfur_Gene_annotation_fix.txt
# 5784328 MAGs1761_w_sulfur_Gene_annotation_fix.txt ##1760. 

sed -i -e 's/\tfasta/Gene_ID\tfasta/1' MAGs1761_w_sulfur_Gene_annotation_fix.txt
rm tpm.txt
```

#summary TPM based on tax and functional categories
```
#add tax info to TPM values


python /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/joint_htseq_output.py OWC2014-2018_Bac1712_N_cycle_TPM_h.txt ../marker_gene_confirmation_final/MAGs1761_w_sulfur_Gene_annotation_fix.txt >OWC2014-2018_Bac1712_N_cycle_TPM_w_annotation.txt

awk -F '\t' '{print NF; exit}' OWC2014-2018_Bac1712_N_cycle_TPM_w_annotation.txt



```
