##peptidase from DRAM annotation and their transcripts abundances

##


##get peptidase from 1712 MAGs
```
cd /home/projects/Wetlands/sulfur_cycling_analysis

mkdir peptidase_metaT

#
/home/projects/Wetlands/sulfur_cycling_analysis/CAZY_metaT/combined_annotations_1712MAGs_bac.tsv

#header of dramout
	fasta	scaffold	gene_position	start_position	end_position	strandedness	kegg_id	kegg_hit	kegg_RBH	kegg_identity	kegg_bitScore	kegg_eVal	peptidase_id#14	peptidase_family	peptidase_hit	peptidase_RBH	peptidase_identity	peptidase_bitScore	peptidase_eVal#20	pfam_hits	cazy_hits	vogdb_description	vogdb_categories	heme_regulatory_motif_count	bin_taxonomy	bin_completeness	bin_contamination



#get peptidase hits; $13
awk -F'\t' '$14!=""' ../CAZY_metaT/combined_annotations_1712MAGs_bac.tsv > tmp.tsv #w all fasta header
mv tmp.tsv  combined_annotations_1712MAGs_bac_peptidase.tsv 

```


## merge gene--cazy category--tpm together
```

#add metaT to data
#
sed -i -e '1 i\Gene_ID' combined_annotations_1712MAGs_bac_peptidase.tsv  

python /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/joint_htseq_output.py combined_annotations_1712MAGs_bac_peptidase.tsv  /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt > OWC2014-2018_Bac1712_peptidase_TPM_w_annotation.txt
```


