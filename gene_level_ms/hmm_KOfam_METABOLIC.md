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

