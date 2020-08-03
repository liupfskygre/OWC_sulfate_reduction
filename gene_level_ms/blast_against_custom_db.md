#


##

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
TPM_Gene_W_5ktax.xlsx 
../ar_ba_taxonomy_r95.tsv

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