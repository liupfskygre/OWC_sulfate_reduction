##


##Annotree dsrA and dsrB
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrAB_tree_with_Annotree_ref/dsrA_B_link_annotree

dsrA_ba_ar_anntree_hits.fasta:889
dsrB_ba_ar_anntree_hits.fasta:858

#fix name to have only MAGs_contigs, remove the last # of gene number for concatenate 

#e.g., 1796 --> 179

sed -e 's/;/___/g' dsrA_ba_ar_anntree_hits.fasta >dsrAf_ba_ar_anntree_hits.fasta
sed -e 's/;/___/g' dsrB_ba_ar_anntree_hits.fasta >dsrBf_ba_ar_anntree_hits.fasta

sed -i -e 's/[0-9]$//g' dsrAf_ba_ar_anntree_hits.fasta
sed -i -e 's/[0-9]$//g' dsrBf_ba_ar_anntree_hits.fasta

#fix Phulosuit will change any symbol to _
sed -i -e 's/\./____/g' dsrAf_ba_ar_anntree_hits.fasta
sed -i -e 's/\./____/g' dsrBf_ba_ar_anntree_hits.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto dsrAf_ba_ar_anntree_hits.fasta > dsrAf_ba_ar_anntree_hits_align.fasta
/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto dsrBf_ba_ar_anntree_hits.fasta > dsrBf_ba_ar_anntree_hits_align.fasta

sed
#phyloSuit to concate, and checing missing genes
sed -i -e 's/____/\./g' dsrAB_annotree_concatenation_partI.fas

# above not working well, _19 and _20 will not get concated
```

#recovered missing genes, partI 
```
dsrBwithout_A.txt

```

#way 2, merge with MAGs name and gene name first, if gene number different is |1|, keep the sequence, add a new sequence names and replace the old name
```
#edit the gene name first
sed -e 's/\(.*\)_/\1\t/' dsrA_ba_ar_anntree_hits.txt >dsrA_ba_ar_anntree_hits_fix.txt

sed -e 's/\(.*\)_/\1\t/' dsrB_ba_ar_anntree_hits.txt >dsrB_ba_ar_anntree_hits_fix.txt

R
setwd("./")
dsrA <- read.delim("dsrA_ba_ar_anntree_hits_fix.txt",header=T, check.names = FALSE) 

dsrB <- read.delim("dsrB_ba_ar_anntree_hits_fix.txt",header=T, check.names = FALSE) 

dsrAB<-merge(dsrA, dsrB, by.x="MAGs_dsrA", by.y="MAGs_dsrB", all=TRUE)

write.table(dsrAB, 'dsrAB_merge_gene_name.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")

#kept, with same MAGs+scaffold and geneID differece==abs(1)

#give a consensus name to both dsrA and dsrB

#check sequence annotated as dsrA&B and also sequence with one dsrA but two dsrB in one line dsrB-A-B
```

