##
cd /home/ORG-Data-2/metaT2018JGI_reads


#reference preparation
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mappping_geneDB
cat ../fccB_mapping_metaT/PF09242.hmm.collection_id_Raw.fna ../hmmsearch_hits_assembly/all_sulfur_cycling_genes_Raw.fna >all_sulfur_cycling_genes_Raw.fna 

#all_sulfur_cycling_genes_Raw.fna, 6903


rsem-prepare-reference all_sulfur_cycling_genes_Raw.fna --bowtie2 all_sulfur_cycling_genes_Raw
```

#part I
```
for sample in $(cat /home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partI/metaT2018JGI_reads_partI_list.txt) #remove Aug_M1_C1_D1_A
do
echo ${sample}
rsem-calculate-expression --bowtie2 --no-qualities -p 12 --paired-end /home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partI/${sample}_R1_trimmed.fa.gz /home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partI/${sample}_R2_trimmed.fa.gz all_sulfur_cycling_genes_Raw ${sample}_Sulfur_RSEM
done 
```
sbatch metaT_mapping_partI.sh
Submitted batch job 7253

#part I
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
```

#
##call sbatch slurm.sh

part II
sbatch metaT_mapping_partII.sh
Submitted batch job 7255
```
#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=14 #number of cores you are requesting
#SBATCH --time=14-00:00:00 #max 14 days, HH:MM:SS if you need to include days do D-HH:MM:SS i.e.requesting 7 days is done as 7-00:00:00
#SBATCH --mem=128gb #How much memory you are requesting.  i.e. 500gb.  For 1Tb use 1024gb.  Notice this is lower case.
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=liupf@colostate.edu
#SBATCH --partition=wrighton-hi,wrighton-low #The options are: wrighton-hi,wrighton-low,wilkins-hi,wilkins-low,debug


#put code here

mrna_path="/home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partII"
#rsem-prepare-reference ../MAGs_3211_all_fixed.fna --bowtie2 MAGs_3211_all_fixed
#build already when mapping 2014-2015 data ==>../metaT_mapping2014_MAGs3211/MAGs_3211_all_fixed

for prefix in $(cat /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/metaT2018JGI_reads_partII_list.txt)
do

echo "${mrna_path}/${prefix}_R1_trimmed.fa"
echo "${mrna_path}/${prefix}_R2_trimmed.fa"
rsem-calculate-expression --bowtie2 -p 14 --num-threads 14 --paired-end  "${mrna_path}"/${prefix}_R1_trimmed.fa "${mrna_path}"/${prefix}_R2_trimmed.fa --no-qualities ./all_sulfur_cycling_genes_Raw ${prefix}_sulfur_cycling_gene
done
```


#
##call sbatch slurm.sh
sbatch metaT_mapping_partIII.sh
Submitted batch job 7254



part III
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
#prepare reference


mrna_path="/home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partIII"
#fastq file here :(

#part III, or use bowtie2 directly, rsem do not read in OWC_Sept_M1_C1_D3_C_interleaved_trimmed.fa.gz

#RSEM use this as parameter
#In particular, we use options '--sensitive --dpad 0 --gbar 99999999 --mp 1,1 --np 1 --score-min L,0,-0.1'

#rsem-prepare-reference ../MAGs_3211_all_fixed.fna --bowtie2 MAGs_3211_all_fixed
#build already when mapping 2014-2015 data ==>../metaT_mapping2014_MAGs3211/MAGs_3211_all_fixed

for prefix in $(cat /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/metaT2018JGI_reads_partIII_list.txt)
do
zcat ${mrna_path}/${prefix}_interleaved_trimmed.fq.gz > ./${prefix}_interleaved_trimmed.fq

 paste - - - - - - - - < ./${prefix}_interleaved_trimmed.fq | tee >(cut -f 1-4 | tr '\t' '\n' > ./${prefix}_R1_trimmed.fq) | cut -f 5-8 | tr '\t' '\n' > ./${prefix}_R2_trimmed.fq

echo "${mrna_path}/${prefix}_R1_trimmed.fq"
echo "${mrna_path}/${prefix}_R2_trimmed.fq"

rsem-calculate-expression --bowtie2 -p 14 --num-threads 14 --paired-end  ./${prefix}_R1_trimmed.fq ./${prefix}_R2_trimmed.fq ./all_sulfur_cycling_genes_Raw ${prefix}_sulfur_cycling_gene

rm ./${prefix}_R1_trimmed.fq
rm ./${prefix}_R2_trimmed.fq
rm ./${prefix}_interleaved_trimmed.fq
done

```

#sbatch metaT_mapping_partIII.sh
#Submitted batch job 7227
