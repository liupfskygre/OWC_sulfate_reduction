## dsrAB tree w annotree ref 


## will this increase the classification level of our data??

##download annotree reference and convert to fasta file
```
#/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrAB_tree_with_Annotree_ref

#dsrA
cat dsrA_arc_annotree_hits.csv dsrA_bac_annotree_hits.csv > dsrA_ba_ar_anntree_hits.csv
#891


#dsrB 
cat dsrB_arc_annotree_hits.csv dsrB_bac_annotree_hits.csv > dsrB_ba_ar_anntree_hits.csv
#860

#annotree_hits_mcrA_PF02249_PF02745.csv
awk -F ',' '{print ">"$4";"$3"\n"$5}' dsrA_ba_ar_anntree_hits.csv > dsrA_ba_ar_anntree_hits.fasta


awk -F ',' '{print ">"$4";"$3"\n"$5}' dsrB_ba_ar_anntree_hits.csv > dsrB_ba_ar_anntree_hits.fasta

#cat annotree_hits_mcrA_PF02249_PF02745_raw.fasta| seqkit rmdup -s -o annotree_hits_mcrA_PF02249_PF02745_clean.fasta
#awk -F ',' '{print ">"$1"_"$2"_"$3"_"$4"\n"$5}' tabseq.tsv > seqs.fa
```


#dsrB
```
cat dsrB_ba_ar_anntree_hits.fasta dsrB.hmm.collection.faa >dsrB.hmm.collection_w_annotreeRef.faa

#on mac
/Users/pengfeiliu/software/mafft-mac/mafftdir/bin/mafft --auto --add dsrB.hmm.collection_w_annotreeRef.faa --thread 4 Full_length_seq_dsrAB_ref_fixH.faa >dsrB_anno_tree_alignment.faa

/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in dsrB_anno_tree_alignment.faa -out dsrB_anno_tree_alignment_trmal.fasta

#remove dsrA part and remove columns w/ 10% gaps

```
