##

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

#https://github.com/liupfskygre/OWC_metaG16_ana2019/blob/master/metaT/metaT_OWC2014_mapping_mcrA.md


2)metaT2018
#

```


#slurm scripts
```


```
