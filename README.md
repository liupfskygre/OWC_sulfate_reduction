# OWC_sulfate_reduction
this is a side story on sulfate reduction in OWC wetland based on 16S, biogeochemical, metaG, metaT


# checking and fix the name header to all DRAMOUT way#

## what did Adrienne do for MAGs renaming ##
```
1) add partial name of MAGs (w/o) MAGs id and .fa to the seq header, +\t scaffold ID
#here 

/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins
e.g.
===> OWC_subtractive_megahit_Surface_metabat_k121_44274654  k121_44274654

#a master file with MAGs ID ===> scaffold ID is created



here: 
/home/projects/Wetlands/sulfur_cycling_analysis

=====>wetlands_db_contigs_to_bins.tsv

#sed -e 's/.fa.relabeled/_/1'  wetlands_db_contigs_to_bins.tsv > wetlands_db_contigs_to_bins_fix.tsv


2)based on this we have prodigal predicition, get gene names like this 

===> OWC_subtractive_megahit_Surface_metabat_k121_44274654_13

all combined genes and protein files are here
/home/projects/Wetlands/sulfur_cycling_analysis/sulfur_cycling_owc_hmmsearh 

===>combined_owc_3211_prodigal.faa
===>all_wetlands_bins_combined.genes.fasta

[liupf@zenith prodigal_files]$ pwd
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/prodigal_files

[liupf@zenith prodigal_files]$
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/prodigal_genes_NT



3)DRAM based on renamed/relabeled MAGs, got gene ids like this

===> Aug_OW2_C1_D1_30Gb_idba_metabat.39_Aug_OW2_C1_D1_30Gb_idba_metabat_scaffold_15_4  #a really long one

combined 3211 DRAMOUT genes and protein files are here


#from dramout genes.fna
#9392229
=========> all_3211_genes_DRAM.fna

#from dramout genes.faa
#9392229, the same as nt file
=======> all_3211_genes_DRAM_aa.faa
```
