##

## update notes
```
1) test the use dram out scafolds as referecence first, which is compatabile with gff from dram, see Adrienne's notes below

```

## reference preparation
#use dramout seqs

**version1**
```
#0 mk wkdir
mkdir /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping

#get the dram scaffolds for all genomes 
for file in *_DRAMOUT
do 
cat ${file}/scaffolds.fna >> all_3211_DRAMout_scaffolds.fna
printf "\n" >>all_3211_DRAMout_scaffolds.fna
done
grep -c '>' all_3211_DRAMout_scaffolds.fna
#1804989, this is different from what we concatenate all fills together (1805837), since dram have a 2500kb cutoff

# this is also why genes are different between dram (2500 kb) and prodigal (no cutoff)
#
#the same as: all_bins_combined_3211db_scaffolds.fna; keep this one 

#reference file1, dramout scaffolds
/home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/all_3211_DRAMout_scaffolds.fna

#reference file2
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/metaT_mappings/groupII_reads/all_bins_combined_3211db_scaffolds.fna

#dramout Genes/CDS
/home/projects/Wetlands/sulfur_cycling_analysis/all_3211_genes_DRAM_aa.faa
/home/projects/Wetlands/sulfur_cycling_analysis/all_3211_genes_DRAM.fna

```

#mapping
```
#ref
/home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping 


#mrna 

1)metaT2014-2015
#reference
#https://github.com/liupfskygre/OWC_metaG16_ana2019/blob/master/metaT/metaT_OWC2014_mapping_mcrA.md

#list of metaT2014-2015
# /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/OWC_megahit_mcrA_coverage, 
OWC2014_trimmed_transcript_reads_prefix_uniq.txt

#sample location
#/home/projects/Wetlands/2014-2015_sampling/MetaT/sickled_transcript_reads
mkdir metaT_mapping2014_MAGs3211

sbatch metaT_2014_to_MAGs3211.sh  #--> slurm-3584.out 

2)metaT2018

#metaT 2018 part I
#reads
# /home/ORG-Data-2/metaT2018JGI_reads
#/home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partI
sbatch metaT_2018_to_MAGs3211_partI.sh

#metaT 2018 part II


#part III 
_interleaved_trimmed.fa.gz

```


#slurm scripts
```


```


## Adrienne pipline note
```
/home/projects/Wetlands/All_genomes/scripts
The run in this order:
bbmap.
samtools
fix bamfiles
filter
htseq-count

fix_headers.py 
run_bbmap.sh #bbmap mapping
run_samtools.sh #convert and sort bam file
run_fix_bamfiles.sh
run_filter.sh #reformat.sh -Xmx100g idfilter=0.95
run_htseq-count.sh # 

I’ll put a link in there to the bbmap database and the gff file.
## /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/all_bins_combined_genes_3211db.gff

11:01
These are slurm scripts so you need to edit them but you can see what I’m doing here.  The fix_bamfiles is because there is a description line in the fasta file that doesn’t match the gff file.  It’s not a typical step.

#/home/projects/Wetlands/All_genomes/scripts

/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/metaT_mappings/groupII_reads/all_bins_combined_3211db_scaffolds.fna


grep -c '>' /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/metaT_mappings/groupII_reads/all_bins_combined_3211db_scaffolds.fna

#1804989==>different from 1805837 directly from conconated fa files (dram have a 2500kb cut off)
```

## testing new references and Adrienne's pipeline with gff file, using RSEM based on bowtie2
#
```
1) mapping with bowtie2
sbatch metaT_2014_to_MAGs3211_test.sh
Submitted batch job 3593

2）filtering with MAGQ


3)htseq count number of reads
/home/projects/Wetlands/All_genomes/scripts/run_htseq-count.sh 

/home/anarrowe/.local/bin/htseq-count -s no -f bam -t CDS -i ID -m union -r name  Aug_M1_C1_D1_A_MAGs3211_RSEM.transcript.bam /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/all_bins_combined_genes_3211db.gff > Aug_M1_C1_D1_A_htseq_counts.out 
```



## archive log
**version1**
```
#0 mk wkdir
mkdir /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping



#1 copy all Adrienne reanmed files 
cp /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/*.fa /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping 

#fixheader of genomes
1), remove added header (prefix \t conitgs ==> contigs)
for file in *.fa
do
sed -e 's/>.* \(.*\)/>\1/g' ${file} >"${file%.*}".fasta
done

2), fix header of genomes start with numbers (keep only contigs, unify as the others)
for file in $(cat number_started_MAGs.txt)
do
sed -i -e "s/${file}//1" "${file}".fasta
done
#
for file in $(cat number_started_MAGs.txt)
do
sed -i -e "s/\.fa${file}_//1" "${file}".fasta
done

3)
for file in *.fasta
do
a="${file%.*}"
echo ${a}
sed -i -e "s/>\(.*\)/>${a}_;\1/g" ${file}
done

4) cat all refix fasta files to the final references
cat *.fasta >MAGs_3211_all_fixed.fna
#check num of conitgs
let a=0
let b=0
for file in *.fasta
do
b=$(grep -c '>' "${file}")
a=`expr "$a" + "$b"`
done
echo $a

[liupf@zenith metaT_mapping]$ echo $a
1805837
[liupf@zenith metaT_mapping]$ grep -c '>' *.fna
1805837

sed -i -e 's/\.fa//g' MAGs_3211_all_fixed.fna
```

#mapping
```
#ref
/home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping 
MAGs_3211_all_fixed.fna

#mrna 

1)metaT2014-2015
#reference
#https://github.com/liupfskygre/OWC_metaG16_ana2019/blob/master/metaT/metaT_OWC2014_mapping_mcrA.md

#list of metaT2014-2015
# /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/OWC_megahit_mcrA_coverage, 
OWC2014_trimmed_transcript_reads_prefix_uniq.txt

#sample location
#/home/projects/Wetlands/2014-2015_sampling/MetaT/sickled_transcript_reads
mkdir metaT_mapping2014_MAGs3211

sbatch metaT_2014_to_MAGs3211.sh  #--> slurm-3584.out 

2)metaT2018

#metaT 2018 part I
#reads
# /home/ORG-Data-2/metaT2018JGI_reads
#/home/ORG-Data-2/metaT2018JGI_reads/metaT2018JGI_reads_partI
sbatch metaT_2018_to_MAGs3211_partI.sh

#metaT 2018 part II


#part III 
_interleaved_trimmed.fa.gz

```


