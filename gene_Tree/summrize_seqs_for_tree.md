# checking and fix the name header to all DRAMOUT way#

## what did Adrienne do for MAGs renaming ##
```
1) add partial name of MAGs (w/o) MAGs id and .fa to the seq header, +\t scaffold ID
e.g.
===> OWC_subtractive_megahit_Surface_metabat_k121_44274654  k121_44274654

#a master file with MAGs ID ===> scaffold ID is created



here: 
/home/projects/Wetlands/sulfur_cycling_analysis/sulfur_cycling_owc_hmmsearh 

wetlands_db_contigs_to_bins.tsv




2)based on this we have prodigal predicition, get gene names like this 

===> OWC_subtractive_megahit_Surface_metabat_k121_44274654_13

all combined genes and protein files are here


[liupf@zenith prodigal_files]$ pwd
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/prodigal_files
[liupf@zenith prodigal_files]$


/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/prodigal_genes_NT



3)DRAM based on renamed/relabeled MAGs, got gene ids like this

===> Aug_OW2_C1_D1_30Gb_idba_metabat.39_Aug_OW2_C1_D1_30Gb_idba_metabat_scaffold_15_4  #a really long one

combined 3211 DRAMOUT genes and protein files are here




```


## hmmsearch output
#based on the prodigal prediction file
#
```
[liupf@zenith prodigal_files]$ cat *.faa >~/combined_owc_3211_prodigal.faa

[liupf@zenith prodigal_files]$ pwd
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/prodigal_files

#aa sequences
/home/projects/Wetlands/sulfur_cycling_analysis/sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa

#nt sequence 
/home/projects/Wetlands/sulfur_cycling_analysis/sulfur_cycling_owc_hmmsearh/

```

## DRAMOUT protein and gene, based on Adrienne relabeld genomes
```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_3211db_genes.faa


```
