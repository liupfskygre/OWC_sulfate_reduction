## checking the metabolic annotaiton of fccB gene
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/fccB_checking

#run metabolic on previous genes from MAGs by using

# FCSD-flav_bind (PF09242) - Pfam: Family

perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/fccB_checking -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/fccB_checking

#METABOLIC reject all hits of fccB
```



```
#change to 21.4 of tc in the hmm_table_template.txt

rerun, recovered hits, but no sequences recovered
```

#re-run METABOLIC based on the fccB new TC
```
for file in $(cat sample_sub_dir.txt);
do echo ${file}
mv ./${file}/*.faa ./Congtis
done

#too big for my mac
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/Congtis -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/Congtis

for file in *.faa
do echo ${file}
sed -i -e "s/>/>"${file%.*}"~~/g" ${file}
done

#change it b
for file in *.faa
do echo ${file}
sed -i -e "s/>"${file%.*}"~~/>/g" ${file}
done

for file in $(cat sample_faa_list.txt)
do echo ${file}
mv ./Congtis/${file} ./${file}_contigs
done


for file in $(cat sample_sub_dir.txt)
do 
echo ${file}
mkdir ./${file}/metabolic_run2
mv ./${file}/Each_HMM_Amino_Acid_Sequence ./${file}/metabolic_run2
mv ./${file}/intermediate_files ./${file}/metabolic_run2
mv ./${file}/KEGG_identifier_result ./${file}/metabolic_run2
mv ./${file}/METABOLIC_Figures ./${file}/metabolic_run2
mv ./${file}/METABOLIC_Figures_Input ./${file}/metabolic_run2
mv ./${file}/METABOLIC_result_each_spreadsheet ./${file}/metabolic_run2
mv ./${file}/METABOLIC_result.xlsx ./${file}/metabolic_run2
done
```

#
```
cd /Users/pengfeiliu/software/METABOLIC-master

for file in $(cat /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/metaG16_prodigal.txt) 
do 
echo $file
perl METABOLIC-G.pl -t 4 -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/${file}_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/${file}_contigs
done
```

#get all fccB sequences together
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length

for folder in $(cat sample_sub_dir.txt)
do 
cd ./${folder}/Each_HMM_Amino_Acid_Sequence/
pwd
for file in *.faa
do
cp ${file} /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/Each_HMM_Amino_Acid_Sequence/${folder}_${file}
done
cd ../../
done

#
cd Each_HMM_Amino_Acid_Sequence 
for file in PF09242.hmm.collection.faa
do 
cat *${file} > ../"${file}".faa
done 

for file in *.faa.faa; do mv ${file} "${file%.*}"; done


grep -e '>' PF09242.hmm.collection.faa >  PF09242.hmm.collection_id.txt

sed -i -e 's/>//g' PF09242.hmm.collection_id.txt 
#830 sequences
pullseq -i PF09242.hmm.collection.faa  -m 53 >PF09242.hmm.collection_len.faa 

grep -e '>' PF09242.hmm.collection_len.faa  >  PF09242.hmm.collection_id.txt
sed -i -e 's/>//g' PF09242.hmm.collection_id.txt #812 sequences
```


#following are del, 2020July-9

#do mapping on fccB along
```
cd hmmsearch_hits_assembly
#reference sequence
pullseq -i all_prodigal_tmp.fna  -n PF09242.hmm.collection_id.txt > PF09242.hmm.collection_id_Raw.fna
grep -c '>' PF09242.hmm.collection_id_Raw.fna 
#812

#mkdir of fccB mapping
cd /home/projects/Wetlands/sulfur_cycling_analysis/fccB_mapping_metaT
mv ../hmmsearch_hits_assembly/PF09242.hmm.collection_id_Raw.fna ./


#reference prearation
rsem-prepare-reference PF09242.hmm.collection_id_Raw.fna --bowtie2 PF09242_hmm_Raw

```

#do mapping slurm files
part I
```
#nano PF09242_metaT.sh

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

for prefix in $(cat /home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partI/metaT2018JGI_reads_partI_list.txt)
do

echo "${mrna_path}/${prefix}_R1_trimmed.fa.gz"
echo "${mrna_path}/${prefix}_R2_trimmed.fa.gz"
rsem-calculate-expression --bowtie2 -p 14 --num-threads 14 --paired-end "${mrna_path}"/${prefix}_R1_trimmed.fa.gz "${mrna_path}"/${prefix}_R2_trimmed.fa.gz --no-qualities ./PF09242_hmm_Raw ${prefix}_fccB_PF09242
done


```

#part II
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
rsem-calculate-expression --bowtie2 -p 14 --num-threads 14 --paired-end  "${mrna_path}"/${prefix}_R1_trimmed.fa "${mrna_path}"/${prefix}_R2_trimmed.fa --no-qualities ./PF09242_hmm_Raw ${prefix}_fccB_PF09242
done


#

```
#part III
```
#nano PF09242_metaT_III.sh


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

rsem-calculate-expression --bowtie2 -p 14 --num-threads 14 --paired-end  ./${prefix}_R1_trimmed.fq ./${prefix}_R2_trimmed.fq ./PF09242_hmm_Raw ${prefix}_fccB_PF09242

rm ./${prefix}_R1_trimmed.fq
rm ./${prefix}_R2_trimmed.fq
rm ./${prefix}_interleaved_trimmed.fq
done


```
