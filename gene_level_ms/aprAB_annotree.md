##



##
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/aprAB_w_annotree_tree

#aprA
cat aprA_arc_annotree_hits.csv aprA_bac_annotree_hits.csv > aprA_ba_ar_anntree_hits.csv

#aprB 
cat aprB_arc_annotree_hits.csv aprB_bac_annotree_hits.csv > aprB_ba_ar_anntree_hits.csv

#aprA
awk -F ',' '{print ">"$4"___"$3"\n"$5}' aprA_ba_ar_anntree_hits.csv > aprA_ba_ar_anntree_hits.fasta

awk -F ',' '{print ">"$4"___"$3"\n"$5}' aprB_ba_ar_anntree_hits.csv > aprB_ba_ar_anntree_hits.fasta

sed -i -e '/gtdbId___geneId/d' aprA_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' aprA_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' aprB_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' aprB_ba_ar_anntree_hits.fasta
```

#submit to GhostKAAS
```
#https://www.kegg.jp/ghostkoala/
#one per time
#download
==> Annotation data: user_ko.txt
==> Taxonomy data: user.out.top.txt ==>this one is the tax


https://www.ncbi.nlm.nih.gov/Structure/bwrpsb/bwrpsb.cgi

```

#filter positive hits from ghostkoala analysis
```
#aprA
grep -c '>' aprA_ba_ar_anntree_hits.fasta #1104
grep 'K00394' aprA_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > aprA_user.out.top_positive_hits.txt
pullseq -i aprA_ba_ar_anntree_hits.fasta -n aprA_user.out.top_positive_hits.txt > aprA_ba_ar_anntree_pos_hits.fasta

#aprB
grep -c '>' aprB_ba_ar_anntree_hits.fasta
grep 'K00395' aprB_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > aprB_user.out.top_positive_hits.txt
pullseq -i aprB_ba_ar_anntree_hits.fasta -n aprB_user.out.top_positive_hits.txt > aprB_ba_ar_anntree_pos_hits.fasta
```

#filtering owc reads based on ghostkola annotation； cat with reference sequence
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/aprAB_w_annotree_tree

#aprA
grep 'aprA' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >aprA_clean_geneID.txt
pullseq -i aprA.hmm.collection.faa -n aprA_clean_geneID.txt > aprA.hmm.collection_clean.fasta

grep -c '>' aprA.hmm.collection_clean.fasta
wc -l aprA_clean_geneID.txt
#233

cat aprA_ba_ar_anntree_pos_hits.fasta aprA.hmm.collection_clean.fasta > aprA_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto aprA_owc_clean_wAnnRef.fasta > aprA_owc_clean_wAnnRef_align.fasta
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in aprA_owc_clean_wAnnRef_align.fasta -out aprA_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  aprA_owc_clean_wAnnRef_trimal.fasta 
```

#
```
#aprB
grep 'aprB' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >aprB_clean_geneID.txt
pullseq -i aprB.hmm.collection.faa -n aprB_clean_geneID.txt > aprB.hmm.collection_clean.fasta

grep -c '>' aprB.hmm.collection_clean.fasta
wc -l aprB_clean_geneID.txt
#233

cat aprB_ba_ar_anntree_pos_hits.fasta aprB.hmm.collection_clean.fasta > aprB_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto aprB_owc_clean_wAnnRef.fasta > aprB_owc_clean_wAnnRef_align.fasta
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in aprB_owc_clean_wAnnRef_align.fasta -out aprB_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  aprB_owc_clean_wAnnRef_trimal.fasta 

```



#combine annotree reference and owc_seqs for alignment and tree
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/aprAB_w_annotree_tree


/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto aprA_4_tree_w_ref.fasta > aprA_4_tree_w_ref_align.fasta

aprA_4_tree_w_ref_align_refine.fasta

#trimAl v1.4.rev22 build[2015-05-21]
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in aprA_4_tree_w_ref_align_refine.fasta -out aprA_4_tree_w_ref_align_refine_trim.fasta


```
