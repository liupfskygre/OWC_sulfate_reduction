##

#sqr_annotree
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/sqr_annotree_gene_tree

sqr_bac_annotree_hits.csv

#soxB, search by tigrfam, 
awk -F ',' '{print ">"$4"___"$3"\n"$8}' sqr_bac_annotree_hits.csv > sqr_ba_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' sqr_ba_anntree_hits.fasta

sed -i -e '/sequence/d' sqr_ba_anntree_hits.fasta


```

```
grep -c '>' sqr_ba_anntree_hits.fasta

grep 'K17218' sqr_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > sqr_user.out.top_positive_hits.txt
pullseq -i sqr_ba_anntree_hits.fasta -n sqr_user.out.top_positive_hits.txt > sqr_ba_anntree_pos_hits.fasta

grep -c 'K17218' sqr_user.out.top.txt
grep -c '>' sqr_ba_anntree_pos_hits.fasta

#6642
```


```
#sqr

cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/sqr_annotree_gene_tree

grep 'sqr' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >sqr_clean_geneID.txt
pullseq -i sulfide_quinone_oxidoreductase_sqr.hmm.collection.faa -n sqr_clean_geneID.txt > sqr.hmm.collection_clean.fasta

grep -c '>' sqr.hmm.collection_clean.fasta
wc -l sqr_clean_geneID.txt
#

cat sqr_ba_anntree_pos_hits.fasta sqr.hmm.collection_clean.fasta > sqr_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto sqr_owc_clean_wAnnRef.fasta > sqr_owc_clean_wAnnRef_align.fasta
#/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in sqr_owc_clean_wAnnRef_align.fasta -out sqr_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  sqr_owc_clean_wAnnRef_trimal.fasta 

#mafft with global options
#failed as too many sequences. 
#mafft --thread 8 --maxiterate 1000 --globalpair sqr_owc_clean_wAnnRef.fasta > sqr_owc_clean_wAnnRef_align.fasta

```

#upload alignment and do iqtree

/home/projects/Wetlands/sulfur_cycling_analysis/annotree_ref_all_S_iqtree
```
nano sqr_iqtree.sh

sbatch sqr_iqtree.sh
#Submitted batch job 7553
```

#slurm
```
#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=10 #number of cores you are requesting
#SBATCH --time=14-00:00:00 #max 14 days, HH:MM:SS if you need to include days do D-HH:MM:SS i.e.requesting 7 days is done as 7-00:00:00
#SBATCH --mem=50gb #How much memory you are requesting.  i.e. 500gb.  For 1Tb use 1024gb.  Notice this is lower case.
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=liupf@colostate.edu
#SBATCH --partition=wrighton-hi,wrighton-low #The options are: wrighton-hi,wrighton-low,wilkins-hi,wilkins-low,debug


#put your code block here for running
iqtree -s sqr_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre sqr_annoRef_ 

```
