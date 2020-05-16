##

## kofam scan is wired, not for sorA, but get results for the results are sqr/fcc based on the 
## problem of the kofamscan, the output is sulfide dehydrogenase (flavocytochrome) ==sqr/fcc


#dram annotation showing only 1 hits. 

#not used

##sorA 
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/
mkdir sorA_tree
cd sorA_tree

#seqs list
grep -i 'sorA' ../MAGs_Pro_Con_wide_w_profile_name.txt|cut -f8 -d$'\t' >sorA_gene_list.txt
cat sorA_gene_list.txt|sort|uniq|wc -l
#636 


#file name 
pullseq -i ../sulfur_cycling_owc_hmmsearh/combined_owc_3211_prodigal.faa -n sorA_gene_list.txt>sorA_gene_aa.faa
#636  grep -c '>' sorA_gene_aa.faa 
sed -e 's/ #.*$//g' sorA_gene_aa.faa >sorA_gene_aa_fixheader.faa


#
pullseq -i ../sulfur_cycling_owc_hmmsearh/all_wetlands_bins_combined.genes.fasta -n sorA_gene_list.txt>sorA_gene_nt.fna
#635; grep -c '>' sorA_gene_nt.fna
sed -e 's/ #.*$//g' sorA_gene_nt.fna > sorA_gene_nt_fixheader.fna

## problem of the kofamscan, the output is sulfide dehydrogenase (flavocytochrome) ==sqr/fcc
```
