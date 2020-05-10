## run bam filtering, htseq and joint all htseq into one table

## bam filtering

**part I metaT_mapping2018_MAGs3211**
```
/home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/metaT_mapping2018_MAGs3211

mkdir RSEM_transcript_bam
cd RSEM_transcript_bam

sbatch metaT__filter_HTSeq_2018OWC.sh
Submitted batch job 4010

```

##gene list from gff file

```
cd /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping 

cat /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_genes_3211db.gff|cut -f9 -d$'\t' |cut -f1 -d';' >  all_bins_combined_genes_3211db_ID.txt
sed -i -e 's/ID=//g' all_bins_combined_genes_3211db_ID.txt

sed -i -e 's/\#\#gff-version 3/gene_ID/g' all_bins_combined_genes_3211db_ID.txt
9480542 all_bins_combined_genes_3211db_ID.txt



#adding header to HTseq out
for file in Mud_11_14_A.L20.Q20_MAGs3211_RSEM.sorted.out
do
sed -i "1i gene_ID\t"${file}""  Mud_11_14_A.L20.Q20_MAGs3211_RSEM.sorted.out
done

python joint_htseq_output.py host.txt ./metaT_mapping2014_MAGs3211/Mud_11_14_A.L20.Q20_MAGs3211_RSEM.sorted.out > tmp.txt

mv tmp.txt host.txt


```
