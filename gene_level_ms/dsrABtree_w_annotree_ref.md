## dsrAB tree w annotree ref 


## will this increase the classification level of our data??

##download annotree reference and convert to fasta file
```
#/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrAB_tree_with_Annotree_ref

#dsrA
cat dsrA_arc_annotree_hits.csv dsrA_bac_annotree_hits.csv > dsrA_ba_ar_anntree_hits.csv
#891


#dsrB 
cat dsrB_arc_annotree_hits.csv dsrB_bac_annotree_hits.csv > dsrB_ba_ar_anntree_hits.csv
#860

#annotree_hits_mcrA_PF02249_PF02745.csv
awk -F ',' '{print ">"$4";"$3"\n"$5}' dsrA_ba_ar_anntree_hits.csv > dsrA_ba_ar_anntree_hits.fasta


awk -F ',' '{print ">"$4";"$3"\n"$5}' dsrB_ba_ar_anntree_hits.csv > dsrB_ba_ar_anntree_hits.fasta

#cat annotree_hits_mcrA_PF02249_PF02745_raw.fasta| seqkit rmdup -s -o annotree_hits_mcrA_PF02249_PF02745_clean.fasta
#awk -F ',' '{print ">"$1"_"$2"_"$3"_"$4"\n"$5}' tabseq.tsv > seqs.fa
```


#dsrB
```
cat dsrB_ba_ar_anntree_hits.fasta dsrB.hmm.collection.faa >dsrB.hmm.collection_w_annotreeRef.faa

#on mac
/Users/pengfeiliu/software/mafft-mac/mafftdir/bin/mafft --auto --add dsrB.hmm.collection_w_annotreeRef.faa --thread 4 Full_length_seq_dsrAB_ref_fixH.faa >dsrB_anno_tree_alignment.faa

/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in dsrB_anno_tree_alignment.faa -out dsrB_anno_tree_alignment_trmal.fasta

#remove dsrA part and remove columns w/ 5% gaps；
#2625 sequences with 287 position


```

#dsrA
```
cat dsrA_ba_ar_anntree_hits.fasta dsrA.hmm.collection.faa >dsrA.hmm.collection_w_annotreeRef.faa

#on mac
/Users/pengfeiliu/software/mafft-mac/mafftdir/bin/mafft --auto --add dsrA.hmm.collection_w_annotreeRef.faa --thread 4 Full_length_seq_dsrAB_ref_fixH.faa >dsrA_anno_tree_alignment.faa

/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in dsrA_anno_tree_alignment.faa -out dsrA_anno_tree_alignment_trmal.fasta

#remove dsrB part and remove columns w/ 5% gaps；
#2715 sequences with 233 position

#
```
#
```
#change ; in the header to __
dsrA_anno_tree_alignment_trmal.fasta

dsrB_anno_tree_alignment_trmal.fasta

```

#dsrAB tree
```{r}
#upload to 

cd /home/projects/Wetlands/sulfur_cycling_analysis/dsrABD_tree

FastTree -gamma -lg -boot 1000 <dsrA_anno_tree_alignment_trmal.fasta> dsrA_anno_tree_alignment_trmal.tree

FastTree -gamma -lg -boot 1000 <dsrB_anno_tree_alignment_trmal.fasta> dsrB_anno_tree_alignment_trmal.tree

```


#prepare classification file for dsrA and dsrB==>r89
```
#on mac, annotree reference 
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrAB_tree_with_Annotree_ref/tree_annotation

#list of anntree MAGs id for dsrA and dsrB
ar122_taxonomy_r89.tsv
bac120_taxonomy_r89.tsv


cut -f4 -d, dsrA_arc_annotree_hits.csv > dsrA_arc_annotree_hits_MAGid.txt
cut -f4 -d, dsrB_arc_annotree_hits.csv > dsrB_arc_annotree_hits_MAGid.txt

cut -f4 -d, dsrA_bac_annotree_hits.csv >dsrA_bac_annotree_hits_MAGid.txt
cut -f4 -d, dsrB_bac_annotree_hits.csv >dsrB_bac_annotree_hits_MAGid.txt

#archaea 
grep -w -f dsrA_arc_annotree_hits_MAGid.txt ar122_taxonomy_r89.tsv >dsrA_arc_annotree_hits_MAG_tax.txt
grep -w -f dsrB_arc_annotree_hits_MAGid.txt ar122_taxonomy_r89.tsv >dsrB_arc_annotree_hits_MAG_tax.txt

cat 

#bacteria
grep -w -f dsrA_bac_annotree_hits_MAGid.txt bac120_taxonomy_r89.tsv>dsrA_bac_annotree_hits_MAG_tax.txt
grep -w -f dsrB_bac_annotree_hits_MAGid.txt bac120_taxonomy_r89.tsv>dsrB_bac_annotree_hits_MAG_tax.txt

#check    
55 dsrA_arc_annotree_hits_MAGid.txt
     836 dsrA_bac_annotree_hits_MAGid.txt
      52 dsrB_arc_annotree_hits_MAGid.txt
     808 dsrB_bac_annotree_hits_MAGid.txt
    1751 total
    
 #some MAGs cotain more than 1 dsrA or dsrB
      41 dsrA_arc_annotree_hits_MAG_tax.txt
     761 dsrA_bac_annotree_hits_MAG_tax.txt
      38 dsrB_arc_annotree_hits_MAG_tax.txt
     757 dsrB_bac_annotree_hits_MAG_tax.txt
    1597 total    
    #

#dsrA arc and bac

cat dsrA_arc_annotree_hits_MAG_tax.txt dsrA_bac_annotree_hits_MAG_tax.txt >dsrA_arc_bac_annotree_hits_MAG_tax.txt
cat dsrB_arc_annotree_hits_MAG_tax.txt dsrB_bac_annotree_hits_MAG_tax.txt >dsrB_arc_bac_annotree_hits_MAG_tax.txt

#get sequences ID of dsrA and dsrB
grep  '>'  dsrA_ba_ar_anntree_hits.fasta >dsrA_ba_ar_anntree_hits_ID.txt 
grep  '>'  dsrB_ba_ar_anntree_hits.fasta >dsrB_ba_ar_anntree_hits_ID.txt 
#edit in excel to get tree id and MAGs id done

```

