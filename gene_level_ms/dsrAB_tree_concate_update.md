##


##Annotree dsrA and dsrB
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrAB_tree_with_Annotree_ref/dsrA_B_link_annotree

dsrA_ba_ar_anntree_hits.fasta:889
dsrB_ba_ar_anntree_hits.fasta:858
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

#check sequence annotated as dsrA&B 

#and also sequence with one dsrA but two dsrB in one line dsrB-A-B
#checked with suspected backuped

#
```


#pullseq with both dsrA and dsrB
```
pullseq -n dsrA_filtered_w_dsrB -i dsrA_ba_ar_anntree_hits.fasta > dsrA_filtered_w_dsrB.fas

pullseq -n dsrB_filtered_w_dsrA -i dsrB_ba_ar_anntree_hits.fasta > dsrB_filtered_w_dsrA.fas

#change to consencus name with seqkit
seqkit replace -p '(.+)$' -r '{kv}' -k dsrA_alias.txt dsrA_filtered_w_dsrB.fas > dsrA_filtered_consensus.fas
seqkit replace -p '(.+)$' -r '{kv}' -k dsrB_alias.txt dsrB_filtered_w_dsrA.fas > dsrB_filtered_consensus.fas


sed -i -e 's/\./____/g' dsrA_filtered_consensus.fas
sed -i -e 's/\./____/g' dsrB_filtered_consensus.fas

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto dsrA_filtered_consensus.fas > dsrAf_ba_ar_anntree_hits_align.fasta
/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto dsrB_filtered_consensus.fas > dsrBf_ba_ar_anntree_hits_align.fasta

sed
#phyloSuit to concate, and checing missing genes
sed -i -e 's/____/\./g' dsrAB_annotree_concatenation_partI.fas

```

#prepare sequence tax for concate sequence
```
nano dsrAB_concat_geneID_MAGsID.txt
cat dsrAB_concat_geneID_MAGsID.txt|cut -f1 -d$'\t'|sort|uniq  > dsrAB_concat_MAGsID.txt
grep -w -f dsrAB_concat_MAGsID.txt ../../ar_ba_taxonomy_r95.tsv >dsrAB_concat_MAGsID_tax.txt


##merge in R

R
setwd("./")
dsrAB_tax <- read.delim("dsrAB_concat_MAGsID_tax.txt",header=T, check.names = FALSE) 

dsrAB_ID <- read.delim("dsrAB_concat_geneID_MAGsID.txt",header=T, check.names = FALSE) 

dsrAB_ID_tax<-merge(dsrAB_ID, dsrAB_tax, by.x="MAGs_ID", by.y="MAGsID", all=TRUE)

write.table(dsrAB_ID_tax, 'dsrAB_ID_tax_annotree.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")

#after merge from R
#prepare nodes label
#,-1,#000000,normal,1,0


cat dsrAB_ID_tax_annotree.txt|cut -f2,3 -d$'\t'|sed -e 's/\t/,/g' - >dsrAB_ID_tax_annotree_R95.txt
sed -i -e 's/$/,-1,#000000,normal,1,0/g' dsrAB_ID_tax_annotree_R95.txt

cat ../../itol_dataset_text_template_head.txt dsrAB_ID_tax_annotree_R95.txt >itol_dsrAB_tax_concate_annotree_R95_label.txt

```


#selected part sequence from AK
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrAB_tree_with_Annotree_ref/sequences_AKarthik
grep -i -E 'Hydrotherm|rokubacteria|zixi|nitrospinae|plant|Acido|Actino|Verrucom' dsrAB.concatenated.alignments.v9_header.txt >dsrAB.concatenated.alignments.v9_header_sel.txt

pullseq -i dsrAB.concatenated.alignments.v9.fasta -n  dsrAB.concatenated.alignments.v9_header_sel.txt>dsrAB.concatenated.alignments.v9_header_sel.fasta
```



#mafft add sequence to alignment
```
Full_length_seq_dsrAB_ref_fixH.faa
dsrAB.concatenated.alignments.v9_header_sel.fasta
dsrAB_annotree_concatenation815.fas

cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrAB_tree_with_Annotree_ref/dsrA_B_link_annotree

/Users/pengfeiliu/software/mafft-mac/mafftdir/bin/mafft --auto --add dsrAB_annotree_concatenation815.fas --thread 4 Full_length_seq_dsrAB_ref_fixH.faa >dsrAB_anno_tree_alignment1.faa

/Users/pengfeiliu/software/mafft-mac/mafftdir/bin/mafft --auto --add dsrAB.concatenated.alignments.v9_header_sel.fasta --thread 4 dsrAB_anno_tree_alignment1.faa >dsrAB_anno_tree_alignment2.faa

/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in dsrA_anno_tree_alignment.faa -out dsrA_anno_tree_alignment_trmal.fasta

```








#first try log
```
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


#recovered missing genes, partI 

dsrBwithout_A.txt

```
