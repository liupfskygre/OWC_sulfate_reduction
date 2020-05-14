## run bam filtering, htseq and joint all htseq into one table

## bam filtering

**part I metaT_mapping2018_MAGs3211**
```
/home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/metaT_mapping2018_MAGs3211

mkdir RSEM_transcript_bam
cd RSEM_transcript_bam

sbatch metaT__filter_HTSeq_2018OWC.sh
Submitted batch job 4010


#2014
sbatch metaT__filter_HTSeq_2014OWC.sh

```

##gene list from gff file

```
cd /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping 

cat /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_genes_3211db.gff|cut -f9 -d$'\t' |cut -f1 -d';' >  all_bins_combined_genes_3211db_ID.txt
sed -i -e 's/ID=//g' all_bins_combined_genes_3211db_ID.txt

sed -i -e 's/\#\#gff-version 3/gene_ID/g' all_bins_combined_genes_3211db_ID.txt
9480542 all_bins_combined_genes_3211db_ID.txt

## feature length 
#get an adding feature length of genes

cat  /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_genes_3211db.gff | cut -f 4,5,9 -d$'\t'|grep -v '#' >gene_feathure_cor.txt  
cat gene_feathure_cor.txt |cut -f1 -d';' >  gene_feathure_cor_tmp.txt

sed -i -e 's/ID=//g' gene_feathure_cor_tmp.txt


sed -i '/^$/d' gene_feathure_cor_tmp.txt

sed -i "1i start\tend\tgene_ID" gene_feathure_cor_tmp.txt 

awk -F '\t' '{print $3"\t"$1"\t"$2}' gene_feathure_cor_tmp.txt  > gene_feathure_cor.txt  

awk 'BEGIN { OFS = "\t" } NR == 1 { $4 = "Length" } NR >= 2 { $4 = $3 - $2 + 1 } 1' gene_feathure_cor.txt   > gene_feathure.txt
```

## adding header to HTseq out and join all samples

```
## OWC_2018
# /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/metaT_mapping2018_MAGs3211/RSEM_transcript_bam

for file in $(cat bam.list)
do
sed -i "1i gene_ID\t"${file}"" "${file}"_htseq.out #Aug_M1_C2_D3_htseq.out
done


for file in $(cat bam.list)
do 
python ../../joint_htseq_output.py host.txt "${file}"_htseq.out> tmp.txt
mv tmp.txt host.txt

done 


##adding feature length 
python ../../joint_htseq_output.py ../../gene_feathure.txt host.txt > metaT_mapping2018_MAGs3211_htseq_out.txt

awk -F '\t' '{print NF; exit}' metaT_mapping2018_MAGs3211_htseq_out.txt
##116 


## OWC_2014
# /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/metaT_mapping2014_MAGs3211/RSEM_transcript_bam

for file in $(cat bam.list)
do
sed -i "1i gene_ID\t"${file}"" "${file}"_htseq.out #Aug_M1_C2_D3_htseq.out
done


for file in $(cat bam.list)
do 
python ../../joint_htseq_output.py host.txt "${file}"_htseq.out> tmp.txt
mv tmp.txt host.txt

done 


##merge into OWC_2018

python ../../joint_htseq_output.py ../../gene_feathure.txt host.txt > metaT_mapping2014_MAGs3211_htseq_out.txt

python ../../joint_htseq_output.py ../../metaT_mapping2018_MAGs3211_htseq_out.txt host.txt > metaT_mapping2014_2018_MAGs3211_htseq_out.txt

#metaT_mapping2018_MAGs3211_htseq_out.txt
```

TPM 
```
#remove t_RNA lines

#omit NA in R

#



```
