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
grep -v '^#' sulfur_cycling_tblout.txt > sulfur_cycling_tblout_edit1.txt. #41164
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

grep -c 'PF00581' sulfur_cycling_tblout_edit1.txt
grep -c 'PF09242' sulfur_cycling_tblout_edit1.txt
grep -c 'PF13501' sulfur_cycling_tblout_edit1.txt
grep -c 'PF08770' sulfur_cycling_tblout_edit1.txt
```
#change to just use Tigrfam and pfam, not use KOfam and COG

```
#
#pfam
cd /home/liupf/sulfur_cycling_owc_hmmsearh/sulfur_cycling_pfam
screen -r hmmsearch

for file in *.hmm
do
hmmsearch --cpu 20 --tblout "${file}"_tblout.txt --domtblout "${file}"_domtblout.txt $file ../combined_owc_3211_prodigal.faa &>"${file}".log
done

for file in DoxA.hmm
do
hmmsearch --cpu 20 --tblout "${file}"_tblout.txt --domtblout "${file}"_domtblout.txt $file ../combined_owc_3211_prodigal.faa &>"${file}".log
done


#tigrfam
screen -s hmmsearch2
for file in *.HMM
do
hmmsearch --cpu 20 --tblout "${file}"_tblout.txt --domtblout "${file}"_domtblout.txt $file ../combined_owc_3211_prodigal.faa &>"${file}".log
done

#kofam

screen -S hmmsearch2
for file in *.hmm
do
hmmsearch --cpu 20 --tblout "${file}"_tblout.txt --domtblout "${file}"_domtblout.txt $file ../combined_owc_3211_prodigal.faa &>"${file}".log
done
```

#filtering
```
#$3, len, >75% of the hmm len
#$7, evale <1e-5 #use bitscore as final cutoff
#$8, whole seq cutoff 
#$14, domain cutoff

#Each model has a trusted cutoff, above which there should be no false positive hits, and a noise cutoff below which hits to the model are considered uninteresting. The range between trusted cutoff and noise cutoff represents scores that may or may not be true hits.

#JGI-DOE procedure, (Marcel Huntemann, 2015)
###For TIGRfam, the noise cutoff (−−cug_nc) is used, with hits below the trusted cutoff and at/above the noise cutoff flagged as “marginal”. For Pfam, the gathering threshold (−−cut_ga) is used inside the pfam_scan.pl script.
## kofam, could only use KofamScan tools ??? We developed a database of pHMMs based on the KO and GENES databases. The adaptive score thresholds are pre-computed for individual KO families, and can be used to assign KOs (K numbers) to sequences using KofamScan and KofamKOALA. Sequence matches with scores above the thresholds are considered more reliable than other matches and thus highlighted with ‘*’ marks in the output of these tools.

#example TIGR04555.HMM
awk -F '\t' '$3>300 &&$8>420 && $14>420' TIGR04555.HMM_domtblout.txt|wc -l
awk -F '\t' '$3>300 &&$8>330 && $14>330' TIGR04555.HMM_domtblout.txt|wc -l
cat TIGR04555.HMM_domtblout.txt |cut -f3,7,8,13,14 -d$'\t' |head 

awk -F '\t' '$3>$a &&$8>$b && $14>$b' TIGR04555.HMM_domtblout.txt|wc -l
awk -F '\t' -v  '$3>$a &&$8>$b && $14>$b' TIGR04555.HMM_domtblout.txt|wc -l
```
## fitering standard ##
```
domtblout.txt
#1, length, $3 > 75% percent of HMM len
#2, cuoff, tigrfam, use full noise cutoff $8> && domain $14> 
#3, For Pfam, the gathering threshold
#4, kofam, ???? KofamScan or hmmsearch ?
awk -F '\t' '$3>300 &&$8>330 && $14>330'
```

## filtering Tigrfam
```
#cd /home/liupf/sulfur_cycling_owc_hmmsearh/sulfur_cycling_tigrfam

for file in *domtblout.txt
do 
grep -v '^#' $file >"${file%.*}"_e.txt
sed -i -e 's/ \+/\t/g'  "${file%.*}"_e.txt
done

