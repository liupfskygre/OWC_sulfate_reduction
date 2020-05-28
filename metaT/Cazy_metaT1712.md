## summary of metaT bacteria Cazy TPM values

/home/projects/Wetlands/sulfur_cycling_analysis/CAZY_metaT


#file preparation
```
awk -F'\t' '$1!=""' infile > out file
 
 #cazy_hits

annotation.tsv ==> cazy_hits, get cazy_hits ==> 

gene_id gene_description	module	header	subheader in metabolism_summary for category

also check the CAZY_outline
```


get cazy_hits from 1712 MAGs
```
#gtdb_and_checkm_1712MAGs_bac.list
cd /home/projects/Wetlands/sulfur_cycling_analysis/CAZY_metaT

for file in $(cat /home/projects/Wetlands/sulfur_cycling_analysis/phylogenomics_tree/gtdb_and_checkm_1712MAGs_bac.list)
do
echo /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"${file}"_DRAMOUT

cat /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"${file}"_DRAMOUT/annotations.tsv >> combined_annotations_1712MAGs_bac.tsv

printf "\n" >>combined_annotations_1712MAGs_bac.tsv
done

grep -v "fasta"  combined_annotations_1712MAGs_bac.tsv > combined_annotations_1712MAGs_bac_fix.tsv

awk -F'\t' '$22!=""' combined_annotations_1712MAGs_bac.tsv > tmp.tsv #w all fasta header
head -1 combined_annotations_1712MAGs_bac.tsv > header
cat header tmp.tsv > combined_annotations_1712MAGs_bac_cazy.tsv 

cat header combined_annotations_1712MAGs_bac_fix.tsv >combined_annotations_1712MAGs_bac.tsv
```


#get metabolism_summary
```
source /opt/Miniconda2/miniconda2/bin/activate DRAM

DRAM.py distill -i combined_annotations_1712MAGs_bac.tsv -o 1712MAGs_bac
```

# merge gene--cazy category--tpm together
```
combined_annotations_1712MAGs_bac_cazy.tsv

cat combined_annotations_1712MAGs_bac_cazy.tsv | grep -v 'fasta' |cut -f1 -d$'\t' > combined_annotations_1712MAGs_bac_cazy.list

grep -w -f combined_annotations_1712MAGs_bac_cazy.list /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt > OWC2014-2018_Bac1712_CAZY_TPM.txt

```

#simple version of cazy_hits annotation, with cazy hit id being one column
```
cat combined_annotations_1712MAGs_bac_cazy.tsv| cut -f1,2,22 -d$'\t' > combined_annotations_1712MAGs_bac_CAZY_simple.tsv

sed -i -e 's/ /\t/1' combined_annotations_1712MAGs_bac_CAZY_simple.tsv

#cohesin and SLH, with $3==None 
awk -F'\t' '$3!="None"' combined_annotations_1712MAGs_bac_CAZY_simple.tsv >combined_annotations_1712MAGs_bac_CAZY_SF.tsv
```
