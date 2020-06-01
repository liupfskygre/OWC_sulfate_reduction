##peptidase from DRAM annotation and their transcripts abundances

##


##get peptidase from 1712 MAGs
```
cd /home/projects/Wetlands/sulfur_cycling_analysis

mkdir peptidase_metaT

#
/home/projects/Wetlands/sulfur_cycling_analysis/CAZY_metaT/combined_annotations_1712MAGs_bac.tsv

#get peptidase hits
awk -F'\t' '$22!=""' combined_annotations_1712MAGs_bac.tsv > tmp.tsv #w all fasta header
head -1 combined_annotations_1712MAGs_bac.tsv > header
cat header tmp.tsv > combined_annotations_1712MAGs_bac_cazy.tsv 

```


## simple version of peptidase annotation, with peptidase hit id being one column
```
cat combined_annotations_1712MAGs_bac_cazy.tsv| cut -f1,2,22,26 -d$'\t' > combined_annotations_1712MAGs_bac_CAZY_simple.tsv

sed -i -e 's/ /\t/1' combined_annotations_1712MAGs_bac_CAZY_simple.tsv
sed -i -e '1d' combined_annotations_1712MAGs_bac_CAZY_simple.tsv

sed -i -e '1 i\Gene_ID\tfasta\tcazy_hits\tcazy_description\tbin_taxonomy' combined_annotations_1712MAGs_bac_CAZY_simple.tsv

#cohesin and SLH, with $3==None, fix none cazy gene-id 
awk -F '\t' '$3!="None"' combined_annotations_1712MAGs_bac_CAZY_simple.tsv >combined_annotations_1712MAGs_bac_CAZY_SF.tsv
```

## merge gene--cazy category--tpm together
```
combined_annotations_1712MAGs_bac_cazy.tsv

cat combined_annotations_1712MAGs_bac_cazy.tsv | grep -v 'fasta' |cut -f1 -d$'\t' > combined_annotations_1712MAGs_bac_cazy.list


#add metaT to data
#
sed -i -e '1 i\Gene_ID' combined_annotations_1712MAGs_bac_cazy.list 

python /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/joint_htseq_output.py combined_annotations_1712MAGs_bac_CAZY_SF.tsv /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt > OWC2014-2018_Bac1712_CAZY_cycle_TPM_w_annotation.txt
```


