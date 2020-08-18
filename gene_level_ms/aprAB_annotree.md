##



##
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/aprAB_w_annotree_tree

#aprA
cat aprA_arc_annotree_hits.csv aprA_bac_annotree_hits.csv > aprA_ba_ar_anntree_hits.csv

#aprB 
cat aprB_arc_annotree_hits.csv aprB_bac_annotree_hits.csv > aprB_ba_ar_anntree_hits.csv

#aprA
awk -F ',' '{print ">"$4"___"$3"\n"$5}' aprA_ba_ar_anntree_hits.csv > aprA_ba_ar_anntree_hits.fasta

awk -F ',' '{print ">"$4"___"$3"\n"$5}' aprB_ba_ar_anntree_hits.csv > aprB_ba_ar_anntree_hits.fasta

sed -i -e '/gtdbId___geneId/d' aprA_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' aprA_ba_ar_anntree_hits.fasta


sed -i -e '/gtdbId___geneId/d' aprB_ba_ar_anntree_hits.fasta
sed -i -e '/sequence/d' aprB_ba_ar_anntree_hits.fasta
```

#submit to GhostKAAS
```
#https://www.kegg.jp/ghostkoala/
#one per time
#download
==> Annotation data: user_ko.txt
==> Taxonomy data: user.out.top.txt ==>this one is the tax


https://www.ncbi.nlm.nih.gov/Structure/bwrpsb/bwrpsb.cgi

```

#filter positive hits from ghostkoala analysis
```
#aprA
grep -c '>' aprA_ba_ar_anntree_hits.fasta #1104
grep 'K00394' aprA_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > aprA_user.out.top_positive_hits.txt
pullseq -i aprA_ba_ar_anntree_hits.fasta -n aprA_user.out.top_positive_hits.txt > aprA_ba_ar_anntree_pos_hits.fasta

#aprB
grep -c '>' aprB_ba_ar_anntree_hits.fasta
grep 'K00395' aprB_user.out.top.txt|cut -f1 -d$'\t' |cut -f2 -d':' > aprB_user.out.top_positive_hits.txt
pullseq -i aprB_ba_ar_anntree_hits.fasta -n aprB_user.out.top_positive_hits.txt > aprB_ba_ar_anntree_pos_hits.fasta
```

#filtering owc reads based on ghostkola annotation； cat with reference sequence
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/aprAB_w_annotree_tree

#aprA
grep 'aprA' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >aprA_clean_geneID.txt
pullseq -i aprA.hmm.collection.faa -n aprA_clean_geneID.txt > aprA.hmm.collection_clean.fasta

grep -c '>' aprA.hmm.collection_clean.fasta
wc -l aprA_clean_geneID.txt
#233

cat aprA_ba_ar_anntree_pos_hits.fasta aprA.hmm.collection_clean.fasta > aprA_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto aprA_owc_clean_wAnnRef.fasta > aprA_owc_clean_wAnnRef_align.fasta
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in aprA_owc_clean_wAnnRef_align.fasta -out aprA_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  aprA_owc_clean_wAnnRef_trimal.fasta 
```

#
```
#aprB
grep 'aprB' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >aprB_clean_geneID.txt
pullseq -i aprB.hmm.collection.faa -n aprB_clean_geneID.txt > aprB.hmm.collection_clean.fasta

grep -c '>' aprB.hmm.collection_clean.fasta
wc -l aprB_clean_geneID.txt
#233

cat aprB_ba_ar_anntree_pos_hits.fasta aprB.hmm.collection_clean.fasta > aprB_owc_clean_wAnnRef.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto aprB_owc_clean_wAnnRef.fasta > aprB_owc_clean_wAnnRef_align.fasta
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in aprB_owc_clean_wAnnRef_align.fasta -out aprB_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  aprB_owc_clean_wAnnRef_trimal.fasta 

```


#upload alignment and do iqtree
```
/home/projects/Wetlands/sulfur_cycling_analysis/annotree_ref_all_S_iqtree

nano aprA_iqtree.sh

nano aprB_iqtree.sh

sbatch aprA_iqtree.sh 
#Submitted batch job 7549
Submitted batch job 7567 

#failed, tree to resume (by default) with the model
#iqtree -s aprA_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre aprA_annoRef_ -m LG+R10  #stop manually


sbatch aprB_iqtree.sh
#Submitted batch job 7550

```

#slurm
```
sbatch dsrA_iqtree.sh sbatch dsrB_iqtree.sh

#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=10 #number of cores you are requesting
#SBATCH --time=14-00:00:00 #max 14 days, HH:MM:SS if you need to include days do D-HH:MM:SS i.e.requesting 7 days is done as 7-00:00:00
#SBATCH --mem=50gb #How much memory you are requesting.  i.e. 500gb.  For 1Tb use 1024gb.  Notice this is lower case.
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=liupf@colostate.edu
#SBATCH --partition=wrighton-hi,wrighton-low #The options are: wrighton-hi,wrighton-low,wilkins-hi,wilkins-low,debug


#put your code block here for running
iqtree -s aprA_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre aprA_annoRef_

iqtree -s aprB_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre aprB_annoRef_ 

```

