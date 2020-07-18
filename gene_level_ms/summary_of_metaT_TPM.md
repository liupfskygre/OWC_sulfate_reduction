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

#reference scripts
```
#paste all/merge all _RSEM.genes.results files

Aug_N3_C1_D5_A_MG89_RSEM.genes.results 

for file in $(cat metaT2018JGI_reads_partI_II_list53.txt)
do 
#sed -i -e "s/TPM/TPM_$file/g" ${file}_MG89_RSEM.genes.results 
echo "${file}"
paste "${file}"_MG89_RSEM.genes.results tmp.txt>tmp.txt
done
paste *_MG89_RSEM.genes.results >tmp.txt

paste PS42_S_cout.out PS43_S_cout.out PS44_S_cout.out PS46_S_cout.out phz2_S_cout.out phz3_S_cout.out phz4_S_cout.out phz5_S_cout.out | awk -F "\t" '{print $1"\t"$2"\t"$4"\t"$6"\t"$8"\t"$10"\t"$12"\t"$14"\t"$16}' > PSI_htseqcounts.txt
```


##
```
for prefix in $(cat ../OWC2014_trimmed_transcript_reads_prefix_uniq.txt)
do
bbmap.sh ref=../OWC_seqs_id_2014_2018_mcrA_ALL7June19.fna in=/home/projects/Wetlands/2014-2015_sampling/MetaT/sickled_transcript_reads/${prefix}.R1.fq in2=/home/projects/Wetlands/2014-2015_sampling/MetaT/sickled_transcript_reads/${prefix}.R2.fq xmtag=t ambiguous=random outm=./${prefix}_mcrA_2A.bam threads=12 -Xmx150g &>./${prefix}_mcrA_2A.log
samtools sort -@ 8 ${prefix}_mcrA_2A.bam > ${prefix}_mcrA_2A.bam.sorted
done

#outm only kept mapped reads, ok for TPM cal?

jgi_summarize_bam_contig_depths --outputDepth mcrA_metaT2014_mapping_depth_2A.txt *.bam.sorted
```
