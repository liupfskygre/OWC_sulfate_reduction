##for all contigs with functional genes, get 5k contigs 

#put all 5k contigs together
```
1, fix header of each file
cd /home/projects/Wetlands/sulfur_cycling_analysis/hmmsearch_hits_assembly

for file in $(cat metaG16_list.txt)
do 
pullseq -m 5000 -i ${file}.fa > ${file}_5k.fa
done

#add name header
for file in $(cat metaG16_list.txt)
do 
sed -i -e "s/>/>"${file}"_prodigal~~/g" ${file}_5k.fa
done

#AugM1C1D1B_prodigal~~k121_10149869

2, cat all 16 metaG== 5kb together
#
cat *_5k.fa > all_metaG16_5k.fa


```

```
all_Raw_genes_id.txt #fix name with total during hmmsearch
sed -i -e 's/AugM2C1D5C~~k121/AugM2C1D5C_prodigal~~k121/g'  all_Raw_genes_id.txt #, 6901

#AugM1C1D1B_prodigal~~k121_10149869_2 

#remove _1=2, contigs_id=

sed -e 's/\(k121.*_.*\)_[0-9].*$/\1/1' all_Raw_genes_id.txt >all_Raw_genes_id_fix.txt 
#6091 all_Raw_genes_id_fix.txt

#remove duplicate
cat all_Raw_genes_id_fix.txt |sort|uniq >all_Raw_genes_id_fix_uniq.txt
#4969

pullseq -i all_metaG16_5k.fa -n all_Raw_genes_id_fix_uniq.txt > all_Raw_genes_uniq_conitgs_5k.fasta
#grep -c '>' all_Raw_genes_uniq_conitgs_5k.fasta
#1725==> which means for all 4974 contigs with sulfur cycling genes, 1/3 were in contigs w lenght >5k, not bad
# how about each categroy, especiall dsrA and dsrB


#get contigs seqs
with AugM1C1D1B

all_Raw_genes_id.txt
```


##adding sequence of fccB
```
# 812 PF09242.hmm.collection_id.txt

cd /home/projects/Wetlands/sulfur_cycling_analysis/hmmsearch_hits_assembly

sed -e 's/\(k121.*_.*\)_[0-9].*$/\1/1' PF09242.hmm.collection_id.txt >PF09242.hmm.collection_id_fix.txt 

#remove duplicate contigs
cat PF09242.hmm.collection_id_fix.txt |sort|uniq >PF09242.hmm.collection_id_fix_uniq.txt 
#796

cat PF09242.hmm.collection_id_fix_uniq.txt all_Raw_genes_id_fix_uniq.txt|sort|uniq > all_Raw_genes_id_fix_uniq_final.txt
#5743

pullseq -i all_metaG16_5k.fa -n all_Raw_genes_id_fix_uniq_final.txt > all_Raw_genes_uniq_conitgs_5k.fasta
grep -c '>' all_Raw_genes_uniq_conitgs_5k.fasta
#2006==> which means for all 4974 contigs with sulfur cycling genes, 1/3 were in contigs w lenght >5k, not bad

```

#dram annotation on 
```
[liupf@zenith hmmsearch_hits_assembly]$ nano sulfur_genes_in_uniq_conitgs_5k.sh


#!/bin/bash
#SBATCH --nodes=1 #always =1 on zenith, usually will be =1 on summit
#SBATCH --ntasks=16 #number of cores you are requesting
#SBATCH --time=14-00:00:00 #max 14 days, HH:MM:SS if you need to include days do D-HH:MM:SS i.e.requesting 7 days is done as 7-00:00:00
#SBATCH --mem=480gb #How much memory you are requesting.  i.e. 500gb.  For 1Tb use 1024gb.  Notice this is lower case.
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=liupf@colostate.edu
#SBATCH --partition=wrighton-hi,wrighton-low #The options are: wrighton-hi,wrighton-low,wilkins-hi,wilkins-low,debug


#code block for running
source /opt/Miniconda2/miniconda2/bin/activate DRAM
# DRAM.py annotate -i  -o  --threads 20  --min_contig_size 2500

DRAM.py annotate -i all_Raw_genes_uniq_conitgs_5k.fasta -o sulfur_genes_in_uniq_conitgs_5k --threads 16  --use_uniref --min_contig_size 4999


##call sbatch test_slurm.sh

 slurm-7252.out
```
