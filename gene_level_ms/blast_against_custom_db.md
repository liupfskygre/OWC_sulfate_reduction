##using customed annotree db to  classify my sequence

#1, top hits based way
##
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/blast_custome_annotree_db




```


```
makeblastdb -in all_sequences.fasta -dbtype prot -out HCOx_classifier_allSeq -parse_seqids

blastp -query query.fasta -db HCOx_classifier_allSeq -outfmt 6 -max_target_seqs 1 -num_threads 6 -out out6MTS3.txt
```



#prepare blast DB
```
cat *pos* >sulfur_marker_gene_annotree_db.fas #22449
cat *hmm* > sulfur_marker_gene_OWC.fas # 4902

```

#

#use diamond instead of blast due to the "-max_target_seqs" issue 
#--max-target-seqs
```
#https://github.com/bbuchfink/diamond/issues/232

conda activate diamondv201

diamond makedb --in sulfur_marker_gene_annotree_db.fas  -d sulfur_marker_gene_annotree_db
diamond blastp -p 4 -d sulfur_marker_gene_annotree_db -o sulfur_owc_diamond_out.txt -f 6 -q sulfur_marker_gene_OWC.fas --max-target-seqs 1 --ultra-sensitive

--ultra-sensitive

diamond v2.0.1.139
```

#checking and get tax for each hits based no the annotree search
```
#combine all info together

sulfur_owc_diamond_out.txt
TPM_Gene_W_5ktax.txt 
../ar_ba_taxonomy_r95.tsv

#header: Gene_ID	Subject_ID	Percentage_of_identical_matches	Alignment_length	Number_of_mismatches	Number_of_gap_openings	Start_of_alignment_in_query	End_of_alignment_in_query	Start_of_alignment_in_subject	End_of_alignment_in_subject	Expected_value	Bit_score

#
grep -w -f Diamond_OWC_hits_annotree_MAGs_uniq.txt ../ar_ba_taxonomy_r95.tsv > Diamond_OWC_hits_annotree_MAGs_Taxonomy.txt

sed -i '1 i\MAGsID\tTax_full'  Diamond_OWC_hits_annotree_MAGs_Taxonomy.txt

R
setwd("./")
MAGs_tax <- read.delim("Diamond_OWC_hits_annotree_MAGs_Taxonomy.txt",header=T, check.names = FALSE) 

blastP_out <- read.delim("sulfur_owc_diamond_out.txt",header=T, check.names = FALSE) 

OWC_genes <-read.delim("TPM_Gene_W_5ktax.txt",header=T, check.names = FALSE) 

#merge 1

MAGs_tax_blastp <-merge(MAGs_tax, blastP_out, by.x="MAGsID", by.y="MAGs", all=TRUE)


MAGs_tax_blastp_OWC <-merge(MAGs_tax_blastp, OWC_genes, by.x="Gene_ID", by.y="Gene_ID", all=TRUE)

write.table(MAGs_tax_blastp_OWC, 'Sulfur_OWC_genes_w_anntree_MAGs_tax.txt', sep = '\t', col.names = NA, quote = FALSE)
quit("no")


```



```

Value 6 may be followed by a space-separated list of these keywords:

	qseqid means Query Seq - id
	qlen means Query sequence length
	sseqid means Subject Seq - id
	sallseqid means All subject Seq - id(s), separated by a ';'
	slen means Subject sequence length
	qstart means Start of alignment in query
	qend means End of alignment in query
	sstart means Start of alignment in subject
	send means End of alignment in subject
	qseq means Aligned part of query sequence
	full_qseq means Query sequence
	sseq means Aligned part of subject sequence
	full_sseq means Subject sequence
	evalue means Expect value
	bitscore means Bit score
	score means Raw score
	length means Alignment length
	pident means Percentage of identical matches
	nident means Number of identical matches
	mismatch means Number of mismatches
	positive means Number of positive - scoring matches
	gapopen means Number of gap openings
	gaps means Total number of gaps
	ppos means Percentage of positive - scoring matches
	qframe means Query frame
	btop means Blast traceback operations(BTOP)
	staxids means unique Subject Taxonomy ID(s), separated by a ';' (in numerical order)
	stitle means Subject Title
	salltitles means All Subject Title(s), separated by a '<>'
	qcovhsp means Query Coverage Per HSP
	qtitle means Query title
	qqual means Query quality values for the aligned part of the query
	full_qqual means Query quality values
	qstrand means Query strand

```


#try using megan LCA alogrithm for the classification
```
#https://metagenomics-workshop.readthedocs.io/en/latest/annotation/taxonomic_annotation.html

diamond blastp -p 6 -d sulfur_marker_gene_annotree_db --daa sulfur_owc_diamond.results -q sulfur_marker_gene_OWC.fas --ultra-sensitive

diamond view -a sulfur_owc_diamond.results > sulfur_owc_diamond.search_result.tab
 sed -e 's/___.*_[0-9]*\t/\t/1' sulfur_owc_diamond.search_result.tab >sulfur_owc_diamond.search_result_fix.tab

http://megan.informatik.uni-tuebingen.de/t/build-custom-database-with-accession/563

#
accession2taxid abin file 

o add to an incomplete mapping, in addition to the abin file, you can also specify an “synonyms” file, and the lines should be formatted like this:
UQO_contig00001.1_1 tab taxon-id
UQO_contig00001.1_2 tab taxon-id
…
Then, specify both the .abin file (as accession mapping file) and your .map file containing such lines as a synonyms file and that should work.


# /Applications/MEGAN/tools/

```

