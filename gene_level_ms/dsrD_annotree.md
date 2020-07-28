##


##
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrD_w_annotree_gene_tree

awk -F ',' '{print ">"$2"___"$1"\n"$3}' annotree_hits_dsrD_sel.csv > annotree_hits_dsrD_sel.fasta
#CDD search verified


```

dsrD.hmm.collection.faa

#dsrD
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrD_annotree_gene_tree

grep 'dsrD' ../clean_sulfur_genenID_type.txt |cut -f1 -d$'\t' >dsrD_clean_geneID.txt
pullseq -i dsrD.hmm.collection.faa  -n dsrD_clean_geneID.txt > dsrD.hmm.collection_clean.fasta

grep -c '>' dsrD.hmm.collection_clean.fasta
wc -l dsrD_clean_geneID.txt
#132

cat annotree_hits_dsrD_sel.fasta dsrD.hmm.collection_clean.fasta > dsrD_owc_clean_wAnnRef.fasta

#/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto dsrD_owc_clean_wAnnRef.fasta > dsrD_owc_clean_wAnnRef_align.fasta
#done with muscle

/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in dsrD_owc_clean_wAnnRef_align.fasta -out dsrD_owc_clean_wAnnRef_trimal.fasta

#change ~~ to ___in the sequence header
sed -i -e 's/~~/___/g'  dsrD_owc_clean_wAnnRef_trimal.fasta 

```
#upload alignment and do iqtree
```
/home/projects/Wetlands/sulfur_cycling_analysis/annotree_ref_all_S_iqtree

nano dsrD_iqtree.sh

sbatch dsrD_iqtree.sh
#Submitted batch job 7561
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
iqtree -s dsrD_owc_clean_wAnnRef_trimal.fasta  -nt AUTO -bb 1000 -pre dsrD_annoRef_ 
```

#prepare annotation file
```
cat annotree_hits_dsrD_sel.fasta|grep '>' |sed -e 's/>//g' - > dsrD_user.out.top_positive_hits.txt

#prepare classification file for aprA and dsrD==>r95

#on mac, annotree reference 

sed -i -e 's/___/;/g' dsrD_user.out.top_positive_hits.txt

cut -f1 -d$';' dsrD_user.out.top_positive_hits.txt > dsrD_ba_ar_annotree_hits_MAGid.txt

#ba-ar 
grep -w -f dsrD_ba_ar_annotree_hits_MAGid.txt ../ar_ba_taxonomy_r95.tsv >dsrD_ba_ar_annotree_hits_MAG_tax.txt

sed -i -e 's/;/___/g' dsrD_user.out.top_positive_hits.txt


#edit in excel to get tree id and MAGs id done
#e.g.
#Tree_ID	MAGs_ID	Seq_ID
#GB_GCA_000007225.1___AE009441.1_1796	GB_GCA_000007225.1	AE009441.1_1796

# MAGsID	Tax_full

sed -i '1 i\MAGsID\tTax_full'  dsrD_ba_ar_annotree_hits_MAG_tax.txt

cat dsrD_user.out.top_positive_hits.txt|awk -F '\t' '{print $0"\t"$1}'|sed -e 's/___/\t/2' - > dsrD_geneID_MAGsID.txt  
sed -i '1 i\Tree_ID\tMAGs_ID\tSeq_ID' dsrD_geneID_MAGsID.txt

##merge in R

R
setwd("./")
dsrD_tax <- read.delim("dsrD_ba_ar_annotree_hits_MAG_tax.txt",header=T, check.names = FALSE) 

dsrD_ID <- read.delim("dsrD_geneID_MAGsID.txt",header=T, check.names = FALSE) 

dsrD_ID_tax<-merge(dsrD_ID, dsrD_tax, by.x="MAGs_ID", by.y="MAGsID", all=TRUE)

write.table(dsrD_ID_tax, 'dsrD_ID_tax_annotree.txt', sep = '\t', col.names = NA, quote = FALSE)
quit()

#after merge from R
#prepare nodes label
#,-1,#000000,normal,1,0


cat dsrD_ID_tax_annotree.txt|cut -f3,5 -d$'\t'|sed -e 's/\t/,/g' - >dsrD_ID_tax_annotree_R95.txt
sed -i -e 's/$/,-1,#000000,normal,1,0/g' dsrD_ID_tax_annotree_R95.txt


```


