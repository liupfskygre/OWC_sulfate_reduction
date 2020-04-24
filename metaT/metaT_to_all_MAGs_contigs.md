##

## update notes
```
1) test the use dram out scafolds as referecence first, which is compatabile with gff from dram, see Adrienne's notes below
```

##reference preparation

#
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


#slurm scripts
```


```


## Adrienne pipline
```
/home/projects/Wetlands/All_genomes/scripts
The run in this order:
bbmap.
samtools
fix bamfiles
filter
htseq-count
10:59
I’ll put a link in there to the bbmap database and the gff file.
11:01
These are slurm scripts so you need to edit them but you can see what I’m doing here.  The fix_bamfiles is because there is a description line in the fasta file that doesn’t match the gff file.  It’s not a typical step.

#/home/projects/Wetlands/All_genomes/scripts

/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/metaT_mappings/groupII_reads/all_bins_combined_3211db_scaffolds.fna


grep -c '>' /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/metaT_mappings/groupII_reads/all_bins_combined_3211db_scaffolds.fna
1804989==>different from 1805837; this means 

```

## testing new references, using RSEM based on bowtie2
#
```
sbatch metaT_2014_to_MAGs3211_test.sh
Submitted batch job 3593



```

