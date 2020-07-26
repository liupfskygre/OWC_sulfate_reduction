##

##
```
#cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/fccB_annotree_gene_tree


fccB_bac_annotree_hits.csv #pfam 

awk -F ',' '{print ">"$4"___"$3"\n"$6}' fccB_bac_annotree_hits.csv > fccB_ba_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' fccB_ba_anntree_hits.fasta

sed -i -e '/sequence/d' fccB_ba_anntree_hits.fasta
```

#
```
grep -c '>' fccB_ba_anntree_hits.fasta

grep 'K17229' fccB_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > fccB_user.out.top_positive_hits.txt
pullseq -i fccB_ba_anntree_hits.fasta -n fccB_user.out.top_positive_hits.txt > fccB_ba_anntree_pos_hits.fasta

grep -c 'K17229' fccB_user.out.top.txt
grep -c '>' fccB_ba_anntree_pos_hits.fasta
#1568
```

#filtering owc reads based on ghostkola annotation； cat with reference sequence
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/fccB_annotree_gene_tree

#fccB
grep 'fccB' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >fccB_clean_geneID.txt
pullseq -i PF09242.hmm.collection_len.faa -n fccB_clean_geneID.txt > fccB.hmm.collection_clean.fasta

grep -c '>' fccB.hmm.collection_clean.fasta
wc -l fccB_clean_geneID.txt
#

cat fccB_ba_anntree_pos_hits.fasta fccB.hmm.collection_clean.fasta > fccB_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto fccB_owc_clean_wAnnRef.fasta > fccB_owc_clean_wAnnRef_align.fasta
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in fccB_owc_clean_wAnnRef_align.fasta -out fccB_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  fccB_owc_clean_wAnnRef_trimal.fasta 
```

#upload alignment and do iqtree
```
/home/projects/Wetlands/sulfur_cycling_analysis/annotree_ref_all_S_iqtree

nano fccB_iqtree.sh

sbatch fccB_iqtree.sh
#Submitted batch job 7551

#slurm
```
sbatch dsrA_iqtree.sh sbatch dsrB_iqtree.sh
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
iqtree -s fccB_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre fccB_annoRef_ 

```
