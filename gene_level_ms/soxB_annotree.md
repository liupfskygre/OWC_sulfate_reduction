##

##
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/soxB_annotree_gene_tree
#soxB
cat soxB_arc_annotree_hits.csv soxB_bac_annotree_hits.csv > soxB_ba_ar_anntree_hits.csv


#soxB, search by tigrfam, 
awk -F ',' '{print ">"$4"___"$3"\n"$5}' soxB_ba_ar_anntree_hits.csv > soxB_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' soxB_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' soxB_ba_ar_anntree_hits.fasta

```


```
grep -c '>' soxB_ba_ar_anntree_hits.fasta

grep 'K17224' soxB_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > soxB_user.out.top_positive_hits.txt
pullseq -i soxB_ba_ar_anntree_hits.fasta -n soxB_user.out.top_positive_hits.txt > soxB_ba_ar_anntree_pos_hits.fasta

grep -c 'K17224' soxB_user.out.top.txt
grep -c '>' soxB_ba_ar_anntree_pos_hits.fasta


```

#soxB
```

cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/soxB_annotree_gene_tree

grep 'soxB' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >soxB_clean_geneID.txt
pullseq -i soxB.hmm.collection.faa -n soxB_clean_geneID.txt > soxB.hmm.collection_clean.fasta

grep -c '>' soxB.hmm.collection_clean.fasta
wc -l soxB_clean_geneID.txt
#

cat soxB_ba_ar_anntree_pos_hits.fasta soxB.hmm.collection_clean.fasta > soxB_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto soxB_owc_clean_wAnnRef.fasta > soxB_owc_clean_wAnnRef_align.fasta
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in soxB_owc_clean_wAnnRef_align.fasta -out soxB_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  soxB_owc_clean_wAnnRef_trimal.fasta 
```

#upload alignment and do iqtree
```
/home/projects/Wetlands/sulfur_cycling_analysis/annotree_ref_all_S_iqtree

nano soxB_iqtree.sh

sbatch soxB_iqtree.sh
#Submitted batch job 7554
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
iqtree -s soxB_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre soxB_annoRef_ 
