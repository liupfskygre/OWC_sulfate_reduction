
##Annotree aprA and aprB
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/aprAB_w_annotree_tree/aprA_B_link_annotree


aprA_ba_ar_anntree_pos_hits.fasta:1103
aprB_ba_ar_anntree_pos_hits.fasta:1008

1103 aprA_user.out.top_positive_hits.txt
1008 aprB_user.out.top_positive_hits.txt
```
#way 2, merge with MAGs name and gene name first, if gene number different is |1|, keep the sequence, add a new sequence names and replace the old name
```
#edit the gene name first
sed -e 's/\(.*\)_/\1\t/g' aprA_user.out.top_positive_hits.txt >aprA_user.out.top_positive_hits_fix.txt

sed -e 's/\(.*\)_/\1\t/g' aprB_user.out.top_positive_hits.txt >aprB_user.out.top_positive_hits_fix.txt

R
setwd("./")
aprA <- read.delim("aprA_user.out.top_positive_hits_fix.txt",header=T, check.names = FALSE)

aprB <- read.delim("aprB_user.out.top_positive_hits_fix.txt",header=T, check.names = FALSE)

aprAB<-merge(aprA, aprB, by.x="MAGs_aprA", by.y="MAGs_aprB", all=TRUE)

write.table(aprAB, 'aprAB_merge_gene_name.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")

#kept, with same MAGs+scaffold and geneID differece==abs(1)

#give a consensus name to both aprA and aprB

#check sequence annotated as aprA&B

#and also sequence with one aprA but two aprB in one line aprB-A-B
#checked with suspected backuped

#pullseq with both aprA and aprB
```
aprA_ba_ar_anntree_pos_hits.fasta:1103
aprB_ba_ar_anntree_pos_hits.fasta:1008

pullseq -n aprA_filtered_w_aprB -i aprA_ba_ar_anntree_pos_hits.fasta > aprA_filtered_w_aprB.fas

pullseq -n aprB_filtered_w_aprA -i aprB_ba_ar_anntree_pos_hits.fasta > aprB_filtered_w_aprA.fas

#change to consencus name with seqkit
seqkit replace -p '(.+)$' -r '{kv}' -k aprA_alias.txt aprA_filtered_w_aprB.fas > aprA_filtered_consensus.fas
seqkit replace -p '(.+)$' -r '{kv}' -k aprB_alias.txt aprB_filtered_w_aprA.fas > aprB_filtered_consensus.fas


sed -i -e 's/\./____/g' aprA_filtered_consensus.fas
sed -i -e 's/\./____/g' aprB_filtered_consensus.fas

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto aprA_filtered_consensus.fas > aprAf_ba_ar_anntree_hits_align.fasta
/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto aprB_filtered_consensus.fas > aprBf_ba_ar_anntree_hits_align.fasta


#run add aprA, aprB, trimal and run fasttree
```


```
```

#prepare sequence tax for concate sequence
```
nano aprAB_concat_geneID_MAGsID.txt
cat aprAB_concat_geneID_MAGsID.txt|cut -f1 -d$'\t'|sort|uniq  > aprAB_concat_MAGsID.txt
grep -w -f aprAB_concat_MAGsID.txt ../../ar_ba_taxonomy_r95.tsv >aprAB_concat_MAGsID_tax.txt


##merge in R

R
setwd("./")
aprAB_tax <- read.delim("aprAB_concat_MAGsID_tax.txt",header=T, check.names = FALSE)

aprAB_ID <- read.delim("aprAB_concat_geneID_MAGsID.txt",header=T, check.names = FALSE)

aprAB_ID_tax<-merge(aprAB_ID, aprAB_tax, by.x="MAGs_ID", by.y="MAGsID", all=TRUE)

write.table(aprAB_ID_tax, 'aprAB_ID_tax_annotree.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")

#after merge from R
#prepare nodes label
#,-1,#000000,normal,1,0


cat aprAB_ID_tax_annotree.txt|cut -f2,3 -d$'\t'|sed -e 's/\t/,/g' - >aprAB_ID_tax_annotree_R95.txt
sed -i -e 's/\n$/,-1,#000000,normal,1,0\n/g' aprAB_ID_tax_annotree_R95.txt

cat ../../itol_dataset_text_template_head.txt aprAB_ID_tax_annotree_R95.txt >itol_aprAB_tax_concate_annotree_R95_label.txt

```
