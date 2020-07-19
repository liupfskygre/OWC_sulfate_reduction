## put all TPM from metaT mapping together


## 
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mappping_geneDB



*_sulfur_cycling_gene.genes.results 
f1,6 

for file in *_sulfur_cycling_gene.genes.results 
do
cat ${file}|cut -f1,6 -d$'\t' > ${file}.fix
sed -i -e "s/TPM/${file}/g" ${file}.fix
sed -i -e 's/_sulfur_cycling_gene\.genes\.results//g' ${file}.fix
done

for file in *.fix
do 
echo $file
python /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/joint_htseq_output.py host.txt $file> tmp.txt
mv tmp.txt host.txt
done 

awk -F '\t' '{$2=""; sub(" ", " "); print}' host.txt > OWC_Sulfur_109.txt


```

##keep TPM of only verified hits 
```
#after comparing GhostKAAS and Conserved domain search, we 4902 hits were kept for metaT

#on MAC
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/metaT-NMDS-MRPP
nano sulful_marker_genes_4902list.txt

grep -w -f sulful_marker_genes_4902list.txt OWC_metaT2018_Sulfur_109.txt >OWC_metaT2018_Sulfur_109_clean4902.txt
```






##reference 
```
for prefix in $(cat ../OWC2014_trimmed_transcript_reads_prefix_uniq.txt)
do
bbmap.sh ref=../OWC_seqs_id_2014_2018_mcrA_ALL7June19.fna in=/home/projects/Wetlands/2014-2015_sampling/MetaT/sickled_transcript_reads/${prefix}.R1.fq in2=/home/projects/Wetlands/2014-2015_sampling/MetaT/sickled_transcript_reads/${prefix}.R2.fq xmtag=t ambiguous=random outm=./${prefix}_mcrA_2A.bam threads=12 -Xmx150g &>./${prefix}_mcrA_2A.log
samtools sort -@ 8 ${prefix}_mcrA_2A.bam > ${prefix}_mcrA_2A.bam.sorted
done

#outm only kept mapped reads, ok for TPM cal?

jgi_summarize_bam_contig_depths --outputDepth mcrA_metaT2014_mapping_depth_2A.txt *.bam.sorted
```
