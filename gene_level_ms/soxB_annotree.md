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
```


#prepare classification file for aprA and soxB==>r95
```
#on mac, annotree reference 
sed -i -e 's/___/;/g' soxB_user.out.top_positive_hits.txt

cut -f1 -d$';' soxB_user.out.top_positive_hits.txt > soxB_ba_ar_annotree_hits_MAGid.txt

#ba-ar 
grep -w -f soxB_ba_ar_annotree_hits_MAGid.txt ../ar_ba_taxonomy_r95.tsv >soxB_ba_ar_annotree_hits_MAG_tax.txt

sed -i -e 's/;/___/g' soxB_user.out.top_positive_hits.txt


#edit in excel to get tree id and MAGs id done
#e.g.
#Tree_ID	MAGs_ID	Seq_ID
#GB_GCA_000007225.1___AE009441.1_1796	GB_GCA_000007225.1	AE009441.1_1796

# MAGsID	Tax_full

sed -i '1 i\MAGsID\tTax_full'  soxB_ba_ar_annotree_hits_MAG_tax.txt

cat soxB_user.out.top_positive_hits.txt|awk -F '\t' '{print $0"\t"$1}'|sed -e 's/___/\t/2' - > soxB_geneID_MAGsID.txt  
sed -i '1 i\Tree_ID\tMAGs_ID\tSeq_ID' soxB_geneID_MAGsID.txt

##merge in R

R
setwd("./")
soxB_tax <- read.delim("soxB_ba_ar_annotree_hits_MAG_tax.txt",header=T, check.names = FALSE) 

soxB_ID <- read.delim("soxB_geneID_MAGsID.txt",header=T, check.names = FALSE) 

soxB_ID_tax<-merge(soxB_ID, soxB_tax, by.x="MAGs_ID", by.y="MAGsID", all=TRUE)

write.table(soxB_ID_tax, 'soxB_ID_tax_annotree.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")

#after merge from R
#prepare nodes label
#,-1,#000000,normal,1,0


cat soxB_ID_tax_annotree.txt|cut -f3,5 -d$'\t'|sed -e 's/\t/,/g' - >soxB_ID_tax_annotree_R95.txt
sed -i -e 's/$/,-1,#000000,normal,1,0/g' soxB_ID_tax_annotree_R95.txt
```


#tree_label
```
cat ../itol_dataset_text_template_head.txt  soxB_ID_tax_annotree_R95.txt >itol_soxB_tax_annotree_R95_label.txt
```

