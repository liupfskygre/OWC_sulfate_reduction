##

##
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/soxA_annotree_gene

cat soxA_arc_annotree_hits.csv soxA_bac_annotree_hits.csv > soxA_ba_ar_anntree_hits.csv


#soxA, search by tigrfam, 
awk -F ',' '{print ">"$4"___"$3"\n"$5}' soxA_ba_ar_anntree_hits.csv > soxA_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' soxA_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' soxA_ba_ar_anntree_hits.fasta

```

```
grep -c '>' soxA_ba_ar_anntree_hits.fasta

grep 'K17222' soxA_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > soxA_user.out.top_positive_hits.txt
pullseq -i soxA_ba_ar_anntree_hits.fasta -n soxA_user.out.top_positive_hits.txt > soxA_ba_ar_anntree_pos_hits.fasta

grep -c 'K17222' soxA_user.out.top.txt
grep -c '>' soxA_ba_ar_anntree_pos_hits.fasta


#soxA

grep 'soxA' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >soxA_clean_geneID.txt
pullseq -i soxA.hmm.collection.faa -n soxA_clean_geneID.txt > soxA.hmm.collection_clean.fasta

grep -c '>' soxA.hmm.collection_clean.fasta
wc -l soxA_clean_geneID.txt
#403
```

#
```
cat soxA_ba_ar_anntree_pos_hits.fasta soxA.hmm.collection_clean.fasta > soxA_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto soxA_owc_clean_wAnnRef.fasta > soxA_owc_clean_wAnnRef_align.fasta
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in soxA_owc_clean_wAnnRef_align.fasta -out soxA_owc_clean_wAnnRef_trimal.fasta

#fasttree
FastTree -gamma -lg -boot 1000 <soxA_owc_clean_wAnnRef_trimal.fasta> soxA_OWC_trmal_fasttree.tree

```


#prepare classification file for aprA and soxB==>r95
```
#on mac, annotree reference
sed -i -e 's/___/;/g' soxA_user.out.top_positive_hits.txt

cut -f1 -d$';' soxA_user.out.top_positive_hits.txt > soxA_ba_ar_annotree_hits_MAGid.txt

#ba-ar
grep -w -f soxA_ba_ar_annotree_hits_MAGid.txt ../ar_ba_taxonomy_r95.tsv >soxA_ba_ar_annotree_hits_MAG_tax.txt

sed -i -e 's/;/___/g' soxA_user.out.top_positive_hits.txt


#edit in excel to get tree id and MAGs id done
#e.g.
#Tree_ID	MAGs_ID	Seq_ID
#GB_GCA_000007225.1___AE009441.1_1796	GB_GCA_000007225.1	AE009441.1_1796

# MAGsID	Tax_full

sed -i '1 i\MAGsID\tTax_full'  soxA_ba_ar_annotree_hits_MAG_tax.txt

cat soxA_user.out.top_positive_hits.txt|awk -F '\t' '{print $0"\t"$1}'|sed -e 's/___/\t/2' - > soxA_geneID_MAGsID.txt
sed -i '1 i\Tree_ID\tMAGs_ID\tSeq_ID' soxA_geneID_MAGsID.txt

##merge in R

R
setwd("./")
soxA_tax <- read.delim("soxA_ba_ar_annotree_hits_MAG_tax.txt",header=T, check.names = FALSE)

soxA_ID <- read.delim("soxA_geneID_MAGsID.txt",header=T, check.names = FALSE)

soxA_ID_tax<-merge(soxA_ID, soxA_tax, by.x="MAGs_ID", by.y="MAGsID", all=TRUE)

write.table(soxA_ID_tax, 'soxA_ID_tax_annotree.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")

#after merge from R
#prepare nodes label
#,-1,#000000,normal,1,0


cat soxA_ID_tax_annotree.txt|cut -f3,5 -d$'\t'|sed -e 's/\t/,/g' - >soxA_ID_tax_annotree_R95.txt
sed -i -e 's/$/,-1,#000000,normal,1,0/g' soxA_ID_tax_annotree_R95.txt
#tree_label

cat ../itol_dataset_text_template_head.txt  soxA_ID_tax_annotree_R95.txt >itol_soxA_tax_annotree_R95_label.txt
```
