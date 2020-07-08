##
```
soeA	K21307
soeB	K21308
soeC	K21309

sreA	K17219.hmm
sreB	K17220.hmm
sreC	K17221.hmm

sir/mccA	TIGR02042？#not in METABOLIC database
otr TIGR04315？ # METABOLIC database

tsdA (cytochrome)	K19713	thiosulfate dehydrogenase [EC:1.8.2.2]
ttrA	K08357	ttrA; tetrathionate reductase subunit A
ttrB	K08358	ttrB; tetrathionate reductase subunit B
ttrC	K08359	ttrC; tetrathionate reductase subunit C

pengfeiliu /Users/pengfeiliu/software/METABOLIC-master/kofam_database/profiles $ nano kegg_short_list

keep a short list and run METABOLIC 
# silence the  whole datbase

for file in $(cat kegg_short_list)
do 
mv ${file}.hmm ./kegg_short_hmm
done
```

#clean last run and run METABOLIC on selected kofam hmm
```
cd /Users/pengfeiliu/software/METABOLIC-master

for file in $(cat /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/metaG16_prodigal.txt) 
do 
echo $file
#mkdir /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/${file}_contigs
#mv ${file} /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/${file}_contigs

#METBOLIC requeire sequences names no '#', otherwise error

#sed -i -e 's/#/ /g' /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/${file}_contigs/$file
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/${file}_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/${file}_contigs
done

```

```
memory issue,
#AugM2C1D5C_prodigal.faa
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugM2C1D5C_prodigal.faa_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugM2C1D5C_prodigal.faa_contigs


#AugN3C1D1A_prodigal.faa
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugN3C1D1A_prodigal.faa_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugN3C1D1A_prodigal.faa_contigs

#AugN3C1D5A_prodigal.faa
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugN3C1D5A_prodigal.faa_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugN3C1D5A_prodigal.faa_contigs

#AugOW2C1D1C_prodigal.faa
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugOW2C1D1C_prodigal.faa_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugOW2C1D1C_prodigal.faa_contigs


#AugOW2C1D5C_prodigal.faa
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugOW2C1D5C_prodigal.faa_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugOW2C1D5C_prodigal.faa_contigs


AugT1C1D1A_prodigal.faa
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugT1C1D1A_prodigal.faa_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugT1C1D1A_prodigal.faa_contigs

AugT1C1D5A_prodigal.faa
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugT1C1D5A_prodigal.faa_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugT1C1D5A_prodigal.faa_contigs

MayM1C1D1A_prodigal.faa
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/MayM1C1D1A_prodigal.faa_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/MayM1C1D1A_prodigal.faa_contigs

MayM1C1D3A_prodigal.faa
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/MayM1C1D3A_prodigal.faa_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/MayM1C1D3A_prodigal.faa_contigs

MayM1C1D6A_prodigal.faa
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/MayM1C1D6A_prodigal.faa_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/MayM1C1D6A_prodigal.faa_contigs
```

#build raw gene database of each functional 
```
#MAC
cd /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length

for folder in $(cat sample_sub_dir.txt)
do 
cd ./${folder}/Each_HMM_Amino_Acid_Sequence/
pwd
for file in *.faa
do
cp ${file} /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/Each_HMM_Amino_Acid_Sequence/${folder}_${file}
done
cd ../../
done

#
cd Each_HMM_Amino_Acid_Sequence 
for file in $(cat ../METABOLIC_inluded_hmm_hits_list_2.txt)
do 
cat *${file} > ../"${file}".faa
done 

for file in *.faa.faa; do mv ${file} "${file%.*}"; done


#1, get gene names from hmmsearch hits 
.collection.faa
>AugM1C1D1B_prodigal~~k121_10370813_4

#for nt sequecnes, fix gene names first ==> add sample names to header and match METABOLIC style


#pullseq and remove duplicates sequences based on nt sequences

```
#
```
#pull seqs id 
for file in K21307 K21308 K21309 K19713 K08357 K08358
do 
#grep -e '>' "${file}".hmm.collection.faa >  "${file}".hmm.collection_id.txt

sed -i -e 's/>//g' "${file}".hmm.collection_id.txt 
done


for file in *faa
do 
echo "${file}" >> counts_of_hits.txt
grep -c '>' ${file} >> counts_of_hits.txt
done

#one bug
total~~k121_2550817_2===AugM2C1D5C_prodigal~~k121_2550817_2

```
