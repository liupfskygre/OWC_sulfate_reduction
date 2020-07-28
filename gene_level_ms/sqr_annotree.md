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

```
#prepare classification file for aprA and sqr==>r95

#on mac, annotree reference 

sed -i -e 's/___/;/g' sqr_user.out.top_positive_hits.txt

cut -f1 -d$';' sqr_user.out.top_positive_hits.txt > sqr_ba_ar_annotree_hits_MAGid.txt

#ba-ar 
grep -w -f sqr_ba_ar_annotree_hits_MAGid.txt ../ar_ba_taxonomy_r95.tsv >sqr_ba_ar_annotree_hits_MAG_tax.txt

sed -i -e 's/;/___/g' sqr_user.out.top_positive_hits.txt


#edit in excel to get tree id and MAGs id done
#e.g.
#Tree_ID	MAGs_ID	Seq_ID
#GB_GCA_000007225.1___AE009441.1_1796	GB_GCA_000007225.1	AE009441.1_1796

# MAGsID	Tax_full

sed -i '1 i\MAGsID\tTax_full'  sqr_ba_ar_annotree_hits_MAG_tax.txt

cat sqr_user.out.top_positive_hits.txt|awk -F '\t' '{print $0"\t"$1}'|sed -e 's/___/\t/2' - > sqr_geneID_MAGsID.txt  
sed -i '1 i\Tree_ID\tMAGs_ID\tSeq_ID' sqr_geneID_MAGsID.txt

##merge in R

R
setwd("./")
sqr_tax <- read.delim("sqr_ba_ar_annotree_hits_MAG_tax.txt",header=T, check.names = FALSE) 

sqr_ID <- read.delim("sqr_geneID_MAGsID.txt",header=T, check.names = FALSE) 

sqr_ID_tax<-merge(sqr_ID, sqr_tax, by.x="MAGs_ID", by.y="MAGsID", all=TRUE)

write.table(sqr_ID_tax, 'sqr_ID_tax_annotree.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")

#after merge from R
#prepare nodes label
#,-1,#000000,normal,1,0


cat sqr_ID_tax_annotree.txt|cut -f3,5 -d$'\t'|sed -e 's/\t/,/g' - >sqr_ID_tax_annotree_R95.txt
sed -i -e 's/$/,-1,#000000,normal,1,0/g' sqr_ID_tax_annotree_R95.txt

```
#
```
#sqr

cat ../itol_dataset_text_template_head.txt  sqr_ID_tax_annotree_R95.txt >itol_sqr_tax_annotree_R95_label.txt

```
