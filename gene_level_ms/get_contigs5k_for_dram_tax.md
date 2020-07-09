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

sed -e 's/\(k121.*_.*\)_[0-9].*$/\1/1' PF09242.hmm.collection_id.txt >PF09242.hmm.collection_id_fix.txt 

#remove duplicate contigs
cat PF09242.hmm.collection_id_fix.txt |sort|uniq >PF09242.hmm.collection_id_fix_uniq.txt 

cat PF09242.hmm.collection_id_fix_uniq.txt all_Raw_genes_id_fix_uniq.txt|sort|uniq > all_Raw_genes_id_fix_uniq_final.txt

pullseq -i all_metaG16_5k.fa -n all_Raw_genes_id_fix_uniq_final.txt > all_Raw_genes_uniq_conitgs_5k.fasta
#grep -c '>' all_Raw_genes_uniq_conitgs_5k.fasta
#1725==> which means for all 4974 contigs with sulfur cycling genes, 1/3 were in contigs w lenght >5k, not bad


```
