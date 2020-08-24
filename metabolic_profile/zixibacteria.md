##zixibacteria

```
source /opt/Miniconda2/miniconda2/bin/activate DRAM

/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins

cd /home/projects/Wetlands/sulfur_cycling_analysis

cd /home/projects/Wetlands/sulfur_cycling_analysis/MAGs_metabolic_profiling/zixbacteria

for file in $(cat zixbacteria.txt)
do
cat /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"$file"_DRAMOUT/annotations.tsv >> zixbacteria_annotation.tsv
done


head -1 zixbacteria_annotation.tsv >header.txt
 

grep -v "fasta" zixbacteria_annotation.tsv > tmp.txt


cat header.txt tmp.txt > zixbacteria_annotation_Fix.tsv

DRAM.py distill -i zixbacteria_annotation_Fix.tsv -o zixbacteria


```
