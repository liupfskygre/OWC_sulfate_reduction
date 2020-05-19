##

# get gtdb and checkM classification of genomes
```
cd /home/projects/Wetlands/sulfur_cycling_analysis/phylogenomics_tree

sed -e 's/2\.5kb/2-5kb/g' gtdb_and_checkm_for_dram.txt > gtdb_and_checkm_for_dram_match_genome_name.txt
sed -i -e 's/2\.5k/2-5kb/g' gtdb_and_checkm_for_dram_match_genome_name.txt


grep -w -f ../marker_gene_confirmation_final/all_22_marker_gene_final_MAGs.tsv gtdb_and_checkm_for_dram_match_genome_name.txt > gtdb_and_checkm_1761MAGs.txt

#bacteria
#1712
grep 'Bacteria' gtdb_and_checkm_1761MAGs.txt > gtdb_and_checkm_1761MAGs_bac.txt

grep 'Bacteria' gtdb_and_checkm_1761MAGs.txt |cut -f1 -d$'\t' > gtdb_and_checkm_1712MAGs_bac.list


#archaea
#49
grep 'Archaea' gtdb_and_checkm_1761MAGs.txt > gtdb_and_checkm_1761MAGs_arc.txt
grep 'Archaea' gtdb_and_checkm_1761MAGs.txt |cut -f1 -d$'\t' > gtdb_and_checkm_49MAGs_arc.list

```

##based on the alignment from GTDB, build tree for bacteria using owc-sequence only first
```
pullseq -i /home/projects/Wetlands/sulfur_cycling_analysis/phylogenomics_tree/de_novo_wf_bac/gtdbtk.bac120.msa.fasta -n  gtdb_and_checkm_1712MAGs_bac.list > gtdb_and_checkm_1712MAGs_bac_msa.fasta

trimal -keepheader -automated1 -in gtdb_and_checkm_1712MAGs_bac_msa.fasta -out gtdb_and_checkm_1712MAGs_bac_msa_trim.fasta


FastTree -gamma -wag -boot 1000 <gtdb_and_checkm_1712MAGs_bac_msa_trim.fasta> gtdb_and_checkm_1712MAGs_bac.tree
```

