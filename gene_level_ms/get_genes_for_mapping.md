## on zenith

## add header to prodigal genes
#>AugM1C1D1B_prodigal~~
```

cd /home/projects/Wetlands/sulfur_cycling_analysis/hmmsearch_hits_assembly

for file in *_prodigal.fna
do
sed -e "s/>/>"${file%.*}"~~/g" ${file} > "${file%.*}"_tmp.fna
cat  "${file%.*}"_tmp.fna >> all_prodigal_tmp.fna
rm "${file%.*}"_tmp.fna
done
```

#remove and clean header
```
all_prodigal_tmp.fna
sed -i -e 's/#.*$//g' all_prodigal_tmp.fna 

```

#cat all gene ids
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AA-sequences-for-tree
cat *hmm.collection_id.txt >all_Raw_genes_id.txt

 6091 all_Raw_genes_id.txt genes
 
pullseq -i all_prodigal_tmp.fna  -n all_Raw_genes_id.txt > all_sulfur_cycling_genes_Raw.fna
```

## pullseq seqs based on gene names
```


```

##move to metaT mapping


##
