##
cd /home/ORG-Data-2/metaT2018JGI_reads

cd /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mappping_geneDB

#reference preparation
rsem-prepare-reference all_sulfur_cycling_genes_Raw.fna --bowtie2 all_sulfur_cycling_genes_Raw


#
for sample in $(cat /home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partI/metaT2018JGI_reads_partI_list.txt) #remove Aug_M1_C1_D1_A
do
echo ${sample}
rsem-calculate-expression --bowtie2 --no-qualities -p 12 --paired-end /home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partI/${sample}_R1_trimmed.fa.gz /home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partI/${sample}_R2_trimmed.fa.gz all_sulfur_cycling_genes_Raw ${sample}_Sulfur_RSEM
done 

Submitted batch job 7224


#slurm scripts backup
```
#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=14 #number of cores you are requesting
#SBATCH --time=14-00:00:00 #max 14 days, HH:MM:SS if you need to include days do D-HH:MM:SS i.e.requesting 7 days is done as 7-00:00:00
#SBATCH --mem=128gb #How much memory you are requesting.  i.e. 500gb.  For 1Tb use 1024gb.  Notice this is lower case.
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=liupf@colostate.edu
#SBATCH --partition=wrighton-hi,wrighton-low #The options are: wrighton-hi,wrighton-low,wilkins-hi,wilkins-low,debug


#put your code block here for running
#prepare reference, done

mrna_path="/home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partI"
#rsem-prepare-reference ../MAGs_3211_all_fixed.fna --bowtie2 MAGs_3211_all_fixed
#build already when mapping 2014-2015 data ==>../metaT_mapping2014_MAGs3211/MAGs_3211_all_fixed

for prefix in $(cat /home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partI/metaT2018JGI_reads_partI_list.txt)
do

echo "${mrna_path}/${prefix}_R1_trimmed.fa.gz"
echo "${mrna_path}/${prefix}_R2_trimmed.fa.gz"
rsem-calculate-expression --bowtie2 -p 14 --num-threads 14 --paired-end "${mrna_path}"/${prefix}_R1_trimmed.fa.gz "${mrna_path}"/${prefix}_R2_trimmed.fa.gz --no-qualities ./all_sulfur_cycling_genes_Raw ${prefix}_sulfur_cycling_gene
done


#
##call sbatch slurm.sh

```
