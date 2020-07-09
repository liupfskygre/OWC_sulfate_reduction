##metaG16 mapping to all genes
#mkdir 
```
metaG16_mapping_to_all_genes

/home/projects/Wetlands/sulfur_cycling_analysis/metaG16_mapping_to_all_genes

cat ../fccB_mapping_metaT/PF09242.hmm.collection_id_Raw.fna ../metaT_mappping_geneDB/all_sulfur_cycling_genes_Raw.fna >all_sulfur_cycling_genes_Raw_final6903.fna

#6881??
```
#
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/metaG16_mapping_to_all_genes
nano file_path_link.txt

for sample in $(cat file_path_link.txt)
do
echo ${sample}
#get sample name
name=$(echo ${sample}| cut -f1 -d$'\t' )

file=$(echo ${sample}| cut -f2 -d$'\t' )

#get sample prodigal.faa
path=$(echo ${sample}| cut -f3 -d$'\t')

#checkpoint
echo ${name} ${file} ${path}

bowtie2 -q --phred33 --sensitive --dpad 0 --gbar 99999999 --mp 1,1 --np 1 --score-min L,0,-0.1 -I 1 -X 1000 --no-mixed --no-discordant -p 14 -k 200 -x ../ref.fna --interleaved ${path}/${file} | samtools view -S -b -o ${sample}.bam -

done
```
