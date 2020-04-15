## Adrienne run dram on all dataset

#location
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

cd /home/projects/Wetlands/sulfur_cycling_analysis

cp /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/all_bins_combined_annotations_3211db.tsv ./

nano sulfur_cycling_KO_gene.list
grep -w -f sulfur_cycling_KO_gene.list all_bins_combined_annotations_3211db.tsv >sulfur_cycling_KO_gene_annotations_3211db.tsv

#
grep -i 'TIGR04315' all_bins_combined_annotations_3211db.tsv |wc -l
# otr 
```
