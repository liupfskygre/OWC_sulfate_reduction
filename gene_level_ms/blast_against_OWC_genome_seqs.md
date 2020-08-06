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
#
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/blastp_OWC_genome_db
nano sulfur_cycing_gene_in_OWC_MAGs.txt

#all_22_marker_gene_final_DRAM_annotation_w_genename.xlsx, cleaned
#add sdo to the confirmed list14 ==>15 genes as using in the gene level analysis
#make a list of genes

cat /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Data_analysis_sulfur_cycling/gene_tree/sdo_tree/sdo_user.out.top_positive_hits.txt >>sulfur_cycing_gene_in_OWC_MAGs.txt
#4252 sequences


#zenith
cd /home/projects/Wetlands/sulfur_cycling_analysis/DRAM_OUT
all_bins_combined_annotations_3211db.tsv

grep -w -f sulfur_cycing_gene_in_OWC_MAGs.txt all_bins_combined_annotations_3211db.tsv > sulfur_cycing_gene_in_OWC_MAGs4253DRAM.tsv

#get all sequence from all_bins_combined_annotations_3211db.tsv 
pullseq -i ../all_3211_genes_DRAM_aa.faa -n sulfur_cycing_gene_in_OWC_MAGs.txt > sulfur_cycing_gene_in_OWC_MAGs.fasta 
#4252 sequences
```

#adding all Muller2015, Annotree, OWC_MAGs db to creat a master db
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/blastp_OWC_genome_db
cat Full_length_seq_dsrAB_ref_fixH.faa sulfur_cycing_gene_in_OWC_MAGs.fasta sulfur_marker_gene_annotree_db.fas > sulfur_gene_master_cus_db1.fasta

diamond makedb --in sulfur_gene_master_cus_db1.fasta  -d sulfur_gene_master_cus_db1

diamond blastp -p 8 -d sulfur_gene_master_cus_db1 --daa sulfur_gene_master_cus_db1.out -q sulfur_marker_gene_OWC.fas --ultra-sensitive

diamond blastp -p 8 -d sulfur_gene_master_cus_db1 -o sulfur_owc_diamond_out_cus_master.txt -f 6 -q sulfur_marker_gene_OWC.fas --max-target-seqs 1 --ultra-sensitive


diamond view -a sulfur_owc_diamond.results > sulfur_owc_diamond.search_result.tab
 sed -e 's/___.*_[0-9]*\t/\t/1' sulfur_owc_diamond.search_result.tab >sulfur_owc_diamond.search_result_fix.tab

```

#merge the blastp results with the taxonomic info
```
sulfur_owc_diamond_out_cus_master.txt #blastp out

nano OWC_hits_seqs_ID.txt
nano Annotree_hits_seqs_ID.txt

grep -w -f Annotree_hits_seqs_ID.txt ../ar_ba_taxonomy_r95.tsv > Diamond_OWC_hits_annotree_MAGs_Taxonomy.txt

grep -w -f OWC_hits_seqs_ID.txt sulfur_cycing_gene_in_OWC_MAGs4253DRAM.tsv >Diamond_OWC_hits_OWC_MAGs_Taxonomy.txt

```
```

R
setwd("./")
#annotree resultes
MAGs_tax <- read.delim("Diamond_OWC_hits_annotree_MAGs_Taxonomy.txt",header=T, check.names = FALSE) 

blastP_out <- read.delim("sulfur_owc_diamond_out_cus_master.txt",header=T, check.names = FALSE) 

OWC_genes <-read.delim("../blast_custome_annotree_db/TPM_Gene_W_5ktax.txt",header=T, check.names = FALSE) 

#merge 1

MAGs_tax_blastp <-merge(MAGs_tax, blastP_out, by.x="MAGsID", by.y="MAGs_ID", all=TRUE)


MAGs_tax_blastp_OWC <-merge(MAGs_tax_blastp, OWC_genes, by.x="Gene_ID", by.y="Gene_ID", all=TRUE)

write.table(MAGs_tax_blastp_OWC, 'Sulfur_OWC_genes_w_all_MAGs_tax.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")

```



#1, blast
#diamond blastp
```



```
