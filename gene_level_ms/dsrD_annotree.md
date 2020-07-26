##


##
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrD_w_annotree_gene_tree

awk -F ',' '{print ">"$2"___"$1"\n"$3}' annotree_hits_dsrD_sel.csv > annotree_hits_dsrD_sel.fasta
#CDD search verified


```

dsrD.hmm.collection.faa

#dsrD
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrD_annotree_gene_tree

grep 'dsrD' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >dsrD_clean_geneID.txt
pullseq -i dsrD.hmm.collection.faa  -n dsrD_clean_geneID.txt > dsrD.hmm.collection_clean.fasta

grep -c '>' dsrD.hmm.collection_clean.fasta
wc -l dsrD_clean_geneID.txt
#132

cat annotree_hits_dsrD_sel.fasta dsrD.hmm.collection_clean.fasta > dsrD_owc_clean_wAnnRef.fasta

#/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto dsrD_owc_clean_wAnnRef.fasta > dsrD_owc_clean_wAnnRef_align.fasta
#done with muscle

/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in dsrD_owc_clean_wAnnRef_align.fasta -out dsrD_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  dsrD_owc_clean_wAnnRef_trimal.fasta 

```
#upload alignment and do iqtree
```
/home/projects/Wetlands/sulfur_cycling_analysis/annotree_ref_all_S_iqtree

nano dsrD_iqtree.sh

sbatch dsrD_iqtree.sh
#Submitted batch job 7561
#slurm

#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=10 #number of cores you are requesting
#SBATCH --time=14-00:00:00 #max 14 days, HH:MM:SS if you need to include days do D-HH:MM:SS i.e.requesting 7 days is done as 7-00:00:00
#SBATCH --mem=50gb #How much memory you are requesting.  i.e. 500gb.  For 1Tb use 1024gb.  Notice this is lower case.
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=liupf@colostate.edu
#SBATCH --partition=wrighton-hi,wrighton-low #The options are: wrighton-hi,wrighton-low,wilkins-hi,wilkins-low,debug


#put your code block here for running
iqtree -s dsrD_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre dsrD_annoRef_ 