awk -F '\t' '$8>181.8 && $14>181.8&& $3>288' TIGR00339.HMM_domtblout_e.txt>sat.met3_TIGR00339_domtblout_filter.txt #322.95	181.8 383 288

awk -F '\t' '$8>326  && $14>326  && $3>462' TIGR02061.HMM_domtblout_e.txt >aprA_TIGR02061_domtblout_filter.txt #aprA	TIGR02061	641.75	326	616	462

awk -F '\t' '$8>55.15  && $14>55.15  && $3>99 ' TIGR02060.HMM_domtblout_e.txt> aprB_TIGR02060_domtblout_filter.txt #aprB	TIGR02060	135.65	55.15	132	99

awk -F '\t' '$8>222.85  && $14>222.85  && $3>302 ' TIGR02064.HMM_domtblout_e.txt >dsrA_TIGR02064_domtblout_filter.txt #dsrA	TIGR02064	392.85	222.85	402	302

awk -F '\t' '$8> 205.25 && $14>205.25  && $3>256' TIGR02066.HMM_domtblout_e.txt >dsrB_TIGR02066_domtblout_filter.txt #dsrB	TIGR02066	341.5	205.25	341	256

awk -F '\t' '$8>223.6  && $14>223.6  && $3>251' TIGR02910.HMM_domtblout_e.txt >asrA_TIGR02910_domtblout_filter.txt #asrA	TIGR02910	323.15	223.6	334	251

awk -F '\t' '$8>226.95  && $14>226.95 && $3> 196' TIGR02911.HMM_domtblout_e.txt>asrB_TIGR02911_domtblout_filter.txt #asrB	TIGR02911	288.6	226.95	261	196


awk -F '\t' '$8>194.3  && $14>194.3  && $3>236' TIGR02912.HMM_domtblout_e.txt >asrC_TIGR02912_domtblout_filter.txt #asrC_TIGR02912	324.1	194.3	314	236

awk -F '\t' '$8> 16.7 && $14>16.7  && $3>22 ' .HMM_domtblout_e.txt #phsA	TIGR01409	16.7	16.7	29	22??
awk -F '\t' '$8>  && $14>  && $3> ' .HMM_domtblout_e.txt #fccB	TIGR01409 	16.7	16.7	29	22?

awk -F '\t' '$8>100  && $14>60  && $3> 159' TIGR04484.HMM_domtblout_e.txt >soxA_TIGR04484_domtblout_filter.txt #soxA	TIGR04484	110	100/60.00	212	159

awk -F '\t' '$8> 60 && $14> 45 && $3>59' TIGR04485.HMM_domtblout_e.txt> soxX_TIGR04485_domtblout_filter.txt #soxX	TIGR04485	61	60.00/45.00	78	59

awk -F '\t' '$8> 550 && $14> 550 && $3>417' TIGR04486.HMM_domtblout_e.txt>soxB_TIGR04486_domtblout_filter.txt #soxB_TIGR04486	550	550	556	417

awk -F '\t' '$8>330  && $14> 330 && $3>306' TIGR04555.HMM_domtblout_e.txt >soxC_TIGR04555_domtblout_filter.txt #soxC	TIGR04555	420	330	408	306


awk -F '\t' '$8>120  && $14>120  && $3>112' TIGR04488.HMM_domtblout_e.txt >soxY_TIGR04488_domtblout_filter.txt #soxY	TIGR04488	125	120	149	112
awk -F '\t' '$8>69  && $14>69  && $3> 74' TIGR04490.HMM_domtblout_e.txt >soxZ_TIGR04490_domtblout_filter.txt  #soxZ	TIGR04490 	76	69	98	74

awk -F '\t' '$8>  && $14>  && $3> ' .HMM_domtblout_e.txt #sreA	TIGR01409	16.7	16.7	29	22???

awk -F '\t' '$8>325  && $14>325  && $3>330 ' TIGR04315.HMM_domtblout_e.txt >otr_TIGR04315_domtblout_filter.txt #otr	TIGR04315	375	325	440	330

