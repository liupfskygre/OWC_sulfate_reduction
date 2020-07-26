##

##

```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/phsA_annotree_gene_tree

#phsA
cat phsA_arc_annotree_hits.csv phsA_bac_annotree_hits.csv > phsA_ba_ar_anntree_hits.csv


#phsA
awk -F ',' '{print ">"$4"___"$3"\n"$8}' phsA_ba_ar_anntree_hits.csv > phsA_ba_ar_anntree_hits.fasta



sed -i -e '/gtdbId___geneId/d' phsA_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' phsA_ba_ar_anntree_hits.fasta


```

```
grep -c '>' phsA_ba_ar_anntree_hits.fasta

grep 'K08352' phsA_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > phsA_user.out.top_positive_hits.txt
pullseq -i phsA_ba_ar_anntree_hits.fasta -n phsA_user.out.top_positive_hits.txt > phsA_ba_ar_anntree_pos_hits.fasta

grep -c 'K08352' phsA_user.out.top.txt
grep -c '>' phsA_ba_ar_anntree_pos_hits.fasta
#1079
```


#phsA
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/phsA_annotree_gene_tree

grep 'phsA' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >phsA_clean_geneID.txt
pullseq -i thiosulfate_reductase_phsA.hmm.collection.faa -n phsA_clean_geneID.txt > phsA.hmm.collection_clean.fasta

grep -c '>' phsA.hmm.collection_clean.fasta
wc -l phsA_clean_geneID.txt
#

cat phsA_ba_ar_anntree_pos_hits.fasta phsA.hmm.collection_clean.fasta > phsA_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto phsA_owc_clean_wAnnRef.fasta > phsA_owc_clean_wAnnRef_align.fasta
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in phsA_owc_clean_wAnnRef_align.fasta -out phsA_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  phsA_owc_clean_wAnnRef_trimal.fasta 
```

#upload alignment and do iqtree
```
/home/projects/Wetlands/sulfur_cycling_analysis/annotree_ref_all_S_iqtree

nano phsA_iqtree.sh

sbatch phsA_iqtree.sh
#Submitted batch job 7558
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
iqtree -s phsA_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre phsA_annoRef_ 

```
