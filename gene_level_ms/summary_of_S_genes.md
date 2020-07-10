##

#
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AA-sequences-for-tree
for file in *.hmm.collection_id.txt
do
sed -i -e "s/$/\t"${file}"/g" ${file}
sed -i -e "s/\.hmm\.collection_id\.txt//g" ${file}
done


cat *.hmm.collection_id.txt> all_Raw_genes_id_w_tax.txt

#6903

mv all_Raw_genes_id_w_tax.txt all_Raw_genes_id_w_gene_type.txt


#annotation of genes by EGGNOG
cat *.faa > all_Raw_genes_aa_sequences.fasta
#grep -c '>'  all_Raw_genes_aa_sequences.fasta


#eggnog annotation


#kegg annotation
https://www.kegg.jp/blastkoala/

#
```
