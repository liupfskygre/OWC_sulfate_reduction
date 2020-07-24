##sor

#reference 
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/sor_tree_annotree_gene_tree

#sor
cat sor_arc_annotree_hits.csv sor_bac_annotree_hits.csv > sor_ba_ar_anntree_hits.csv


#sor, search by KO, 
awk -F ',' '{print ">"$4"___"$3"\n"$6}' sor_ba_ar_anntree_hits.csv > sor_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' sor_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' sor_ba_ar_anntree_hits.fasta


```
#2
```
grep -c '>' sor_ba_ar_anntree_hits.fasta

grep 'K16952' sor_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > sor_user.out.top_positive_hits.txt
pullseq -i sor_ba_ar_anntree_hits.fasta -n sor_user.out.top_positive_hits.txt > sor_ba_ar_anntree_pos_hits.fasta

grep -c 'K16952' sor_user.out.top.txt
grep -c '>' sor_ba_ar_anntree_pos_hits.fasta
```
