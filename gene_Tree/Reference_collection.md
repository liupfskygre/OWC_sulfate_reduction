## place to collect confident Reference for tree

```



```



## software to find outliers in alignment or a tree. 
```
conda activate treeshrink


#1
run_treeshrink.py  -t dsrD_iqtree.treefile -m per-gene -o dsrD_treeshrink_pergene -a dsrD_aa_cat_trim.faa

#2


#dsrA trees
fasttree -gamma -lg -boot 1000 <dsrA_4_tree_alignment.faa> dsrA_4_raw.fasttree

run_treeshrink.py  -t dsrA_4_raw.fasttree -m per-gene -o dsrA_treeshrink_pergene -a dsrA_4_tree_alignment.faa

#dsrA trees
/Users/pengfeiliu/software/trimal-trimAl/source/trimal -automated1 -keepheader -in dsrB_4_tree_alignment.faa -out dsrB_4_tree_alignment1.faa

fasttree -gamma -lg -boot 1000 <dsrB_4_tree_alignment1.faa> dsrB_4_raw.fasttree

run_treeshrink.py  -t dsrB_4_raw.fasttree -m per-gene -o dsrB_treeshrink_pergene -a dsrB_4_tree_alignment1.faa

```

#summary, 
based on the treeshrink testing on dsrD and dsrA, B, it is reliable and easier way to check which sequences are potentially outliers:

1, for dsrD, 2 seqs out of 394 were identified as outliers. CD-search of the two sequences showing them as dsrD.

2, dsrA and B give exactly the same results, 19 sequences as outliers, archaeal type, and Moorella, which are true 'outliers' in the tree. 

therefore, we can applied this to other genes to check the outliers, remove them after additional check (CD search, blast or eggnog searching)