```

## filtering pfam
```

for file in *domtblout.txt
do 
grep -v '^#' $file >"${file%.*}"_e.txt
sed -i -e 's/ \+/\t/g'  "${file%.*}"_e.txt
done

#
awk -F '\t' '$8>23.4 && $14>23.4 && $3>159 ' ATP-sulfurylase.hmm_domtblout_e.txt >Sat_PF01747_domtblout_filter.txt #sat/met3	PF01747 ATP-sulfurylase	23.4	23.3	212	159

awk -F '\t' '$8>25 && $14>25 && $3>62 ' APS-reductase_C.hmm_domtblout_e.txt  >aprB_PF12139_domtblout_filter.txt #aprB	PF12139 APS-reductase_C	25	25	82	62

awk -F '\t' '$8>21 && $14>6.8 && $3>119 ' NIR_SIR.hmm_domtblout_e.txt  >dsrAB_pf01077_domtblout_filter.txt  #dsrA	pf01077	21	6.8	159	119

awk -F '\t' '$8>24.2 && $14>24.2 && $3>48 ' DsrD.hmm_domtblout_e.txt  >dsrD_pf08679_domtblout_filter.txt
#dsrD	pf08679	24.2	24.2	64	48

awk -F '\t' '$8>24.2 && $14>24.2 && $3>53 ' FCSD-flav_bind.hmm_domtblout_e.txt >fccB_PF09242_domtblout_filter.txt
#fccB	PF09242	21.5	21.5	70	53

awk -F '\t' '$8>28.1 && $14>28.1 && $3>84 ' SoxY.hmm_domtblout_e.txt >soxY_PF13501_domtblout_filter.txt
#soxY	PF13501	28.1	28.1	112	84

awk -F '\t' '$8>20 && $14>20 && $3>72 ' SoxZ.hmm_domtblout_e.txt >soxZ_PF08770_domtblout_filter.txt
#soxZ	PF08770	20	20	96	72

awk -F '\t' '$8>25 && $14>25 && $3>227 ' SOR.hmm_domtblout_e.txt >SOR_PF07682_domtblout_filter.txt
#SOR	PF07682	25	25	302	227

awk -F '\t' '$8>22.7 && $14>22.7 && $3>125 ' DoxD.hmm_domtblout_e.txt >DoxD_PF04173_domtblout_filter.txt
#DoxD_PF04173 	22.7	22.7	167	125

awk -F '\t' '$8>20.7 && $14>20.7 && $3>98 ' DoxA.hmm_domtblout_e.txt >DoxA_PF07680_domtblout_filter.txt
#PF07680 DoxA	20.7	20.7	130	98

awk -F '\t' '$8>22.5 && $14>22.5 && $3>64 ' DoxX.hmm_domtblout_e.txt > DoxX_PF07681_domtblout_filter.txt
#PF07680 DoxX	22.5	22.5	85	64

awk -F '\t' '$8>100 && $14>100 && $3>311 ' sulfide_quinone_oxidoreductase_sqr.hmm_domtblout_e.txt > sqr_domtblout_filter.txt
#K.A.sqr_alignment	100	100	415	311

awk -F '\t' '$8>200 && $14>200 && $3>184 ' sulfur_dioxygenase_sdo.hmm_domtblout_e.txt> sdo_domtblout_filter.txt
#A.K.sdo	200	200	245	184

awk -F '\t' '$8>300 && $14>300 && $3>569 ' thiosulfate_reductase_phsA.hmm_domtblout_e.txt> phsA_domtblout_filter.txt
#K.A.phs	300	300	758	569
```


## Kofamscan
```
#kofamcan dir
/home/liupf/software_liu/kofamscan

#only report with reads above threshold (score >threshold, and E-value < setted)

/home/liupf/software_liu/kofamscan/kofamscan-1.2.0/exec_annotation -o sulfur_cycling_kofamscan.txt -p sulfur_cyclinge_profile -k sulfur_related_kofam_list -f mapper --keep-tabular --no-report-unannotated -E 1e-5  --cpu 12 ../combined_owc_3211_prodigal.faa &>sulfur_kofamscan.txt

