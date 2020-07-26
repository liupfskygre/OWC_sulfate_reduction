##sor

#reference 
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/sor_tree_annotree_gene_tree

#sor
cat sor_arc_annotree_hits.csv sor_bac_annotree_hits.csv > sor_ba_ar_anntree_hits.csv


#sor, search by KO, 
awk -F ',' '{print ">"$4"___"$3"\n"$6}' sor_ba_ar_anntree_hits.csv > sor_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' sor_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' sor_ba_ar_anntree_hits.fasta


```
#2
```
grep -c '>' sor_ba_ar_anntree_hits.fasta

grep 'K16952' sor_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > sor_user.out.top_positive_hits.txt
pullseq -i sor_ba_ar_anntree_hits.fasta -n sor_user.out.top_positive_hits.txt > sor_ba_ar_anntree_pos_hits.fasta

grep -c 'K16952' sor_user.out.top.txt
grep -c '>' sor_ba_ar_anntree_pos_hits.fasta
```


#sor
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/sor_tree_annotree_gene_tree

grep 'SOR' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >sor_clean_geneID.txt
pullseq -i PF07682.hmm.collection.faa -n sor_clean_geneID.txt > sor.hmm.collection_clean.fasta

grep -c '>' sor.hmm.collection_clean.fasta
wc -l sor_clean_geneID.txt


cat sor_ba_ar_anntree_pos_hits.fasta sor.hmm.collection_clean.fasta > sor_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto sor_owc_clean_wAnnRef.fasta > sor_owc_clean_wAnnRef_align.fasta
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in sor_owc_clean_wAnnRef_align.fasta -out sor_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  sor_owc_clean_wAnnRef_trimal.fasta 
#upload alignment and do iqtree

/home/projects/Wetlands/sulfur_cycling_analysis/annotree_ref_all_S_iqtree

nano sor_iqtree.sh

sbatch sor_iqtree.sh
#Submitted batch job 7560
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
iqtree -s sor_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre sor_annoRef_ 
