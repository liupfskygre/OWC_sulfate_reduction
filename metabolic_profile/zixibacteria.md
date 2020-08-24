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


```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/genom_annotation/zixbacteria/zixbacteria
awk -F '\t' '$8!=""' zixbacteria_annotation_Fix.tsv  >Zixibacteria_annotation_w_KO.tsv

cat Zixibacteria_annotation_w_KO.tsv|cut -f2,8 -d$'\t' >Zixibacteria_annotation_w_KO28.tsv


head Desulfobacterota_BSN033_annotation_w_KO28.tsv
sed -i -e 's/\,/\t/g' Zixibacteria_annotation_w_KO28.tsv

awk -F '\t' 'BEGIN{ORS=RS="\n"} {for(a=2; a<=NF;) print $1"\t"$(a++)}' Zixibacteria_annotation_w_KO28.tsv > Zixibacteria_annotation_w_KO28_fix.tsv

#sed -i -e 1b -e '/fasta/d' Zixibacteria_annotation_w_KO28_fix.tsv
sed -i -e '/fasta/d' Zixibacteria_annotation_w_KO28_fix.tsv

sed -i -e 's/_/;/g' Zixibacteria_annotation_w_KO28_fix.tsv

awk '{print $1"_"NR"\t"$2}' Zixibacteria_annotation_w_KO28_fix.tsv > Zixibacteria_annotation_w_KO28_fix_wN.tsv

conda activate Decoder
KEGG-decoder --input Zixibacteria_annotation_w_KO28_fix_wN.tsv --output Zixibacteria_FUNCTION_OUT.list --vizoption interactive


```
