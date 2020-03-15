#a collection of sulfur cycling related marker genes hmmprofile 

#ref
#download tigrfam on mac
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/database_model_profiles_archive/gtdbtk_marker/tigrfam



```


#kofam
```

cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Marker_gene_hmmprofile/sulfur_cycling_KOfam

touch sulfur_related_kofam.list #41 
for file in $(cat sulfur_related_kofam.list)
do
cp ./profiles/"${file}".hmm ./
done
# K00386.hmm: No such file or directory
#one duplicate (phsA==prsA), 39 items

cat *.hmm>sulfur_cycling_kofam.hmm


/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Marker_gene_hmmprofile/sulfur_cycling_KOfam/profiles
```

#tigrfam
```
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Marker_gene_hmmprofile/sulfur_cycling_tigrfam


touch sulfur_related_tigrfam.list #

for file in $(cat sulfur_related_tigrfam.list)
do
cp /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/database_model_profiles_archive/gtdbtk_marker/TIGRFAMs_15.0_HMM/"${file}".HMM ./
done

cp: /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/database_model_profiles_archive/gtdbtk_marker/tigrfam/individual_hmms/TIGR04315.HMM: No such file or directory

/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/database_model_profiles_archive/gtdbtk_marker/tigrfam

cat *.HMM >sulfur_cycling_tigrfam.hmm

```

#download pfam files
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Marker_gene_hmmprofile/sulfur_cycling_pfam

cat *.hmm>sulfur_cycling_pfam.hmm
```

#download specific eggnog 
```
cat *.hmm>sulfur_cycling_eggnog.hmm


```

#cat all profiles togerther for a search
```
cat ./sulfur_cycling_pfam/sulfur_cycling_pfam.hmm ./sulfur_cycling_specific_eggNOG/sulfur_cycling_eggnog.hmm ./sulfur_cycling_tigrfam/sulfur_cycling_tigrfam.hmm ./sulfur_cycling_KOfam/sulfur_cycling_kofam.hmm >sulfur_cycling_hmmprofile.hmm

```


#searching db create
```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/dereplicated_genomes

#based on the prodigal prediction file 
#
[liupf@zenith prodigal_files]$ cat *.faa >~/combined_owc_3211_prodigal.faa
[liupf@zenith prodigal_files]$ pwd
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/prodigal_files
[liupf@zenith prodigal_files]$ 

cd ~
grep -c '>' combined_owc_3211_prodigal.faa. #use this as the database to search all 
9394752


```

#do hmm search , first try
```
cd ~
mkdir sulfur_cycling_owc_hmmsearh

/home/liupf/sulfur_cycling_owc_hmmsearh

screen -S hmmsearch
hmmsearch --cpu 20 --tblout sulfur_cycling_tblout.txt --domtblout sulfur_cycling_domtblout.txt sulfur_cycling_hmmprofile.hmm combined_owc_3211_prodigal.faa

###
grep -v '^#' sulfur_cycling_tblout.txt > sulfur_cycling_tblout_edit1.txt
sed -i -e 's/ \+/\t/g' sulfur_cycling_tblout_edit1.txt 

grep -v '^#' sulfur_cycling_domtblout.txt>sulfur_cycling_domtblout_edit1.txt
[liupf@zenith sulfur_cycling_owc_hmmsearh]$ sed -i -e 's/ \+/\t/g' sulfur_cycling_domtblout_edit1.txt
[liupf@zenith sulfur_cycling_owc_hmmsearh]$ head sulfur_cycling_domtblout_edit1.txt

awk -F '\t' '$5<1e-3' sulfur_cycling_tblout_edit1.txt > sulfur_cycling_tblout_edit1E3.txt
cat sulfur_cycling_tblout_edit1E3.txt |cut -f1 -d$'\t'|sort|uniq > sulfur_cycling_tblout_edit1E3_f1uniq.txt

#
> a <- read.delim("sulfur_cycling_tblout_edit1E3_f1uniq.txt",header=F, check.names=FALSE) 
> View(a)
> b<-read.delim("wetlands_db_contigs_to_bins.tsv",header=F, check.names=FALSE, sep="\t") 
> c<- merge(a,b, by.x="V1", by.y="V1", all.x=T, all.y=F, sort=F)
> write.table(c, file = "sulfur_cycling_bins_contigs.tsv", sep = '\t',row.names = F)
#3093 genomes found.


```
#change to just use Tigrfam and pfam, not use KOfam and COG

```
#



```



#Adrienne created coverage file and relabel#
```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/read_mapping_vs_relabeled_derepped_genomes

/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes

????
[liupf@zenith relabeled_dereplicated_genomes]$ grep -c '>' combined_wetland_bins_genes.faa
9185226
[liupf@zenith relabeled_dereplicated_genomes]$ grep -c '>' combined_wetland_bins_scaffolds.fa
1805837

????
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins/prodigal_files
[liupf@zenith prodigal_files]$ cat *.faa|grep -c '>' -
9394752 # why this is different from the combined bins_genes

[liupf@zenith relabeled_dereplicated_genomes]$ cat *.fa|grep -c '>' -
1805837 #the same as the combined 



##
##besides MBNT-15
what are the DRAM for, just to dram all genomes we need to use later?

##????
ls *.fna|wc -l
3210

```
