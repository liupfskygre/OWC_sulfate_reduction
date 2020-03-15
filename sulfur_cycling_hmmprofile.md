#a collection of sulfur cycling related marker genes hmmprofile 

#ref
#download tigrfam on mac
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/database_model_profiles_archive/gtdbtk_marker/tigrfam



```


#kofam
```

cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Marker_gene_hmmprofile/sulfur_cycling_KOfam

touch sulfur_related_kofam.list #41 
for file in $(cat sulfur_related_kofam.list)
do
cp ./profiles/"${file}".hmm ./
done
# K00386.hmm: No such file or directory
#one duplicate (phsA==prsA), 39 items

/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Marker_gene_hmmprofile/sulfur_cycling_KOfam/profiles
```

#tigrfam
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Marker_gene_hmmprofile/sulfur_cycling_tigrfam


touch sulfur_related_tigrfam.list #

for file in $(cat sulfur_related_tigrfam.list)
do
cp /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/database_model_profiles_archive/gtdbtk_marker/TIGRFAMs_15.0_HMM/"${file}".HMM ./
done

cp: /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/database_model_profiles_archive/gtdbtk_marker/tigrfam/individual_hmms/TIGR04315.HMM: No such file or directory

/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/database_model_profiles_archive/gtdbtk_marker/tigrfam

```

#download pfam files
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Marker_gene_hmmprofile/sulfur_cycling_pfam
```




#searching db create
```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/dereplicated_genomes

[liupf@zenith prodigal_files]$ cat *.faa >~/combined_owc_3211_prodigal.faa
[liupf@zenith prodigal_files]$ pwd
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/prodigal_files
[liupf@zenith prodigal_files]$ 

cd ~
grep -c '>' combined_owc_3211_prodigal.faa. #use this as the database to search all 
9394752

```

#Adrienne created coverage file and relabel#
```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/read_mapping_vs_relabeled_derepped_genomes

/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes

????
[liupf@zenith relabeled_dereplicated_genomes]$ grep -c '>' combined_wetland_bins_genes.faa
9185226
[liupf@zenith relabeled_dereplicated_genomes]$ grep -c '>' combined_wetland_bins_scaffolds.fa
1805837

????
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/prodigal_files
[liupf@zenith prodigal_files]$ cat *.faa|grep -c '>' -
9394752 # why this is different from the combined bins_genes

[liupf@zenith relabeled_dereplicated_genomes]$ cat *.fa|grep -c '>' -
1805837 #the same as the combined 


##besides MBNT-15
what are the DRAM for, just to dram all genomes we need to use later?

##????
ls *.fna|wc -l
3210

```
