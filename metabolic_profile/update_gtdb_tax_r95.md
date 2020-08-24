##update 1712 MAGs info to gtdbtk r95

#
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/genom_annotation

R
setwd("./")
A <- read.delim("all_bins_combined_3217db_checkM_and_gtdbtk_rel95.txt",header=T, check.names = FALSE)

B <- read.delim("Bacteria_Sulfure_genes_TPM.txt",header=T, check.names = FALSE)

AB<-merge(A, B, by.x="Bin_Id", by.y="fasta", all.x=F,all.y=T)

write.table(AB, 'Bacteria_Sulfure_genes_TPM_w_r95tax.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")

```
