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

grep -c '>' soxB_ba_ar_anntree_hits.fasta

grep 'K17222' soxB_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > soxB_user.out.top_positive_hits.txt
pullseq -i soxB_ba_ar_anntree_hits.fasta -n soxB_user.out.top_positive_hits.txt > soxB_ba_ar_anntree_pos_hits.fasta

grep -c 'K17224' soxB_user.out.top.txt
grep -c '>' soxB_ba_ar_anntree_pos_hits.fasta


#soxB


cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/soxB_annotree_gene_tree

grep 'soxB' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >soxB_clean_geneID.txt
pullseq -i soxB.hmm.collection.faa -n soxB_clean_geneID.txt > soxB.hmm.collection_clean.fasta

grep -c '>' soxB.hmm.collection_clean.fasta
wc -l soxB_clean_geneID.txt
#
