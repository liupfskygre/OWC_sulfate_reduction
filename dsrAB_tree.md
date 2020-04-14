# dsrAB tree from MAGs


#purpose, to validate the search of hmm
#to separate rdsr and odsr

#wkdir
```
/home/projects/Wetlands/sulfur_cycling_analysis/dsrABD_tree

```

#all gene list
```
/home/projects/Wetlands/sulfur_cycling_analysis/MAGs_Pro_Con_wide_w_profile_name.txt

```


## dsrB 

## pullseq 
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir dsrABD_tree
#seqs list
grep -i 'dsrB' MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >dsrB_gene_list715.txt
mv dsrB_gene_list715.txt dsrABD_tree
cat dsrB_gene_list715.txt|sort|uniq|wc -l
#363 with header



#all 3211 MAGs gene aa seqs: prodigal.faa
cd 
/home/projects/Wetlands/sulfur_cycling_analysis/sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa


#fna list? confusing about the file from Adrienne: all_wetlands_bins_combined.genes.fasta
cp /home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/prodigal_genes_NT/all_wetlands_bins_combined.genes.fasta ./

cd dsrABD_tree
#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n dsrB_gene_list715.txt>dsrB_gene_list715_nt.fna
# 362 


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n dsrB_gene_list715.txt>dsrB_gene_list715_aa.faa

```



