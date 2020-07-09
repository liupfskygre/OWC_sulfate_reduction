##metaG16 mapping to all genes
#mkdir 
```
metaG16_mapping_to_all_genes

/home/projects/Wetlands/sulfur_cycling_analysis/metaG16_mapping_to_all_genes

cp ../metaT_mappping_geneDB/all_sulfur_cycling_genes_Raw.fna ./

#6903

#build index
bowtie2-build all_sulfur_cycling_genes_Raw.fna all_sulfur_cycling_genes_Raw
```
#
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/metaG16_mapping_to_all_genes
nano file_path_link.txt

nano metaG_mapping_S_genes.sh

sbatch metaG_mapping_S_genes.sh
Submitted batch job 7256

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

zcat ${path}/${file} > ./temp_interleaved_trimmed.fq


bowtie2 --interleaved ./temp_interleaved_trimmed.fq -q --phred33 --sensitive --dpad 0 --gbar 99999999 --mp 1,1 --np 1 --score-min L,0,-0.1 -I 1 -X 1000 --no-mixed --no-discordant -p 14 -k 200 -x ./all_sulfur_cycling_genes_Raw | samtools view -bS - >${name}.bam

done
echo "finish all"
unset IFS
```
