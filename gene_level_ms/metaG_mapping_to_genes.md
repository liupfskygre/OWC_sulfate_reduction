##metaG16 mapping to all genes
#mkdir 
```
metaG16_mapping_to_all_genes

/home/projects/Wetlands/sulfur_cycling_analysis/metaG16_mapping_to_all_genes

cp ../metaT_mappping_geneDB/all_sulfur_cycling_genes_Raw.fna ./

#6903

#build index
rsem-prepare-reference all_sulfur_cycling_genes_Raw.fna --bowtie2 all_sulfur_cycling_genes_Raw
```
#
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/metaG16_mapping_to_all_genes
nano file_path_link.txt

nano metaG_mapping_S_genes.sh

sbatch metaG_mapping_S_genes.sh
Submitted batch job 7258

```
```
#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=14 #number of cores you are requesting
#SBATCH --time=14-00:00:00 #max 14 days, HH:MM:SS if you need to include days do D-HH:MM:SS i.e.requesting 7 days is done as 7-00:00:00
#SBATCH --mem=128gb #How much memory you are requesting.  i.e. 500gb.  For 1Tb use 1024gb.  Notice this is lower case.
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=liupf@colostate.edu
#SBATCH --partition=wrighton-hi,wrighton-low #The options are: wrighton-hi,wrighton-low,wilkins-hi,wilkins-low,debug

IFS=$'\n'
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

#convert fastq file to trimmed fa
zcat ${path}/${file} > ./temp_interleaved.fq

sickle pe -c temp_interleaved.fq -t sanger -M temp_interleaved_trimmed.fq
fq2fa --paired --filter temp_interleaved_trimmed.fq temp_interleaved_trimmed.fa
rm temp_interleaved.fq
rm temp_interleaved_trimmed.fq

reformat.sh in=temp_interleaved_trimmed.fa out1=temp_interleaved_trimmed_R1.fa out2=temp_interleaved_trimmed_R2.fa

rsem-calculate-expression --bowtie2 -p 24 --num-threads 24 --paired-end temp_interleaved_trimmed_R1.fa temp_interleaved_trimmed_R2.fa --no-qualities ./all_sulfur_cycling_genes_Raw ${sample}_S_cycling_gene

done
rm temp_interleaved_trimmed.fa
rm temp_interleaved_trimmed_R1.fa 
rm temp_interleaved_trimmed_R2.fa 
echo "finish all"
unset IFS
```

#
```
May_M1_C1_D6
mv _S_cycling_gene.genes.results May_M1_C1_D6_S_cycling_gene.genes.results
mv _S_cycling_gene.isoforms.results May_M1_C1_D6_S_cycling_gene.isoforms.results
mv _S_cycling_gene.transcript.bam May_M1_C1_D6_S_cycling_gene.transcript.bam
mv _S_cycling_gene.stat May_M1_C1_D6_S_cycling_gene.stat
```
