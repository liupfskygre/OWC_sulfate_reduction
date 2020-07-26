#


#soxC
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/soxC_annotree_gene_tree

cat soxC_arc_annotree_hits.csv soxC_bac_annotree_hits.csv > soxC_ba_ar_anntree_hits.csv


#soxC, search by KO, 
awk -F ',' '{print ">"$4"___"$3"\n"$5}' soxC_ba_ar_anntree_hits.csv > soxC_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' soxC_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' soxC_ba_ar_anntree_hits.fasta
```

```
grep -c '>' soxC_ba_ar_anntree_hits.fasta

grep 'K17225' soxC_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > soxC_user.out.top_positive_hits.txt
pullseq -i soxC_ba_ar_anntree_hits.fasta -n soxC_user.out.top_positive_hits.txt > soxC_ba_ar_anntree_pos_hits.fasta

grep -c 'K17225' soxC_user.out.top.txt
grep -c '>' soxC_ba_ar_anntree_pos_hits.fasta
#1840
```

#soxC
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/soxC_annotree_gene_tree

grep 'soxC' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >soxC_clean_geneID.txt
pullseq -i soxC.hmm.collection.faa -n soxC_clean_geneID.txt > soxC.hmm.collection_clean.fasta

grep -c '>' soxC.hmm.collection_clean.fasta
wc -l soxC_clean_geneID.txt
#

cat soxC_ba_ar_anntree_pos_hits.fasta soxC.hmm.collection_clean.fasta > soxC_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto soxC_owc_clean_wAnnRef.fasta > soxC_owc_clean_wAnnRef_align.fasta
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in soxC_owc_clean_wAnnRef_align.fasta -out soxC_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  soxC_owc_clean_wAnnRef_trimal.fasta 
```

#upload alignment and do iqtree
```
/home/projects/Wetlands/sulfur_cycling_analysis/annotree_ref_all_S_iqtree

nano soxC_iqtree.sh

sbatch soxC_iqtree.sh
#Submitted batch job 7555
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
iqtree -s soxC_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre soxC_annoRef_ 
```
