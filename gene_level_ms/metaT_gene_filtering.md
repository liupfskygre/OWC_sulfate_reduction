

```
#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=10 #number of cores you are requesting
#SBATCH --time=14-00:00:00 #max 14 days, HH:MM:SS if you need to include days do D-HH:MM:SS i.e.requesting 7 days is done as 7-00:00:00
#SBATCH --mem=100gb #How much memory you are requesting.  i.e. 500gb.  For 1Tb use 1024gb.  Notice this is lower case.
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=liupf@colostate.edu
#SBATCH --partition=wrighton-hi,wrighton-low #The options are: wrighton-hi,wrighton-low,wilkins-hi,wilkins-low,debug


#put your code block here for running

#1, bam quality filtering

#2, HTseq counts
cd /home/projects/Wetlands/sulfur_cycling_analysis/metaT_mapping/metaT_mapping2018_MAGs3211/RSEM_transcript_bam


for prefix in $(cat bam.list)
do
samtools view -b -h -q 20 -f 2 -F 0x100 -@ 10 "${prefix}"_MAGs3211_RSEM.transcript.bam > "${prefix}"_MAGs3211_RSEM.filter.bam
# -q 30，-f 0x2 -F 0x100 remove mapping quality lower than 20 (99% confident), multiple mapping and chimera mapping

samtools sort -@ 10 -n "${prefix}"_MAGs3211_RSEM.filter.bam  -o "${prefix}"_MAGs3211_RSEM.sorted.bam
#HTseq requires name sorted bam file for reads counts

htseq-count -s no -f bam -t CDS -i ID -m union -r name  "${prefix}"_MAGs3211_RSEM.sorted.bam /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/all_bins_combined_genes_3211db.gff > "${prefix}"_htseq.out

rm "${prefix}"_MAGs3211_RSEM.filter.bam
rm "${prefix}"_MAGs3211_RSEM.sorted.bam
done
#
##call sbatch slurm.sh

#view

#note on partition
##This specifies which user group/servers you are targeting.  There are these 4 partitions plus one called ‘debug’.
## Wilkins partitions have priority on marmot, Wrighton partitions have priority on tardigrade and zenith.


# squeue -a  | column -t

# watch 'squeue -a | column -t'

```
