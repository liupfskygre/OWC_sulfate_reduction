##1, tree-->arb-->tax assignment

##2, add MAGs info to contigs within MAGs, checking tax assignment of arb and other ways.

#with r95 info updated
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/genom_annotation

R
setwd("./")
A <- read.delim("all_bins_combined_3217db_checkM_and_gtdbtk_rel95.txt",header=T, check.names = FALSE)

B <- read.delim("contigs_in_MAGs_sulfur.txt",header=T, check.names = FALSE)

AB<-merge(A, B, by.x="Bin_Id", by.y="Bin", all.x=F,all.y=T)

write.table(AB, 'contigs_4902_sulfur_gene_wR95.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")



R
setwd("./")
A <- read.delim("OWC_genes_TPM_w_anntree_w_arb_tax16Aug2020.txt",header=T, check.names = FALSE)

B <- read.delim("contigs_4902_sulfur_gene_wR95.txt",header=T, check.names = FALSE)

AB<-merge(A, B, by.x="Scaffolds", by.y="Contigs_short", all.x=T,all.y=F)

write.table(AB, 'OWC_4902_TPM_w_anntree_w_arb_tax16Aug2020_W_r95contigs.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")
```

## notes, 
```
1, contigs_4902_sulfur_gene were generated by search contigs id to all genes in our DB3211

2, while the blast db is generated by filtered MAGs, since the I do search key marker genes in MAGs with lenght filtering,
therefore some of the cotigs in MAGs were not in the search DB, since they are short, but they do in the contigs based search, 
no lenght but only score were applied in METABOLIC.


```