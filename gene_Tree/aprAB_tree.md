## aprAB

```
/home/projects/Wetlands/sulfur_cycling_analysis


#aprA
../pfam_KO_to_seqs_OWC3211.sh aprA TIGR02061 K00394 na_na 462

188

#aprB
../pfam_KO_to_seqs_OWC3211.sh aprB TIGR02060 K00395 PF12139 99

228

```

# downlaod sequence to MAC
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Data_analysis_sulfur_cycling/gene_tree/aprAB



```

# reference and tree ===> aprA 
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Data_analysis_sulfur_cycling/gene_tree/aprAB
cat aprA_arc_annotree_hits.csv aprA_bac_annotree_hits.csv > aprA_annotree_hits.csv

cat aprA_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,5 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > aprA_annotree_hits.fasta

#reference
cat aprA_annotree_hits.fasta aprA_4_tree.faa >aprA_4_tree_w_ref.fasta

/Users/pengfeiliu/software/mafft-mac/mafft.bat --auto aprA_4_tree_w_ref.fasta > aprA_4_tree_w_ref_align.fasta

aprA_4_tree_w_ref_align_refine.fasta

#trimAl v1.4.rev22 build[2015-05-21]
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -keepheader -automated1 -in aprA_4_tree_w_ref_align_refine.fasta -out aprA_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <aprA_4_tree_w_ref_align_refine_trim.fasta> aprA_4_raw.fasttree

run_treeshrink.py  -t aprA_4_raw.fasttree -m per-gene -o aprA_treeshrink_pergene -a aprA_4_tree_w_ref_align_refine_trim.fasta



```

# reference and tree ===> aprB 
```
cd /home/projects/Wetlands/sulfur_cycling_analysis

cat aprB_arc_annotree_hits.csv aprB_bac_annotree_hits.csv > aprB_annotree_hits.csv

cat aprB_annotree_hits.csv|grep -v 'bitscore' |cut -f3,4,5 -d',' > sel.csv

awk -F ',' '{print ">" $1 "link" $2 "_Ref" "\n" $3}' sel.csv > aprB_annotree_hits.fasta

#reference
cat aprB_annotree_hits.fasta aprB_4_tree.faa >aprB_4_tree_w_ref.fasta

mafft --auto aprB_4_tree_w_ref.fasta > aprB_4_tree_w_ref_align.fasta

muscle -in aprB_4_tree_w_ref_align.fasta -out aprB_4_tree_w_ref_align_refine.fasta

trimal -keepheader -automated1 -in aprB_4_tree_w_ref_align_refine.fasta -out aprB_4_tree_w_ref_align_refine_trim.fasta

fasttree -gamma -lg -boot 1000 <aprB_4_tree_w_ref_align_refine_trim.fasta> aprB_4_raw.fasttree

run_treeshrink.py  -t aprB_4_raw.fasttree -m per-gene -o aprB_treeshrink_pergene -a aprB_4_tree_w_ref_align_refine_trim.fasta

```

## gene list, final 
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Data_analysis_sulfur_cycling/gene_tree/aprAB
grep '>' aprA_4_tree.faa |sed 's/>//g' - > aprA_4_tree_gene.txt #188

grep '>' aprB_4_tree.faa |sed 's/>//g' - > aprB_4_tree_gene.txt #228

```
