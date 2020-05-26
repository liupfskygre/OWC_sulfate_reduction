##

## Zixibacteria

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

## Desulfobacterota_BSN033 

```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins

cd /home/projects/Wetlands/sulfur_cycling_analysis/MAGs_metabolic_profiling
mkdir Desulfobacterota_BSN033
cd Desulfobacterota_BSN033
nano Desulfobacterota_BSN033.txt


for file in $(cat Desulfobacterota_BSN033.txt)
do
cat /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"$file"_DRAMOUT/annotations.tsv >> Desulfobacterota_BSN033_annotation.tsv
done


head -1 Desulfobacterota_BSN033_annotation.tsv >header.txt
 

grep -v "fasta" Desulfobacterota_BSN033_annotation.tsv > tmp.txt


cat header.txt tmp.txt > Desulfobacterota_BSN033_annotation_Fix.tsv

DRAM.py distill -i Desulfobacterota_BSN033_annotation_Fix.tsv -o Desulfobacterota_BSN033
```

## Desulfobacterota_others 

```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins

cd /home/projects/Wetlands/sulfur_cycling_analysis/MAGs_metabolic_profiling
mkdir Desulfobacterota_others
cd Desulfobacterota_others
nano Desulfobacterota_others.txt


for file in $(cat Desulfobacterota_others.txt)
do
cat /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"$file"_DRAMOUT/annotations.tsv >> Desulfobacterota_others_annotation.tsv
done


head -1 Desulfobacterota_others_annotation.tsv >header.txt
 

grep -v "fasta" Desulfobacterota_others_annotation.tsv > tmp.txt


cat header.txt tmp.txt > Desulfobacterota_others_annotation_Fix.tsv

DRAM.py distill -i Desulfobacterota_others_annotation_Fix.tsv -o Desulfobacterota_others
```

## Acidobacteriota 

```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins

cd /home/projects/Wetlands/sulfur_cycling_analysis/MAGs_metabolic_profiling
mkdir Acidobacteriota
cd Acidobacteriota
nano Acidobacteriota.txt


for file in $(cat Acidobacteriota.txt)
do
cat /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"$file"_DRAMOUT/annotations.tsv >> Acidobacteriota_annotation.tsv
done


head -1 Acidobacteriota_annotation.tsv >header.txt
 

grep -v "fasta" Acidobacteriota_annotation.tsv > tmp.txt


cat header.txt tmp.txt > Acidobacteriota_annotation_Fix.tsv

DRAM.py distill -i Acidobacteriota_annotation_Fix.tsv -o Acidobacteriota
```

##
## Chloroflexota 

```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins

cd /home/projects/Wetlands/sulfur_cycling_analysis/MAGs_metabolic_profiling
mkdir Chloroflexota
cd Chloroflexota
nano Chloroflexota.txt


for file in $(cat Chloroflexota.txt)
do
cat /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"$file"_DRAMOUT/annotations.tsv >> Chloroflexota_annotation.tsv
done


head -1 Chloroflexota_annotation.tsv >header.txt
 

grep -v "fasta" Chloroflexota_annotation.tsv > tmp.txt


cat header.txt tmp.txt > Chloroflexota_annotation_Fix.tsv

DRAM.py distill -i Chloroflexota_annotation_Fix.tsv -o Chloroflexota
```

## UBP10 

```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins

cd /home/projects/Wetlands/sulfur_cycling_analysis/MAGs_metabolic_profiling
mkdir UBP10
cd UBP10
nano UBP10.txt


for file in $(cat UBP10.txt)
do
cat /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"$file"_DRAMOUT/annotations.tsv >> UBP10_annotation.tsv
done


head -1 UBP10_annotation.tsv >header.txt
 

grep -v "fasta" UBP10_annotation.tsv > tmp.txt


cat header.txt tmp.txt > UBP10_annotation_Fix.tsv

DRAM.py distill -i UBP10_annotation_Fix.tsv -o UBP10
```
