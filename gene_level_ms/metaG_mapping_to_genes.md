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

```

```
sbatch metaG16_4902_gene_partI.sh 
Submitted batch job 7583

```


```
cd /home/projects/Wetlands/sulfur_cycling_analysis/metaG16_mapping_to_all_genes


for file in *.bam
do
samtools sort -@ 2 ${file} > "${file}".sorted
done

#
jgi_summarize_bam_contig_depths --outputDepth metaT_mapping_MGdb89_depth.txt *.sorted.bam

#use cut and R extract data we need

rm *.sam

mv *sorted.bam sorted_bam_backup/


```

#Submitted batch job Submitted batch job 7548


#updated
```
#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=10 #number of cores you are requesting
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
echo "${name}.fq removed"


reformat.sh in=temp_interleaved_trimmed.fa out1=temp_interleaved_trimmed_R1.fa out2=temp_interleaved_trimmed_R2.fa ow=t  #-Xmx112g

rsem-calculate-expression --bowtie2 -p 10 --num-threads 10 --paired-end temp_interleaved_trimmed_R1.fa temp_interleaved_trimmed_R2.fa --no-qualities ./all_sulfur_cycling_genes_Raw ${name}_S_cycling_gene &>${name}.log

ls -1 *.fa >>${name}.log

#filter and sorted
samtools view -b -h -q 20 -F 4 -@ 8 "${name}"_S_cycling_gene.transcript.bam > "${name}"_S_cycling_gene.filter.bam
samtools sort -@ 8 "${name}"_S_cycling_gene.filter.bam  -o "${name}"_S_cycling_gene.filter.sorted.bam

rm "${name}"_S_cycling_gene.transcript.bam
rm "${name}"_S_cycling_gene.filter.bam

#samtools view -b -h -q 20 -F 4 -@ 8 Aug_M1_C1_D1_S_cycling_gene.transcript.bam > Aug_M1_C1_D1_S_cycling_gene.filter.bam
#samtools sort -@ 8 Aug_M1_C1_D1_S_cycling_gene.filter.bam  -o Aug_M1_C1_D1_S_cycling_gene.filter_sorted.bam
#jgi_summarize_bam_contig_depths --outputDepth Aug_M1_C1_D1_S_cycling_gene_depth.txt Aug_M1_C1_D1_S_cycling_gene.filter_sorted.bam

#Aug_M1_C1_D1_S_cycling_gene.transcript.bam
#"${name}"_S_cycling_gene.transcript.bam


rm temp_interleaved_trimmed.fa
rm temp_interleaved_trimmed_R1.fa 
rm temp_interleaved_trimmed_R2.fa 
echo "${name} removed"

done

echo "finish all"
unset IFS
```

