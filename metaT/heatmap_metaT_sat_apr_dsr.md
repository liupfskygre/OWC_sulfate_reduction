## get MAGs list with apr, dsr

```
Bac_MAGs_w_apr_dsr_uniq.txt #570
Bac_MAGs_w_apr_dsr_genes.txt
```

## corresponding gene list
```
sat/met3	K00958	TIGR00339
aprA	K00394	TIGR02061
aprB	K00395	TIGR02060
dsrA	K11180	TIGR02064
dsrB	K11181	TIGR02066
dsrD  NA  PF08679.11

qmoA  K16885
qmoB  K16886
qmoC  K16887


```

## get all apr and dsr gene TPMs 
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/

#all TPM 
==>/home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt

sed -i -e 's/gene_id/Gene_ID/g' Bac_MAGs_w_apr_dsr_genes.txt


python /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/joint_htseq_output.py Bac_MAGs_w_apr_dsr_genes.txt /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt > Apr_DSR_TPM.txt
```


## transform in R to make heatmap
```
Apr_DSR_TPM.txt
#y==MAG, aligned with tax
#x==Sample, aligned with sample depth


```
