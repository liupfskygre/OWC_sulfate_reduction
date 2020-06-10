## expression level of H2ase in 1712 MAGs 

## custome hmm search of H2ase of within 3211 MAGs db
```
hmm profile collection with cutoff 

# db, dramout db

#profile metadata text: include profile name, profile GA, TC, filtering length

#for H2ase, only cutoff used from Zhou and Annathramann et al., METABOLICs 
# file include sample name and all path to protein and nt file

/home/projects/Wetlands/sulfur_cycling_analysis/H2ase_hmm_db_3211

../hmmsearch_get_seqs_w_proteins.sh sample_info.txt hmm_meta_file.txt 2 &>hmmsearch.log
#failed cutoff filtering, typo error


#awk -v v1="${Cutoof1}" -v v2="${Cutoof2}" -v v3="${length}" -F '\t' '$8>v1 && $14>v2 && $3>v3' all_3211_genes_DRAM_TIGR04266.HMM_domtblout.txt|grep -v '^#' > test1.txt #cutoff1, cutoff2

cut -f8 -d$'\t' test1.txt|more
cut -f8 -d$'\t'  all_3211_genes_DRAM_TIGR04266.HMM_domtblout.txt|more 
```

## cat all sequences together
```{r}
for file in *.ID
do 
sed -i -e 's/\t"${file}"//g' ${file}

#sed -i -e "s/$/\t"${file}"/g" ${file}

done

cat *.ID > Hydrogenase_mdh_DB3211_hitsID.txt
all_3211_genes_DRAM_
.hmm_seqs.ID
.HMM_seqs.ID

sed -i -e 's/all_3211_genes_DRAM_//g' Hydrogenase_mdh_DB3211_hitsID.txt
sed -i -e 's/\.hmm_seqs.ID//g' Hydrogenase_mdh_DB3211_hitsID.txt
sed -i -e 's/\.HMM_seqs.ID//g' Hydrogenase_mdh_DB3211_hitsID.txt

sed -i '1 i\Gene_ID\tGene_type' Hydrogenase_mdh_DB3211_hitsID.txt

python /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/joint_htseq_output.py Hydrogenase_mdh_DB3211_hitsID.txt /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt >Hydrogenase_mdh_DB3211_TPM.txt

python /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/joint_htseq_output.py Hydrogenase_mdh_DB3211_TPM.txt ../marker_gene_confirmation_final/MAGs1761_w_sulfur_Gene_annotation_fix.txt > OWC2014-2018_Bac1712_H2ase_mdh_TPM_w_annotation.txt

```


## KO list of hydroogenase
```
cd /home/projects/Wetlands/sulfur_cycling_analysis
mkdir H2ase 
cd H2ase

nano H2ase_KO.txt

H2ase_KO.txt
```


#get H2ase coding genes of all MAGs, then TPM valuse across all samples
```
/home/projects/Wetlands/sulfur_cycling_analysis/H2ase

grep -w -f H2ase_KO.txt ../CAZY_metaT/combined_annotations_1712MAGs_bac.tsv|cut -f1 -d$'\t' > 1712MAGs_bac_H2_cycling_gene.txt

grep -w -f 1712MAGs_bac_H2_cycling_gene.txt /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt > OWC2014-2018_Bac1712_H2_cycle_TPM.txt


#add this three into vhoACT
K14068, K14069, K14070 # all zero


##fix header
head -1 /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt> header

cat header OWC2014-2018_Bac1712_H2_cycle_TPM.txt > OWC2014-2018_Bac1712_H2_cycle_TPM_h.txt


##add tax info to TPM values


python /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/joint_htseq_output.py OWC2014-2018_Bac1712_H2_cycle_TPM_h.txt ../marker_gene_confirmation_final/MAGs1761_w_sulfur_Gene_annotation_fix.txt > OWC2014-2018_Bac1712_H2ase_cycle_TPM_w_annotation.txt

awk -F '\t' '{print NF; exit}' OWC2014-2018_Bac1712_H2ase_cycle_TPM_w_annotation.txt

cat  OWC2014-2018_Bac1712_H2ase_cycle_TPM_w_annotation.txt|cut -f1-128,129,135,153 -d$'\t'>OWC2014-2018_Bac1712_H2ase_cycle_TPM_w_tax.txt

```
