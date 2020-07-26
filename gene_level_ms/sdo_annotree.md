##

#sdo
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/sdo_annotree_gene_tree

#sdo
cat sdo_arc_annotree_hits.csv sdo_bac_annotree_hits.csv > sdo_ba_ar_anntree_hits.csv


#sdo, search by KO, 
awk -F ',' '{print ">"$4"___"$3"\n"$8}' sdo_ba_ar_anntree_hits.csv > sdo_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' sdo_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' sdo_ba_ar_anntree_hits.fasta



```

#sdo30
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/sdo_annotree_gene_tree

#sdo30
cat sdo30_arc_annotree_hits.csv sdo30_bac_annotree_hits.csv > sdo30_ba_ar_anntree_hits.csv


#sdo30, search by KO, 
awk -F ',' '{print ">"$4"___"$3"\n"$8}' sdo30_ba_ar_anntree_hits.csv > sdo30_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' sdo30_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' sdo30_ba_ar_anntree_hits.fasta

```

```
grep -c '>' sdo30_ba_ar_anntree_hits.fasta

grep 'K17725' sdo30_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > sdo30_user.out.top_positive_hits.txt
pullseq -i sdo30_ba_ar_anntree_hits.fasta -n sdo30_user.out.top_positive_hits.txt > sdo30_ba_ar_anntree_pos_hits.fasta

grep -c 'K17725' sdo30_user.out.top.txt
grep -c '>' sdo30_ba_ar_anntree_pos_hits.fasta
#618
```

#sdo
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/sdo_annotree_gene_tree

grep 'sdo' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >sdo_clean_geneID.txt
pullseq -i sulfur_dioxygenase_sdo.hmm.collection.faa -n sdo_clean_geneID.txt > sdo.hmm.collection_clean.fasta

grep -c '>' sdo.hmm.collection_clean.fasta
wc -l sdo_clean_geneID.txt
#

cat sdo30_ba_ar_anntree_pos_hits.fasta sdo.hmm.collection_clean.fasta > sdo_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto sdo_owc_clean_wAnnRef.fasta > sdo_owc_clean_wAnnRef_align.fasta
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in sdo_owc_clean_wAnnRef_align.fasta -out sdo_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  sdo_owc_clean_wAnnRef_trimal.fasta

```
#upload alignment and do iqtree

```
/home/projects/Wetlands/sulfur_cycling_analysis/annotree_ref_all_S_iqtree

nano sdo_iqtree.sh

sbatch sdo_iqtree.sh
#Submitted batch job 7559

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
iqtree -s sdo_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre sdo_annoRef_ 

