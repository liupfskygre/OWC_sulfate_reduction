##metaG16 mapping to all genes
#mkdir 
```
metaG16_mapping_to_all_genes
```
#
```
bowtie2 -q --phred33 --sensitive --dpad 0 --gbar 99999999 --mp 1,1 --np 1 --score-min L,0,-0.1 -I 1 -X 1000 --no-mixed --no-discordant -p 14 -k 200 -x ../MAGs_3211_all_fixed -1 /home/ORG-Data-2/metaT_CU_denver2019/OWC_metaT2018_CU_Denver/Aug_M1_C1_D1_C_trimmed.R1.fq.gz -2 /home/ORG-Data-2/metaT_CU_denver2019/OWC_metaT2018_CU_Denver/Aug_M1_C1_D1_C_trimmed.R2.fq.gz | samtools view -S -b -o Aug_M1_C1_D1_C_CUD_MAGs3211_RSEM.temp/Aug_M1_C1_D1_C_CUD_MAGs3211_RSEM.bam -


```
