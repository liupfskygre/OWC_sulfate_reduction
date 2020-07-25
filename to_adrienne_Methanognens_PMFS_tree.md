#1, gtdbtk denovo tree
#put all MAGs together, GTDBtk give a new fasttree,
#and position of new genomes from refs to genomes of interests in the tree


#from last time 
```
cd /home/projects/Wetlands/2018_sampling/Methanog_targeted_coassembly/Methanogens_final_dRep_clean_db/OWC_GTDBTk_iqtree_Archaea100_update

gtdbtk de_novo_wf --genome_dir ./ --ar122_ms --outgroup_taxon p__Nanoarchaeota --out_dir GTDBTktree_Archaea315 -x fa --cpus 20

```

#2. then fix some unfriendly char in the hearder e.g. '-'
```

sed -e 's/\(^>.*\) .*$/\1/g' gtdbtk.ar122.msa.fasta> gtdbtk.ar122.msa_fix.fasta
sed -i -e 's/\(>.*\) .*$/\1/g' gtdbtk.ar122.msa_fix.fasta

sed -i -e 's/DRTY-/DRTY_/g' gtdbtk.ar122.msa_fix.fasta
sed -i -e 's/JZ-/JZ_/g' gtdbtk.ar122.msa_fix.fasta


```

#3. #build guild tree with the fixed alignment
```
cd /home/projects/Wetlands/2018_sampling/Methanog_targeted_coassembly/Methanogens_final_dRep_clean_db/OWC_GTDBTk_iqtree_Archaea100_update/PMSF_BS100_arch315

iqtree -s gtdbtk.ar122.msa_fix.fasta -m LG+F+G -nt 20 -pre Arc315_iqtree_guilde


```

#4. mixture model tree
```
cd /home/projects/Wetlands/2018_sampling/Methanog_targeted_coassembly/Methanogens_final_dRep_clean_db/OWC_GTDBTk_iqtree_Archaea100_update/PMSF_BS100_arch315

iqtree -nt 24 -s gtdbtk.ar122.msa_fix.fasta -m LG+C10+G+F -ft Arc315_iqtree_guilde.treefile -b 100 -pre MG315_PMSF_BS100

#big notes here ====>
#backup checkpoint file , you can resume the run if it stops, normally backup 5-10 iterations
cp -r PMSF_BS100_arch315 PMSF_BS100_arch315_bs10
cp -r PMSF_BS100_arch315 PMSF_BS100_arch315_bs28
cp -r PMSF_BS100_arch315 PMSF_BS100_arch315_bs43
```

##done #done tree calculation, move to quantification of new lineages

Reading tree MG315_PMSF_BS100.treefile ...
Reading tree(s) file MG315_PMSF_BS100.boottrees ...
100 un-rooted tree(s) loaded
Rescaling split weights by 1.000
6468 splits found
Creating bootstrap support values...
Tree with assigned support written to MG315_PMSF_BS100.treefile

Analysis results written to:
  IQ-TREE report:                MG315_PMSF_BS100.iqtree
  Maximum-likelihood tree:       MG315_PMSF_BS100.treefile
  Likelihood distances:          MG315_PMSF_BS100.mldist
  Screen log file:               MG315_PMSF_BS100.log

Total CPU time for bootstrap: 52269895.436 seconds.
Total wall-clock time for bootstrap: 2516814.225 seconds.

Non-parametric bootstrap results written to:
Bootstrap trees:          MG315_PMSF_BS100.boottrees
  Consensus tree:           MG315_PMSF_BS100.contree
  ```
#sending the tree file, iTOl link with clades of interests and excel file to Chris


```
