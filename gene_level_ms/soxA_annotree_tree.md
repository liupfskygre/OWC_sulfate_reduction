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
pullseq -i soxA.hmm.collection.faa -n soxA_clean_geneID.txt > soxB.hmm.collection_clean.fasta

grep -c '>' soxA.hmm.collection_clean.fasta
wc -l soxA_clean_geneID.txt
#403
```
