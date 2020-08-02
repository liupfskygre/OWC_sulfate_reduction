#


##

```
makeblastdb -in all_sequences.fasta -dbtype prot -out HCOx_classifier_allSeq -parse_seqids

blastp -query query.fasta -db HCOx_classifier_allSeq -outfmt 6 -max_target_seqs 1 -num_threads 6 -out out6MTS3.txt
```

#use diamond instead of blast due to the "-max_target_seqs" issue 
#--max-target-seqs
```
#https://github.com/bbuchfink/diamond/issues/232

conda activate diamondv201

diamond makedb --in db.fasta
diamodn blastp -p 4 -d db -o out.file -f 6 -q query.fasta --max-target-seqs 1 --very-sensitive
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
