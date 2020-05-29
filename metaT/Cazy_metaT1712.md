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


#get metabolism_summary, to get cazy hits curation file
```
source /opt/Miniconda2/miniconda2/bin/activate DRAM

DRAM.py distill -i combined_annotations_1712MAGs_bac.tsv -o 1712MAGs_bac
```

## cazy-hits category curation file
```
DRAM_CAZY_subsystem.txt


```

#simple version of cazy_hits annotation, with cazy hit id being one column
```
cat combined_annotations_1712MAGs_bac_cazy.tsv| cut -f1,2,22,26 -d$'\t' > combined_annotations_1712MAGs_bac_CAZY_simple.tsv

sed -i -e 's/ /\t/1' combined_annotations_1712MAGs_bac_CAZY_simple.tsv
sed -i -e '1d' combined_annotations_1712MAGs_bac_CAZY_simple.tsv

sed -i -e '1 i\Gene_ID\tfasta\tcazy_hits\tcazy_description\tbin_taxonomy' combined_annotations_1712MAGs_bac_CAZY_simple.tsv


#cohesin and SLH, with $3==None, fix none cazy gene-id 
awk -F '\t' '$3!="None"' combined_annotations_1712MAGs_bac_CAZY_simple.tsv >combined_annotations_1712MAGs_bac_CAZY_SF.tsv
```

# merge gene--cazy category--tpm together
```
combined_annotations_1712MAGs_bac_cazy.tsv

cat combined_annotations_1712MAGs_bac_cazy.tsv | grep -v 'fasta' |cut -f1 -d$'\t' > combined_annotations_1712MAGs_bac_cazy.list

#grep -w -f combined_annotations_1712MAGs_bac_cazy.list /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt > OWC2014-2018_Bac1712_CAZY_TPM.txt  ## too slow


#
#

sed -i -e '1 i\Gene_ID' combined_annotations_1712MAGs_bac_cazy.list 

python /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/joint_htseq_output.py combined_annotations_1712MAGs_bac_CAZY_SF.tsv /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt > OWC2014-2018_Bac1712_CAZY_cycle_TPM_w_annotation.txt

```

#add cazy subsystem to cazy tpm with annotaiton
```
TPM_Gene <- read.delim("OWC2014-2018_Bac1712_CAZY_cycle_TPM_w_annotation.txt",header=T, check.names = FALSE)
Subsystem <- read.delim("DRAM_CAZY_subsystem.txt",header=T, check.names = FALSE) 

TPM_w_sub <- merge(TPM_Gene,Subsystem,by.x="cazy_hits",by.y="gene_id", all.x=T, all.y=F, sort=F )

write.table(TPM_w_sub, "OWC2014-2018_Bac1712_CAZY_cycle_TPM_w_ann_w_sub.txt",sep ='\t', quote=F, row.names=F)

awk -F '\t' '{print NF; exit}' OWC2014-2018_Bac1712_CAZY_cycle_TPM_w_ann_w_sub.txt
```

#add Polyphenolics Cleavage 
```
Polyphenolics.KO.list
K05810
K05809
K00422

grep -w -f Polyphenolics.KO.list combined_annotations_1712MAGs_bac.tsv >Polyphenolics_KO_gene_annotation.txt

cat header Polyphenolics_KO_gene_annotation.txt >Polyphenolics_KO_gene_anno_fix.txt

sed -i -e 's/\tfasta/Gene_ID\tfasta/1' Polyphenolics_KO_gene_anno_fix.txt
#1264


python /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/joint_htseq_output.py Polyphenolics_KO_gene_anno_fix.txt /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/OWC2014-2018_DB3211_genes_TPM.txt > Polyphenolics_TPM_w_annotation.txt
```

##rbind KO and cazy_hits kept hits w TPM valuse
```
#column kept: K0/Cazy_hits, TPM 1-127, GeneID, fasta, Tax

cat OWC2014-2018_Bac1712_CAZY_cycle_TPM_w_ann_w_sub.txt|  cut -f1-3,5,6-132-d$'\t' > OWC2014-2018_Bac1712_CAZY_cycle_TPM_unify.txt

Polyphenolics_TPM_w_annotation.txt
awk -F '\t' '{print $8"\t"$0}' Polyphenolics_TPM_w_annotation.txt |cut -f1-3,27,30-156 -d$'\t' > Polyphenolics_TPM_w_annotation_unify.txt
sed -i -e '1d' Polyphenolics_TPM_w_annotation_unify.txt

cat OWC2014-2018_Bac1712_CAZY_cycle_TPM_unify.txt Polyphenolics_TPM_w_annotation_unify.txt > Cazy_Polyphenolics_TPM_w_annotation_unify.txt
```



# add DRAM-liquor subsystem to TPM,

1) flat the DRAM_cazy heatmap category, based on NAR May-2020 resubmissiom
```
pengfeiliu /Users/pengfeiliu/A_Wrighton_lab/AMG_projects/AMG_progress $ sed -i -e 's/, /\n\t\t/g' CaZy_gene_link_flat_May2020NAR.txt

#covered 219 genes
CaZy_gene_link_flat_May2020NAR.txt

Cazy_Polyphenolics_TPM_w_annotation_unify.txt

>>R
TPM_Gene <- read.delim("Cazy_Polyphenolics_TPM_w_annotation_unify.txt",header=T, check.names = FALSE)
Subsystem <- read.delim("CaZy_gene_link_flat_May2020NAR.txt",header=T, check.names = FALSE) 

TPM_w_sub <- merge(Subsystem,TPM_Gene,by.x="function_ids",by.y="cazy_hits", all.x=T, all.y=F, sort=F )

write.table(TPM_w_sub, "OWC2014-2018_Bac1712_CAZY_Polyphenoic.txt",sep ='\t', quote=F, row.names=F)

#tested with duplicated values, function_ids will repeat, which means some of GH will assign several functions

```

