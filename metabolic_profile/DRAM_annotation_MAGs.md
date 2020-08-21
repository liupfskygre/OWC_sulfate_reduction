## Adrienne run dram on all dataset

#update Aug20-2020

#Adrienne rerun GTDBtk on r95, DRAM of 3217 MAGs (before 3211 MAGs)
```
# location of DRAM out 
#newly DRMA without uniref annotation
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/DRAM_directories
all_bins_combined_3217db_annotations_gtdbtk_rel95.txt

all_bins_combined_3217db_checkM_and_gtdbtk_rel95.txt

#location of GTDBtk 
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/


```

#location, following dram out based on gtdbtk r89 were removed
```
#DRAM.py
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins


# the DRAM is run on each bins, not all
# each bins get an annotation.tsv file

#get the annotation.tsv for all genomes 
for file in *_DRAMOUT
do 
cat ${file}/annotations.tsv >> all_3211_annotations.tsv
printf "\n" >>all_3211_annotations.tsv
done
```

#search based on KO number
```
cd /home/projects/Wetlands/sulfur_cycling_analysis

cp /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/all_bins_combined_annotations_3211db.tsv ./

nano sulfur_cycling_KO_gene.list
grep -w -f sulfur_cycling_KO_gene.list all_bins_combined_annotations_3211db.tsv >sulfur_cycling_KO_gene_annotations_3211db.tsv

#
grep -i 'TIGR04315' all_bins_combined_annotations_3211db.tsv |wc -l
# otr 
```

#search based on KO and avaliable PFAM, TIGR HMM acc
```
sulfur_cycling_KO_HMM_gene.list

grep -w -f sulfur_cycling_KO_HMM_gene.list all_bins_combined_annotations_3211db.tsv >sulfur_cycling_KO_HMM_gene_annotations_3211db.tsv
```

#DRAMOUT genes and proteins
```

#DRAM.py
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins

# the DRAM is run on each bins, not all
# each bins get an annotation.tsv file

#get the annotation.tsv for all genomes 
for file in *_DRAMOUT
do 
cat ${file}/genes.fna >> all_3211_genes_DRAM.fna
printf "\n" >>all_3211_genes_DRAM.fna
done
#grep -c '>' *.fna
#9392229


for file in *_DRAMOUT
do 
cat ${file}/genes.faa >> all_3211_genes_DRAM_aa.faa
printf "\n" >> all_3211_genes_DRAM_aa.faa
done
grep -c '^>' *.faa
#9392229
```
#rename header
```


```

# using HMMsearch and KOscan + tree to selecte MAGs for down stream analysis
```


```