#prepare classification file for dsrA and dsrB==>r95
```
#on mac, annotree reference 
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/dsrAB_tree_with_Annotree_ref/dsrAB_gtdb_r95based_annotation

#list of anntree MAGs id for dsrA and dsrB
ar122_taxonomy_r95.tsv
bac120_taxonomy_r95.tsv


cut -f4 -d, dsrA_arc_annotree_hits.csv > dsrA_arc_annotree_hits_MAGid.txt
cut -f4 -d, dsrB_arc_annotree_hits.csv > dsrB_arc_annotree_hits_MAGid.txt

cut -f4 -d, dsrA_bac_annotree_hits.csv >dsrA_bac_annotree_hits_MAGid.txt
cut -f4 -d, dsrB_bac_annotree_hits.csv >dsrB_bac_annotree_hits_MAGid.txt

#archaea 
grep -w -f dsrA_arc_annotree_hits_MAGid.txt ar122_taxonomy_r95.tsv >dsrA_arc_annotree_hits_MAG_tax.txt
grep -w -f dsrB_arc_annotree_hits_MAGid.txt ar122_taxonomy_r95.tsv >dsrB_arc_annotree_hits_MAG_tax.txt

#cat 

#bacteria
grep -w -f dsrA_bac_annotree_hits_MAGid.txt bac120_taxonomy_r95.tsv>dsrA_bac_annotree_hits_MAG_tax.txt
grep -w -f dsrB_bac_annotree_hits_MAGid.txt bac120_taxonomy_r95.tsv>dsrB_bac_annotree_hits_MAG_tax.txt

#check    wc -l *_hits_MAGid.txt
$ wc -l *_hits_MAGid.txt
      55 dsrA_arc_annotree_hits_MAGid.txt
     836 dsrA_bac_annotree_hits_MAGid.txt
      52 dsrB_arc_annotree_hits_MAGid.txt
     808 dsrB_bac_annotree_hits_MAGid.txt
    1751 total

    
 #some MAGs cotain more than 1 dsrA or dsrB
     $ wc -l *_MAG_tax.txt
      39 dsrA_arc_annotree_hits_MAG_tax.txt
     756 dsrA_bac_annotree_hits_MAG_tax.txt
      36 dsrB_arc_annotree_hits_MAG_tax.txt
     751 dsrB_bac_annotree_hits_MAG_tax.txt
    1582 total   
    #

#dsrA arc and bac

cat dsrA_arc_annotree_hits_MAG_tax.txt dsrA_bac_annotree_hits_MAG_tax.txt >dsrA_arc_bac_annotree_hits_MAG_tax.txt
cat dsrB_arc_annotree_hits_MAG_tax.txt dsrB_bac_annotree_hits_MAG_tax.txt >dsrB_arc_bac_annotree_hits_MAG_tax.txt

#get sequences ID of dsrA and dsrB
grep  '>'  dsrA_ba_ar_anntree_hits.fasta >dsrA_ba_ar_anntree_hits_ID.txt 
grep  '>'  dsrB_ba_ar_anntree_hits.fasta >dsrB_ba_ar_anntree_hits_ID.txt 
#edit in excel to get tree id and MAGs id done

```

sbatch dsrA_iqtree.sh
sbatch dsrB_iqtree.sh

```
#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=14 #number of cores you are requesting
#SBATCH --time=14-00:00:00 #max 14 days, HH:MM:SS if you need to include days do D-HH:MM:SS i.e.requesting 7 days is done as 7-00:00:00
#SBATCH --mem=128gb #How much memory you are requesting.  i.e. 500gb.  For 1Tb use 1024gb.  Notice this is lower case.
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=liupf@colostate.edu
#SBATCH --partition=wrighton-hi,wrighton-low #The options are: wrighton-hi,wrighton-low,wilkins-hi,wilkins-low,debug


#put your code block here for running
iqtree -s dsrA_anno_tree_alignment_trmal.fasta -nt AUTO -bb 1000 -pre dsrA_annoRef_ #slurm-7494.out

iqtree -s dsrB_anno_tree_alignment_trmal.fasta -nt AUTO -bb 1000 -pre dsrB_annoRef_ #Submitted batch job 7495
```

