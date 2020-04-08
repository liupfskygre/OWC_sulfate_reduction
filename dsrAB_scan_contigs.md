#use dsrAB hmmprofile to search contigs profile

#all data analysis
cd /home/projects/Wetlands/sulfur_cycling_analysis


#contigs file
```
/home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/OWC2018_16contigs_path.txt
nano OWC_metaG_2013_14_contigs_path.txt

#check prodigal orf calling
for file in $(cat OWC_metaG_2013_14_contigs_path.txt); do ls ${file}/*prodigal*.fna;done
for file in $(cat OWC_metaG_2013_14_contigs_path.txt); do ls ${file}/*prodigal*.faa;done

for file in $(cat OWC2018_16contigs_path.txt); do ls ${file}/*megahit*600_prodigal.fna;done
for file in $(cat OWC2018_16contigs_path.txt); do ls ${file}/*megahit*600_prodigal.faa;done


```

#hmmprofile


#pipeline
```
#profile metadata text: include profile name, profile GA, TC, filtering length
# file include sample name and all path to protein and nt file 

#out, (1), protein reads, (2), nt reads meeting cutoff

/home/projects/Wetlands/2018_sampling/scripts/fetch_contigs_to_mcrA_seqs.sh
```


#kofam is relative easy, will give a
