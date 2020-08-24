


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


O3C3D4_DDIG_MN.279


grep -w 'O3C3D4_DDIG_MN.279' Desulfobacterota_BSN033_annotation.tsv > O3C3D4_DDIG_MN.279_annotation.tsv
cat O3C3D4_DDIG_MN.279_annotation.tsv|cut -f1,8 -d$'\t' > O3C3D4_DDIG_MN.279_annotation_k28.tsv


grep -w 'O3C3D3_DDIG_MN.643' Desulfobacterota_BSN033_annotation.tsv > O3C3D3_DDIG_MN.643_annotation.tsv
cat O3C3D3_DDIG_MN.643_annotation.tsv|cut -f1,8 -d$'\t' > O3C3D3_DDIG_MN.643_annotation_k28.tsv


#O3C3D3_metabat_w_DDIG_2.5k.188
grep -w 'O3C3D3_metabat_w_DDIG_2.5k.188' Desulfobacterota_BSN033_annotation.tsv > O3C3D3_metabat_w_DDIG_2.5k.188_annotation.tsv
cat O3C3D3_metabat_w_DDIG_2.5k.188_annotation.tsv|cut -f1,8 -d$'\t' > O3C3D3_metabat_w_DDIG_2.5k.188_annotation_k28.tsv

#OWC_subtractive_megahit_Surface_metabat.910
grep -w 'OWC_subtractive_megahit_Surface_metabat.910' Desulfobacterota_BSN033_annotation.tsv > OWC_subtractive_megahit_Surface_metabat.910_annotation.tsv
cat OWC_subtractive_megahit_Surface_metabat.910_annotation.tsv|cut -f1,8 -d$'\t' > OWC_subtractive_megahit_Surface_metabat.910_annotation_k28.tsv

#Nov_Mud_rebin_metabat.4
grep -w 'Nov_Mud_rebin_metabat.4' Desulfobacterota_BSN033_annotation.tsv > Nov_Mud_rebin_metabat.4_annotation.tsv
cat Nov_Mud_rebin_metabat.4_annotation.tsv|cut -f1,8 -d$'\t' > Nov_Mud_rebin_metabat.4_annotation_k28.tsv

```
