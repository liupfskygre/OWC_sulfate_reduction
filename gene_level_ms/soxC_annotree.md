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
#Submitted batch job 7556
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

```
#prepare classification file for aprA and soxC==>r95

#on mac, annotree reference 

sed -i -e 's/___/;/g' soxC_user.out.top_positive_hits.txt

cut -f1 -d$';' soxC_user.out.top_positive_hits.txt > soxC_ba_ar_annotree_hits_MAGid.txt

#ba-ar 
grep -w -f soxC_ba_ar_annotree_hits_MAGid.txt ../ar_ba_taxonomy_r95.tsv >soxC_ba_ar_annotree_hits_MAG_tax.txt

sed -i -e 's/;/___/g' soxC_user.out.top_positive_hits.txt


#edit in excel to get tree id and MAGs id done
#e.g.
#Tree_ID	MAGs_ID	Seq_ID
#GB_GCA_000007225.1___AE009441.1_1796	GB_GCA_000007225.1	AE009441.1_1796

# MAGsID	Tax_full

sed -i '1 i\MAGsID\tTax_full'  soxC_ba_ar_annotree_hits_MAG_tax.txt

cat soxC_user.out.top_positive_hits.txt|awk -F '\t' '{print $0"\t"$1}'|sed -e 's/___/\t/2' - > soxC_geneID_MAGsID.txt  
sed -i '1 i\Tree_ID\tMAGs_ID\tSeq_ID' soxC_geneID_MAGsID.txt

##merge in R

R
setwd("./")
soxC_tax <- read.delim("soxC_ba_ar_annotree_hits_MAG_tax.txt",header=T, check.names = FALSE) 

soxC_ID <- read.delim("soxC_geneID_MAGsID.txt",header=T, check.names = FALSE) 

soxC_ID_tax<-merge(soxC_ID, soxC_tax, by.x="MAGs_ID", by.y="MAGsID", all=TRUE)

write.table(soxC_ID_tax, 'soxC_ID_tax_annotree.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")

#after merge from R
#prepare nodes label
#,-1,#000000,normal,1,0


cat soxC_ID_tax_annotree.txt|cut -f3,5 -d$'\t'|sed -e 's/\t/,/g' - >soxC_ID_tax_annotree_R95.txt
sed -i -e 's/$/,-1,#000000,normal,1,0/g' soxC_ID_tax_annotree_R95.txt

```

#soxC
```
cat ../itol_dataset_text_template_head.txt  soxC_ID_tax_annotree_R95.txt >itol_soxC_tax_annotree_R95_label.txt

```
