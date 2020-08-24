##DRAM annotation of Nitrospirota

```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/DRAM_directories
all_bins_combined_3217db_annotations_gtdbtk_rel95.txt

/home/projects/Wetlands/sulfur_cycling_analysis/MAGs_metabolic_profiling/Nitrospirota

grep -e 'Nitrospirota' /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_3217db_annotations_gtdbtk_rel95.txt >Nitrospirota_3217db_annotations_gtdbtk_rel95.txt

#An old copy put here baseon on r89
#
grep -e 'Nitrospirota' /home/projects/Wetlands/sulfur_cycling_analysis/DRAM_OUT/all_bins_combined_annotations_3211db.tsv >Nitrospirota_3211db_annotations_gtdbtk_rel89.txt


```


#fast scan with kegg-decorder
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/genom_annotation/p__Nitrospirota_c__UBA9217

awk -F '\t' '$8!=""' Nitrospirota_3217db_annotations_gtdbtk_rel95.txt  >Nitrospirota_annotation_w_KO.tsv

cat Nitrospirota_annotation_w_KO.tsv|cut -f2,8 -d$'\t' >Nitrospirota_annotation_w_KO28.tsv


head Desulfobacterota_BSN033_annotation_w_KO28.tsv
sed -i -e 's/\,/\t/g' Nitrospirota_annotation_w_KO28.tsv

awk -F '\t' 'BEGIN{ORS=RS="\n"} {for(a=2; a<=NF;) print $1"\t"$(a++)}' Nitrospirota_annotation_w_KO28.tsv > Nitrospirota_annotation_w_KO28_fix.tsv

#sed -i -e 1b -e '/fasta/d' Nitrospirota_annotation_w_KO28_fix.tsv
sed -i -e '/fasta/d' Nitrospirota_annotation_w_KO28_fix.tsv

sed -i -e 's/_/;/g' Nitrospirota_annotation_w_KO28_fix.tsv

awk '{print $1"_"NR"\t"$2}' Nitrospirota_annotation_w_KO28_fix.tsv > Nitrospirota_annotation_w_KO28_fix_wN.tsv

conda activate Decoder
KEGG-decoder --input Nitrospirota_annotation_w_KO28_fix_wN.tsv --output Nitrospirota_FUNCTION_OUT.list --vizoption interactive

#UBA9217

KEGG-decoder --input UBA9217_annotation_w_KO28_fix_wN.tsv --output UBA9217_FUNCTION_OUT.list --vizoption interactive
```
