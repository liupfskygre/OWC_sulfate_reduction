


#1, download MAGs of Desulfobacterota_BSN033 from NCBI

#2, dram on MAGs of interests
```


```


#comparative on metabolic profile data


#updatead genomes annotation here
```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_3217db_checkM_and_gtdbtk_rel95.txt


```


#genome annotation manually checking

```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/genom_annotation/Desulfobacterota_BSN033/Desulfobacterota_BSN033

#1 Aug_N3_C1_D5_30Gb_idba_metabat.11
grep -w 'Aug_N3_C1_D5_30Gb_idba_metabat.11' Desulfobacterota_BSN033_annotation.tsv > Aug_N3_C1_D5_30Gb_idba_metabat.11_annotation.tsv
cat Aug_N3_C1_D5_30Gb_idba_metabat.11_annotation.tsv|cut -f1,8 -d$'\t' > Aug_N3_C1_D5_30Gb_idba_metabat.11_annotation28.tsv

#2 
grep -w 'Aug_M1_C1_D2_megahit_metabat.26' Desulfobacterota_BSN033_annotation.tsv > Aug_M1_C1_D2_megahit_metabat.26_annotation.tsv
cat Aug_M1_C1_D2_megahit_metabat.26_annotation.tsv|cut -f1,8 -d$'\t' > Aug_M1_C1_D2_megahit_metabat.26_annotation_k28.tsv

```