#E5 
#rich formate, default [detail], will report all inlucding below reshold, use star(*) to get the real ones
/home/liupf/software_liu/kofamscan/kofamscan-1.2.0/exec_annotation -o sulfur_cycling_kofamscan_rich.txt -p sulfur_cyclinge_profile -k sulfur_related_kofam_list --no-report-unannotated -E 1e-5  --cpu 24 ../combined_owc_3211_prodigal.faa &>sulfur_kofamscan_rich_log.txt
grep '\*' sulfur_cycling_kofamscan_rich.txt >sulfur_cycling_kofamscan_rich_True_hits.txt #11549
cat sulfur_cycling_kofamscan.txt |cut -f2 -d$'\t' |sort|uniq -c

#E3
/home/liupf/software_liu/kofamscan/kofamscan-1.2.0/exec_annotation -o sulfur_cycling_kofamscan_rich_E3.txt -p sulfur_cyclinge_profile -k sulfur_related_kofam_list --no-report-unannotated -E 1e-3  --cpu 40 ../combined_owc_3211_prodigal.faa &>sulfur_kofamscan_rich_logE3.txt
grep '\*' sulfur_cycling_kofamscan_rich_E3.txt >sulfur_cycling_kofamscan_rich_True_hits_E3.txt #11641
# cat sulfur_cycling_kofamscan.txt |cut -f2 -d$'\t' |sort|uniq -c 


#creat profile and list for kofamscan
cd /home/liupf/sulfur_cycling_owc_hmmsearh/sulfur_cycling_pfam
/home/liupf/sulfur_cycling_owc_hmmsearh/sulfur_cycling_KOfam_kofamcan
grep -f sulfur_related_kofam.list /home/liupf/software_liu/kofamscan/ko_list >sulfur_related_kofam_list


##E5 is similar to the output from tigrfam
#use this one


```

# data summary

## sulfate reduction pathway
```
cd /home/liupf/sulfur_cycling_owc_hmmsearh/sulfate_reduction_hits

##kofamscan based
cp ../sulfur_cycling_KOfam_kofamcan/sulfur_cycling_kofamscan.txt ./
grep -w -f dsrAB_aprAB_KO.list sulfur_cycling_kofamscan.txt >dsrAB_aprAB_KO_kofamscan.txt

cat dsrAB_aprAB_KO_kofamscan.txt |cut -f1 -d$'\t' > dsrAB_aprAB_KO_kofamscan_seqs.txt
sed -i -e 's/_[0-9]*$//1' dsrAB_aprAB_KO_kofamscan_seqs.txt
cat dsrAB_aprAB_KO_kofamscan_seqs.txt|sort|uniq> dsrAB_aprAB_KO_kofamscan_seqs_uniq.txt
grep -w -f dsrAB_aprAB_KO_kofamscan_seqs_uniq.txt ../wetlands_db_contigs_to_bins.tsv > dsrAB_aprAB_KO_kofamscan_seqs_bin_list.txt

cat dsrAB_aprAB_KO_kofamscan_seqs_bin_list.txt|cut -f2 -d$'\t' |sort|uniq > dsrAB_aprAB_KO_kofamscan_bin_list_uniq.txt
#
sed -i -e 's/\.relabeled//1' dsrAB_aprAB_KO_kofamscan_bin_list_uniq.txt
sed -i -e 's/\.fa//1' dsrAB_aprAB_KO_kofamscan_bin_list_uniq.txt

grep -w -f dsrAB_aprAB_KO_kofamscan_bin_list_uniq.txt ../gtdb_and_checkm_for_dram.txt >dsrAB_aprAB_bins_gtdbtk_dram.txt

wc -l dsrAB_aprAB_KO_kofamscan_bin_list_uniq.txt


##tigrfam based
TIGR02061
TIGR02060
TIGR02064
TIGR02066
aprB	PF12139 APS-reductase_C
dsrD	pf08679



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
