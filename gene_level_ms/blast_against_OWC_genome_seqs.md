## sequences from contigs to blast against OWC_genomes derived sequences

#compare with blast resutlts from annotree results

#prepare blast db for diamond, 
```
#https://github.com/liupfskygre/OWC_sulfate_reduction/blob/master/gene_Tree/confirmation_of_final_marker.md

#sdo in genomes filtering based on Ghostkola
#K17725
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Data_analysis_sulfur_cycling/gene_tree/sdo_tree

grep -c '>' sdo_4_tree.faa

grep 'K17725' sdo.user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > sdo_user.out.top_positive_hits.txt
pullseq -i sdo_4_tree.faa -n sdo_user.out.top_positive_hits.txt > sdo_owc_pos_hits.fasta

#173 sequences
```





```
#cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Data_analysis_sulfur_cycling/Confirmed_marker_gene_hits

#zenith
cd /home/projects/Wetlands/sulfur_cycling_analysis
all_bins_combined_annotations_3211db.tsv



#all_22_marker_gene_final_DRAM_annotation_w_genename.xlsx, cleaned
#add sdo to the confirmed list14 ==>15 genes as using in the gene level analysis
#make a list of genes


#get all sequence from all_bins_combined_annotations_3211db.tsv 
pullseq -i all_3211_genes_DRAM_aa.faa -n sulfur_cycing_gene_in_OWC_MAGs.txt > sulfur_cycing_gene_in_OWC_MAGs.txt 
```
