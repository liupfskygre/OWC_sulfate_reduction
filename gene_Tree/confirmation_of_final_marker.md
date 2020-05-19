

## gene list, mac
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Data_analysis_sulfur_cycling/Confirmed_marker_gene_hits
cat *.txt >all_22_marker_gene_final.list
wc -l all_22_marker_gene_final.list
    5168 all_22_marker_gene_final.list


cat all_22_marker_gene_final.list |sort|uniq|wc -l
    5167 #there is one dsrAB gene fusion, others are great


```

## 
```
/home/projects/Wetlands/sulfur_cycling_analysis


```

##bin list
```
grep -w -f all_22_marker_gene_final.list  /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_annotations_3211db.tsv > all_22_marker_gene_final_dram_annotation.tsv

grep -w -f doxX_D_4_tree_final.txt  /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_annotations_3211db.tsv > all_22_marker_gene_final_dram_annotation2.tsv

cat all_22_marker_gene_final_dram_annotation.tsv all_22_marker_gene_final_dram_annotation2.tsv > all_22_marker_gene_final_DRAM_annotation.tsv

cat all_22_marker_gene_final_DRAM_annotation.tsv |cut -f2 -d$'\t' |sort|uniq > all_22_marker_gene_final_MAGs.tsv
# 1761 all_22_marker_gene_final_MAGs.tsv, 1761 MAGs



grep -w -f all_22_marker_gene_final_MAGs.tsv /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_annotations_3211db.tsv > MAGs_w_sulfur_Gene_annotation.txt

/home/projects/Wetlands/sulfur_cycling_analysis/marker_gene_confirmation_final


for file in $(cat all_22_marker_gene_final_MAGs.tsv)
do
echo /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"${file}"_DRAMOUT

cat /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"${file}"_DRAMOUT/annotations.tsv >> MAGs1761_w_sulfur_Gene_annotation.txt
printf "\n" >>MAGs1761_w_sulfur_Gene_annotation.txt
done



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
