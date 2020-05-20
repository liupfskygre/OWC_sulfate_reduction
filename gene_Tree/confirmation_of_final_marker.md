

## gene list, mac
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Data_analysis_sulfur_cycling/Confirmed_marker_gene_hits
cat *.txt >all_22_marker_gene_final.list
wc -l all_22_marker_gene_final.list
    5168 all_22_marker_gene_final.list


cat all_22_marker_gene_final.list |sort|uniq|wc -l
    5167 #there is one dsrAB gene fusion, others are great

#with gene names

#upload to zenith
sed -i -e 's/\.txt//g' all_22_marker_gene_final_w_genename.list


#merge annotation with gene names
#/home/projects/Wetlands/sulfur_cycling_analysis/marker_gene_confirmation_final
 
python ../metaT_mapping/joint_htseq_output.py all_22_marker_gene_final_w_genename.list all_22_marker_gene_final_DRAM_annotation.tsv   >all_22_marker_gene_final_DRAM_annotation_w_genename.tsv 
```

## 
```
/home/projects/Wetlands/sulfur_cycling_analysis


```

## bin list and marker genes with annotation
```
/home/projects/Wetlands/sulfur_cycling_analysis/marker_gene_confirmation_final

grep -w -f all_22_marker_gene_final.list  /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_annotations_3211db.tsv > all_22_marker_gene_final_dram_annotation.tsv

grep -w -f doxX_D_4_tree_final.txt  /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_annotations_3211db.tsv > all_22_marker_gene_final_dram_annotation2.tsv

cat all_22_marker_gene_final_dram_annotation.tsv all_22_marker_gene_final_dram_annotation2.tsv > all_22_marker_gene_final_DRAM_annotation.tsv 
#==> DRAM annotation of functional genes 


cat all_22_marker_gene_final_DRAM_annotation.tsv |cut -f2 -d$'\t' |sort|uniq > all_22_marker_gene_final_MAGs.tsv
# 1761 all_22_marker_gene_final_MAGs.tsv, 1761 MAGs



for file in $(cat all_22_marker_gene_final_MAGs.tsv)
do
echo /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"${file}"_DRAMOUT

cat /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"${file}"_DRAMOUT/annotations.tsv >> MAGs1761_w_sulfur_Gene_annotation.txt
printf "\n" >>MAGs1761_w_sulfur_Gene_annotation.txt
done

#==> DRAM annotation of 1762 MAGs with sulfur cycling genes 

``` 

#functional key genes screen is very conservative, 
#KEGG list of all possible genes for sulfur cycling contains more, make a list 
#mark those from consistent with homologue search, also check gene clusters if possible

```
grep -w -f sulfur_cycling_KO_gene.list MAGs1761_w_sulfur_Gene_annotation.txt > Sulfur_Gene_annotation_DRAM_KO.txt
```

## metaT first view
```
grep -w -f all_22_marker_gene_final.list  ../metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt > OWC2014-2018_DB3211_sulfur_markers_TPM.txt
```





## checking with previous list
```
cd /home/projects/Wetlands/sulfur_cycling_analysis

MAGs_Pro_Con_wide_w_profile_name.txt



#checking with previous list
[liupf@zenith marker_gene_confirmation_final]$ cat MAGs_Gene_num_wide.txt| cut -f1 -d$'\t'  |sort|uniq |wc -l
# 1929 MAGs in the pre-analysis

grep -w -f all_22_marker_gene_final_MAGs.tsv MAGs_Gene_num_wide.txt > MAGs_Gene_num_wide_final.txt
cat MAGs_Gene_num_wide_final.txt| cut -f1 -d$'\t'  |sort|uniq |wc -l
# 1266 in common 
```
