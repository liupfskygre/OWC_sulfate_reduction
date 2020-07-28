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
iqtree -s soeA_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre soeA_annoRef_ 

```

#prepare classification file for aprA and soeA==>r95

```
#on mac, annotree reference 
sed -i -e 's/___/;/g' soeA_user.out.top_positive_hits.txt

cut -f1 -d$';' soeA_user.out.top_positive_hits.txt > soeA_ba_ar_annotree_hits_MAGid.txt

#ba-ar 
grep -w -f soeA_ba_ar_annotree_hits_MAGid.txt ../ar_ba_taxonomy_r95.tsv >soeA_ba_ar_annotree_hits_MAG_tax.txt

sed -i -e 's/;/___/g' soeA_user.out.top_positive_hits.txt


#edit in excel to get tree id and MAGs id done
#e.g.
#Tree_ID	MAGs_ID	Seq_ID
#GB_GCA_000007225.1___AE009441.1_1796	GB_GCA_000007225.1	AE009441.1_1796

# MAGsID	Tax_full

sed -i '1 i\MAGsID\tTax_full'  soeA_ba_ar_annotree_hits_MAG_tax.txt

cat soeA_user.out.top_positive_hits.txt|awk -F '\t' '{print $0"\t"$1}'|sed -e 's/___/\t/2' - > soeA_geneID_MAGsID.txt  
sed -i '1 i\Tree_ID\tMAGs_ID\tSeq_ID' soeA_geneID_MAGsID.txt

##merge in R

R
setwd("./")
soeA_tax <- read.delim("soeA_ba_ar_annotree_hits_MAG_tax.txt",header=T, check.names = FALSE) 

soeA_ID <- read.delim("soeA_geneID_MAGsID.txt",header=T, check.names = FALSE) 

soeA_ID_tax<-merge(soeA_ID, soeA_tax, by.x="MAGs_ID", by.y="MAGsID", all=TRUE)

write.table(soeA_ID_tax, 'soeA_ID_tax_annotree.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")

#after merge from R
#prepare nodes label
#,-1,#000000,normal,1,0


cat soeA_ID_tax_annotree.txt|cut -f3,5 -d$'\t'|sed -e 's/\t/,/g' - >soeA_ID_tax_annotree_R95.txt
sed -i -e 's/$/,-1,#000000,normal,1,0/g' soeA_ID_tax_annotree_R95.txt

```
