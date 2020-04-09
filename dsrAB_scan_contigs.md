#use dsrAB hmmprofile to search contigs profile

#all data analysis
cd /home/projects/Wetlands/sulfur_cycling_analysis


#contigs file
```
/home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/OWC2018_16contigs_path.txt
nano OWC_metaG_2013_14_contigs_path.txt

#check prodigal orf calling
for file in $(cat OWC_metaG_2013_14_contigs_path.txt); do ls -1 ${file}/*prodigal*.fna;done
for file in $(cat OWC_metaG_2013_14_contigs_path.txt); do ls -1 ${file}/*prodigal*.faa;done

for file in $(cat OWC2018_16contigs_path.txt); do ls -1 ${file}/*megahit*600_prodigal.fna;done
for file in $(cat OWC2018_16contigs_path.txt); do ls -1 ${file}/*megahit*600_prodigal.faa;done


```

#hmmprofile
```
cp /home/liupf/sulfur_cycling_owc_hmmsearh/sulfur_cycling_pfam/*.hmm /home/projects/Wetlands/2018_sampling/hmm_profiles
cp /home/liupf/sulfur_cycling_owc_hmmsearh/sulfur_cycling_tigrfam/TIGR*.HMM /home/projects/Wetlands/2018_sampling/hmm_profiles

mv APS-reductase_C.hmm aprB_PF12139.hmm
mv ATP-sulfurylase.hmm Sat_PF01747.hmm
mv FCSD-flav_bind.hmm fccB_PF09242
mv sulfide_quinone_oxidoreductase_sqr.hmm sqr_KA.hmm
mv sulfur_dioxygenase_sdo.hmm sdo_KA.hmm
mv thiosulfate_reductase_phsA.hmm phsA_KA.hmm
```

#fix prodigal header
```
grep -v '#' OWC_MetaG_contigs_fix_header_Apr2020.txt
IFS=$'\n'
for sample in $(cat file1.txt)

do
#echo ${sample}
#get sample name
name=$(echo ${sample}| cut -f1 -d$'\t' )

#get sample prodigal.faa
protein=$(echo ${sample}| cut -f2 -d$'\t')

#get sample DNAseqs
DNAseqs=$(echo ${sample}| cut -f3 -d$'\t')
echo ${protein}
head -1 ${protein}
echo ${DNAseqs}
head -1 ${DNAseqs}
done
unset IFS=$'\n'
```


#pipeline
```
#profile metadata text: include profile name, profile GA, TC, filtering length
# file include sample name and all path to protein and nt file

#out, (1), protein reads, (2), nt reads meeting cutoff

/home/projects/Wetlands/2018_sampling/scripts/fetch_contigs_to_mcrA_seqs.sh
```


#kofam is relative easy, will give a
