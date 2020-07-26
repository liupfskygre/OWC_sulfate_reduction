##

```
#
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/soeA_annotree_gene_tree

#soeA
cat soeA_arc_annotree_hits.csv soeA_bac_annotree_hits.csv > soeA_ba_ar_anntree_hits.csv


#soeA, search by KO, 
awk -F ',' '{print ">"$4"___"$3"\n"$8}' soeA_ba_ar_anntree_hits.csv > soeA_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' soeA_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' soeA_ba_ar_anntree_hits.fasta


```
#filter positive hits from ghostkoala analysis
```
grep -c '>' soeA_ba_ar_anntree_hits.fasta

grep 'K21307' soeA_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > soeA_user.out.top_positive_hits.txt
pullseq -i soeA_ba_ar_anntree_hits.fasta -n soeA_user.out.top_positive_hits.txt > soeA_ba_ar_anntree_pos_hits.fasta

grep -c 'K21307' soeA_user.out.top.txt
grep -c '>' soeA_ba_ar_anntree_pos_hits.fasta 
#1101
```

#soeA
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/soeA_annotree_gene_tree

grep 'soeA' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >soeA_clean_geneID.txt
pullseq -i K21307.hmm.collection.faa -n soeA_clean_geneID.txt > soeA.hmm.collection_clean.fasta

grep -c '>' soeA.hmm.collection_clean.fasta
wc -l soeA_clean_geneID.txt
#233

cat soeA_ba_ar_anntree_pos_hits.fasta soeA.hmm.collection_clean.fasta > soeA_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto soeA_owc_clean_wAnnRef.fasta > soeA_owc_clean_wAnnRef_align.fasta
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in soeA_owc_clean_wAnnRef_align.fasta -out soeA_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  soeA_owc_clean_wAnnRef_trimal.fasta 
```
#


#upload alignment and do iqtree
```
/home/projects/Wetlands/sulfur_cycling_analysis/annotree_ref_all_S_iqtree

nano soeA_iqtree.sh

sbatch soeA_iqtree.sh
#Submitted batch job 7552
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
iqtree -s soeA_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre soeA_annoRef_ 

```
