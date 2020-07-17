## dsrAB_tree

#either dsrA, dsrB or dsrAB concatenated, based the number of sequences
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/dsrABD_tree

```


#data
```
dsrA.hmm.collection.faa #534 sequences
dsrB.hmm.collection.faa
dsrD.hmm.collection.faa

#reference
#reference
# 1292 Full_length_seq_dsrAB_header.txt, nearly full length seqs from Pelican 2016 and Muller 2015, EMI and ISME J

#fix special characters
#Schlˆppnerbrunnen ==>SchlOppnerbrunnen; 43 sequences


Full_length_seq_dsrAB_ref.fasta
sed -e 's/ \+.*$/_Reference/g' Full_length_seq_dsrAB_ref.fasta > Full_length_seq_dsrAB_ref_fixH.faa

```

#alignment with references fasta

######dsrA
```
mafft --auto --add dsrA.hmm.collection.faa --thread 6 Full_length_seq_dsrAB_ref_fixH.faa >dsrA_gene_tree_alignment.faa

#-automated1--- (Optimized for Maximum Likelihood phylogenetic tree reconstruction)
trimal -keepheader -automated1 -in dsrA_gene_tree_alignment.faa -out dsrA_gene_tree_alignment_trimal.faa

#Error: the symbol 'B' accesing the matrix is not defined in this object
#

iqtree -s dsrA_gene_tree_alignment_trimal.faa -nt AUTO -bb 1000 -pre dsrA_gene #slurm-7351.out
#


#fast tree
FastTree -gamma -lg -boot 1000 <dsrA_gene_tree_alignment_trimal.faa> dsrA_gene_tree_Fasttree.tree

```

#dsrB 

```
mafft --auto --add dsrB.hmm.collection.faa --thread 6 Full_length_seq_dsrAB_ref_fixH.faa >dsrB_gene_tree_alignment.faa

trimal -keepheader -automated1 -in dsrB_gene_tree_alignment.faa -out dsrB_gene_tree_alignment_trimal.faa


iqtree -s dsrB_gene_tree_alignment_trimal.faa -nt AUTO -bb 1000 -pre dsrB_gene #slurm-7352.out


FastTree -gamma -lg -boot 1000 <dsrB_gene_tree_alignment_trimal.faa> dsrB_gene_tree_Fasttree.tree

```


#dsrD reference
```


```

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
iqtree -s dsrA_gene_tree_alignment_trimal.faa -nt AUTO -bb 1000 -pre dsrA_gene #slurm-7351.out

iqtree -s dsrB_gene_tree_alignment_trimal.faa -nt AUTO -bb 1000 -pre dsrB_gene #slurm-7352.out

```
