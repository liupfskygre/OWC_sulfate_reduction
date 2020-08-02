# prepare annotree based custome database for blast for soxY

```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/soxY_annotree_gene_tree


#, search by tigrfam, 
awk -F ',' '{print ">"$4"___"$3"\n"$5}' soxY_bac_annotree_hits.csv> soxY_bac_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' soxY_bac_ar_anntree_hits.fasta
sed -i -e '/sequence/d' soxY_bac_ar_anntree_hits.fasta


```

```
grep -c '>' soxY_bac_ar_anntree_hits.fasta
#1727

grep 'K17226' soxY_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > soxY_user.out.top_positive_hits.txt
pullseq -i soxY_bac_ar_anntree_hits.fasta -n soxY_user.out.top_positive_hits.txt > soxY_ba_ar_anntree_pos_hits.fasta

```


```
grep 'soxY' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >soxY_clean_geneID.txt
pullseq -i soxY.hmm.collection.faa  -n soxY_clean_geneID.txt > soxY.hmm.collection_clean.fasta

grep -c '>' soxY.hmm.collection_clean.fasta
wc -l soxY_clean_geneID.txt
#
```

```

#prepare classification file for aprA and soxB==>r95

#on mac, annotree reference 
sed -i -e 's/___/;/g' soxB_user.out.top_positive_hits.txt

cut -f1 -d$';' soxB_user.out.top_positive_hits.txt > soxB_ba_ar_annotree_hits_MAGid.txt

#ba-ar 
grep -w -f soxB_ba_ar_annotree_hits_MAGid.txt ../ar_ba_taxonomy_r95.tsv >soxB_ba_ar_annotree_hits_MAG_tax.txt

sed -i -e 's/;/___/g' soxB_user.out.top_positive_hits.txt


#edit in excel to get tree id and MAGs id done
#e.g.
#Tree_ID	MAGs_ID	Seq_ID
#GB_GCA_000007225.1___AE009441.1_1796	GB_GCA_000007225.1	AE009441.1_1796

# MAGsID	Tax_full

sed -i '1 i\MAGsID\tTax_full'  soxB_ba_ar_annotree_hits_MAG_tax.txt

cat soxB_user.out.top_positive_hits.txt|awk -F '\t' '{print $0"\t"$1}'|sed -e 's/___/\t/2' - > soxB_geneID_MAGsID.txt  
sed -i '1 i\Tree_ID\tMAGs_ID\tSeq_ID' soxB_geneID_MAGsID.txt

##merge in R

R
setwd("./")
soxB_tax <- read.delim("soxB_ba_ar_annotree_hits_MAG_tax.txt",header=T, check.names = FALSE) 

soxB_ID <- read.delim("soxB_geneID_MAGsID.txt",header=T, check.names = FALSE) 

soxB_ID_tax<-merge(soxB_ID, soxB_tax, by.x="MAGs_ID", by.y="MAGsID", all=TRUE)

write.table(soxB_ID_tax, 'soxB_ID_tax_annotree.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")

#after merge from R
#prepare nodes label
#,-1,#000000,normal,1,0


cat soxB_ID_tax_annotree.txt|cut -f3,5 -d$'\t'|sed -e 's/\t/,/g' - >soxB_ID_tax_annotree_R95.txt
sed -i -e 's/$/,-1,#000000,normal,1,0/g' soxB_ID_tax_annotree_R95.txt
```
