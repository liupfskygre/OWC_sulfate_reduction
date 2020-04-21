##Phylogenomic tree of Medium and high quality MAGs with sulfur cycling genes

#wkdir 
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
cd /home/projects/Wetlands/sulfur_cycling_analysis/phylogenomics_tree


[liupf@zenith phylogenomics_tree]$ cat gtdb_and_checkm_for_dram_archaea.txt|cut -f1 -d$'\t' > Archaea.list
[liupf@zenith phylogenomics_tree]$ cat gtdb_and_checkm_for_dram_bacteria.txt|cut -f1 -d$'\t' >bacteria.list

```

## bacteria tree
```
cd /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins


sed -i -e 's/2\.5kb/2-5kb/g' bacteria.list
sed -i -e 's/2\.5k/2-5kb/g' bacteria.list

for file in $(cat bacteria.list)
do
ln -sf /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"${file}".fa ./bacteria_MAGs_link/"${file}".fa
done
```

## archaeal tree
```
sed -i -e 's/2\.5kb/2-5kb/g' Archaea.list
sed -i -e 's/2\.5k/2-5kb/g' Archaea.list

for file in $(cat Archaea.list)
do
ln -sf /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/"${file}".fa ./archaeal_MAGs_link/"${file}".fa
done

#/home/projects/Wetlands/sulfur_cycling_analysis/phylogenomics_tree/archaeal_MAGs_link

```



#slurm scripts

#archaea

```
#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=10 #number of cores you are requesting
#SBATCH --time=14-00:00:00 #max 14 days, HH:MM:SS if you need to include days do D-HH:MM:SS i.e.requesting 7 days is done as 7-00:00:00
#SBATCH --mem=100gb #How much memory you are requesting.  i.e. 500gb.  For 1Tb use 1024gb.  Notice this is lower case.
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=liupf@colostate.edu
#SBATCH --partition=wrighton-hi,wrighton-low #The options are: wrighton-hi,wrighton-low,wilkins-hi,wilkins-low,debug


#code block for running
#
gtdbtk de_novo_wf --genome_dir archaeal_MAGs_link/ --outgroup_taxon p__Nanobacteria --ar122_ms --out_dir de_novo_wf_arc --cpus 10

#gtdbtk de_novo_wf --genome_dir ./genomes --bac120_ms --outgroup_taxon p__Chloroflexota --taxa_filter p__Firmicutes --out_dir de_novo_output


#
##call sbatch de_novo_wf_arc_slurm.sh

#view

#note on partition
##This specifies which user group/servers you are targeting.  There are these 4 partitions plus one called ‘debug’.
## Wilkins partitions have priority on marmot, Wrighton partitions have priority on tardigrade and zenith.



```

#bacteria
```
#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=10 #number of cores you are requesting
#SBATCH --time=14-00:00:00 #max 14 days, HH:MM:SS if you need to include days do D-HH:MM:SS i.e.requesting 7 days is done as 7-00:00:00
#SBATCH --mem=100gb #How much memory you are requesting.  i.e. 500gb.  For 1Tb use 1024gb.  Notice this is lower case.
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=liupf@colostate.edu
#SBATCH --partition=wrighton-hi,wrighton-low #The options are: wrighton-hi,wrighton-low,wilkins-hi,wilkins-low,debug


#code block for running
#
#gtdbtk de_novo_wf --genome_dir archaeal_MAGs_link/ -x fa --outgroup_taxon p__Nanobacteria --ar122_ms --out_dir de_novo_wf_arc --cpus 10

gtdbtk de_novo_wf --genome_dir bacteria_MAGs_link/ -x fa  --outgroup_taxon p__Binatota  --bac120_ms --out_dir de_novo_wf_bac --cpus 10


#
##call sbatch de_novo_wf_bac_slurm.sh

#view, squeue -a  | column -t

#note on partition
##This specifies which user group/servers you are targeting.  There are these 4 partitions plus one called ‘debug’.
## Wilkins partitions have priority on marmot, Wrighton partitions have priority on tardigrade and zenith.



```