#prepare taxonomy file
```
#
#prepare classification file for aprA and aprB==>r95

#prepare classification file for aprA and aprB==>r95

#on mac, annotree reference 
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/aprAB_tree_with_Annotree_ref/aprAB_gtdb_r95based_annotation

cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/aprAB_w_annotree_tree

#list of anntree MAGs id for aprA and aprB
cat ../ar122_taxonomy_r95.tsv ../bac120_taxonomy_r95.tsv > ../ar_ba_taxonomy_r95.tsv



sed -i -e 's/___/;/g' aprA_user.out.top_positive_hits.txt
sed -i -e 's/___/;/g' aprB_user.out.top_positive_hits.txt

cut -f1 -d$';' aprA_user.out.top_positive_hits.txt > aprA_ba_ar_annotree_hits_MAGid.txt
cut -f1 -d$';' aprB_user.out.top_positive_hits.txt > aprB_ba_ar_annotree_hits_MAGid.txt

#ba-ar 
grep -w -f aprA_ba_ar_annotree_hits_MAGid.txt ../ar_ba_taxonomy_r95.tsv >aprA_ba_ar_annotree_hits_MAG_tax.txt
grep -w -f aprB_ba_ar_annotree_hits_MAGid.txt ../ar_ba_taxonomy_r95.tsv >aprB_ba_ar_annotree_hits_MAG_tax.txt

sed -i -e 's/;/___/g' aprA_user.out.top_positive_hits.txt
sed -i -e 's/;/___/g' aprB_user.out.top_positive_hits.txt


#edit in excel to get tree id and MAGs id done
#e.g.
#Tree_ID	MAGs_ID	Seq_ID
#GB_GCA_000007225.1___AE009441.1_1796	GB_GCA_000007225.1	AE009441.1_1796

# MAGsID	Tax_full

sed -i '1 i\MAGsID\tTax_full'  aprA_ba_ar_annotree_hits_MAG_tax.txt
sed -i '1 i\MAGsID\tTax_full'  aprB_ba_ar_annotree_hits_MAG_tax.txt

cat aprA_user.out.top_positive_hits.txt|awk -F '\t' '{print $0"\t"$1}'|sed -e 's/___/\t/2' - > aprA_geneID_MAGsID.txt  
sed -i '1 i\Tree_ID\tMAGs_ID\tSeq_ID' aprA_geneID_MAGsID.txt


cat aprB_user.out.top_positive_hits.txt|awk -F '\t' '{print $0"\t"$1}'|sed -e 's/___/\t/2' - > aprB_geneID_MAGsID.txt  
sed -i '1 i\Tree_ID\tMAGs_ID\tSeq_ID' aprB_geneID_MAGsID.txt

##merge in R

R
setwd("./")
aprA_tax <- read.delim("aprA_ba_ar_annotree_hits_MAG_tax.txt",header=T, check.names = FALSE)
aprB_tax <- read.delim("aprB_ba_ar_annotree_hits_MAG_tax.txt",header=T, check.names = FALSE) 

aprA_ID <- read.delim("aprA_geneID_MAGsID.txt",header=T, check.names = FALSE)
aprB_ID <- read.delim("aprB_geneID_MAGsID.txt",header=T, check.names = FALSE) 

aprA_ID_tax<-merge(aprA_ID, aprA_tax, by.x="MAGs_ID", by.y="MAGsID", all=TRUE)
aprB_ID_tax<-merge(aprB_ID, aprB_tax, by.x="MAGs_ID", by.y="MAGsID", all=TRUE)
write.table(aprA_ID_tax, 'aprA_ID_tax_annotree.txt', sep = '\t', col.names = NA, quote = FALSE)
write.table(aprB_ID_tax, 'aprB_ID_tax_annotree.txt', sep = '\t', col.names = NA, quote = FALSE)
quit()

#after merge from R
#prepare nodes label
#,-1,#000000,normal,1,0

cat aprA_ID_tax_annotree.txt|cut -f3,5 -d$'\t'|sed -e 's/\t/,/g' - >aprA_ID_tax_annotree_R95.txt
sed -i -e 's/$/,-1,#000000,normal,1,0/g' aprA_ID_tax_annotree_R95.txt

cat aprB_ID_tax_annotree.txt|cut -f3,5 -d$'\t'|sed -e 's/\t/,/g' - >aprB_ID_tax_annotree_R95.txt
sed -i -e 's/$/,-1,#000000,normal,1,0/g' aprB_ID_tax_annotree_R95.txt

#compared R89 and R95, many are different, and some representative Genomes were missing in R95 release, 
e.g.RS_GCF_000974685.1; 
```

#tree_label
```
cat ../itol_dataset_text_template_head.txt  aprB_ID_tax_annotree_R95.txt >itol_aprB_tax_annotree_R95_label.txt
cat ../itol_dataset_text_template_head.txt  aprA_ID_tax_annotree_R95.txt >itol_aprA_tax_annotree_R95_label.txt

```
