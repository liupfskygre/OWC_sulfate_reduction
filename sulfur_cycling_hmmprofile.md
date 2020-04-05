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
#none rich
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

#April-2-2020, recheck
asrABC_ko_list_kofam.list
/home/liupf/software_liu/kofamscan/kofamscan-1.2.0/exec_annotation -o asrABC_kofamscan_rich.txt -p asrABC.hal -k asrABC_ko_list_kofam.list --no-report-unannotated -E 1e-5  --cpu 24 ../combined_owc_3211_prodigal.faa
#Error: Unknown KO: K00385, maybe profile and list should be 1to1
nano asrABC.hal
K00385
K16950
K16951

for file in $(cat re_run_kofam.txt)
do
/home/liupf/software_liu/kofamscan/kofamscan-1.2.0/exec_annotation -o "${file}"_kofamscan_rich.txt -p ./sulfur_cyclinge_profile/"${file}".hmm -k sulfur_related_kofam_list --no-report-unannotated -E 1e-5  --cpu 24 ../combined_owc_3211_prodigal.faa &>"${file}".log
done
for file in $(cat re_run_kofam.txt)
#for file in K00392
do
wc -l "${file}"_kofamscan_rich.txt
grep -c "${file}" "${file}"_kofamscan_rich.txt
grep "${file}" "${file}"_kofamscan_rich.txt|grep -c '\*'
done
#without '\*' significant hits
```

mccA kofamscan and tigrfam
```
/home/liupf/software_liu/kofamscan/kofamscan-1.2.0/exec_annotation -o K00392_kofamscan_rich.txt -p ./sulfur_cyclinge_profile/K00392.hmm -k sulfur_related_kofam_list --no-report-unannotated -E 1e-5  --cpu 24 ../combined_owc_3211_prodigal.faa &>K00392.log
grep '\*' K00392_kofamscan_rich.txt >K00392_kofamscan_hits.txt
#convert to simply formate
sed 's/ \+ /\t/g' K00392_kofamscan_hits.txt > K00392_kofamscan_hits_e.txt
sed -i -e 's/\* //g' K00392_kofamscan_hits_e.txt

cat K00392_kofamscan_hits_e.txt |cut -f1,2 -d$'\t' >K00392_kofamscan_hits_f12.txt
#24

screen -r hmmsearch2
for file in TIGR02042.HMM
do
hmmsearch --cpu 20 --tblout "${file}"_tblout.txt --domtblout "${file}"_domtblout.txt $file ../combined_owc_3211_prodigal.faa &>"${file}".log
done

for file in TIGR02042.HMM_domtblout.txt
do
grep -v '^#' $file >TIGR02042.HMM_domtblout_e.txt
sed -i -e 's/ \+/\t/g'  TIGR02042.HMM_domtblout_e.txt
done

awk -F '\t' '$8>574.70  && $14>574.70  && $3>436 ' TIGR02042.HMM_domtblout_e.txt >mccA_TIGR02042_domtblout_filter.txt #mcca
TIGR02042 sir	731.5	"731.5/	574.70"	581	436
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

##fix naming issue in the wetlands_db_contigs_to_bins.tsv
sed -i -e 's/O3C3D3_metabat_w_DDIG_2-5kb/O3C3D3_metabat_w_DDIG_2.5k/g'  dsrAB_aprAB_KO_kofamscan_bin_list_uniq.txt

sed -i -e 's/M3C4D4_metabat_w_DDIG_2-5kb/M3C4D4_metabat_w_DDIG_2.5kb/g'  dsrAB_aprAB_KO_kofamscan_bin_list_uniq.txt

sed -i -e 's/O3C3D4_metabat_w_DDIG_2-5kb/O3C3D4_metabat_w_DDIG_2.5kb/g'  dsrAB_aprAB_KO_kofamscan_bin_list_uniq.txt #607 genomes

grep -w -f dsrAB_aprAB_KO_kofamscan_bin_list_uniq.txt ../gtdb_and_checkm_for_dram.txt >dsrAB_aprAB_bins_gtdbtk_dram2.txt
#607


##tigrfam based
for file in  TIGR02061 TIGR02060 TIGR02064 TIGR02066
do
cp  ../sulfur_cycling_tigrfam/*_"${file}"_domtblout_filter.txt ./
done

#aprB	PF12139 APS-reductase_C
#dsrD	pf08679
cp ../sulfur_cycling_pfam/dsrD_pf08679_domtblout_filter.txt ./
cp ../sulfur_cycling_pfam/aprB_PF12139_domtblout_filter.txt ./
cat *TIGR* dsrD_pf08679_domtblout_filter.txt aprB_PF12139_domtblout_filter.txt  >Tigr_pfam_sulfate_seqs.txt

cat Tigr_pfam_sulfate_seqs.txt|cut -f1 -d$'\t' >Tigr_pfam_sulfate_seqs_f1.txt
sed -i -e 's/_[0-9]*$//1' Tigr_pfam_sulfate_seqs_f1.txt
cat Tigr_pfam_sulfate_seqs_f1.txt |sort|uniq >Tigr_pfam_sulfate_seqs_f1_uniq.txt #786

grep -w -f Tigr_pfam_sulfate_seqs_f1_uniq.txt ../wetlands_db_contigs_to_bins.tsv > dsrABD_aprAB_seqs_bin_list.txt

cat dsrABD_aprAB_seqs_bin_list.txt|cut -f2 -d$'\t' |sort|uniq > dsrABD_aprAB_seqs_bin_list_uniq.txt
#
sed -i -e 's/\.relabeled//1' dsrABD_aprAB_seqs_bin_list_uniq.txt
#
sed -i -e 's/\.fa//1' dsrABD_aprAB_seqs_bin_list_uniq.txt
#

##fix naming issue in the wetlands_db_contigs_to_bins.tsv
sed -i -e 's/O3C3D3_metabat_w_DDIG_2-5kb/O3C3D3_metabat_w_DDIG_2.5k/g'  dsrABD_aprAB_seqs_bin_list_uniq.txt

sed -i -e 's/M3C4D4_metabat_w_DDIG_2-5kb/M3C4D4_metabat_w_DDIG_2.5kb/g'  dsrABD_aprAB_seqs_bin_list_uniq.txt

sed -i -e 's/O3C3D4_metabat_w_DDIG_2-5kb/O3C3D4_metabat_w_DDIG_2.5kb/g'  dsrABD_aprAB_seqs_bin_list_uniq.txt #576

grep -w -f dsrABD_aprAB_seqs_bin_list_uniq.txt ../gtdb_and_checkm_for_dram.txt >dsrABD_aprAB_bins_gtdbtk_dram2.txt

wc -l dsrABD_aprAB_bins_gtdbtk_dram2.txt #576 genomes

#add two together
cat dsrAB_aprAB_KO_kofamscan_bin_list_uniq.txt dsrABD_aprAB_seqs_bin_list_uniq.txt |sort |uniq >kofamscan_tigr_pfam_bin_list_uniq.txt #619 genomes

#
grep -w -f kofamscan_tigr_pfam_bin_list_uniq.txt ../gtdb_and_checkm_for_dram.txt >sulfate_reducing_bins_gtdbtk_dram2.txt #619

#with 619 genomes with aprAB and/or dsrAB
```

#
```
mkdir sox_system_hits

/home/liupf/sulfur_cycling_owc_hmmsearh
nano sox_ko_list.txt

grep -w -f sox_ko_list.txt ../sulfur_cycling_KOfam_kofamcan/sulfur_cycling_kofamscan.txt  >sox_ko_list_KOfamScan.txt

cat sox_ko_list_KOfamScan.txt |cut -f1 -d$'\t' > sox_ko_list_KOfamScan_seqs.txt

sed -i -e 's/_[0-9]*$//1' sox_ko_list_KOfamScan_seqs.txt

cat sox_ko_list_KOfamScan_seqs.txt|sort|uniq> sox_ko_list_KOfamScan_seqs_uniq.txt

grep -w -f sox_ko_list_KOfamScan_seqs_uniq.txt ../wetlands_db_contigs_to_bins.tsv > sox_ko_list_KOfamScan_seqs_bins_list.txt

cat sox_ko_list_KOfamScan_seqs_bins_list.txt|cut -f2 -d$'\t' |sort|uniq > sox_ko_list_KOfamScan_seqs_bins_list_uniq.txt  
### 492
#
sed -i -e 's/\.relabeled//1' sox_ko_list_KOfamScan_seqs_bins_list_uniq.txt
sed -i -e 's/\.fa//1' sox_ko_list_KOfamScan_seqs_bins_list_uniq.txt

##fix naming issue in the wetlands_db_contigs_to_bins.tsv
sed -i -e 's/O3C3D3_metabat_w_DDIG_2-5kb/O3C3D3_metabat_w_DDIG_2.5k/g'  sox_ko_list_KOfamScan_seqs_bins_list_uniq.txt

sed -i -e 's/M3C4D4_metabat_w_DDIG_2-5kb/M3C4D4_metabat_w_DDIG_2.5kb/g'  sox_ko_list_KOfamScan_seqs_bins_list_uniq.txt

sed -i -e 's/O3C3D4_metabat_w_DDIG_2-5kb/O3C3D4_metabat_w_DDIG_2.5kb/g'  sox_ko_list_KOfamScan_seqs_bins_list_uniq.txt

grep -w -f sox_ko_list_KOfamScan_seqs_bins_list_uniq.txt ../gtdb_and_checkm_for_dram.txt >sox_ko_bins_gtdbtk_dram2.txt

##names in the  gtdb_and_checkm_for_dram.txt ！= in wetlands_db_contigs_to_bins.tsv
#M3C4D4_metabat_w_DDIG_2.5kb #w kb ==M3C4D4_metabat_w_DDIG_2-5kb
#O3C3D3_metabat_w_DDIG_2.5k.150 #w/o kb == O3C3D3_metabat_w_DDIG_2-5kb
#O3C3D4_metabat_w_DDIG_2.5kb #w kb  ==O3C3D4_metabat_w_DDIG_2-5kb

wc -l sox_ko_bins_gtdbtk_dram2.txt
#492

## tigrfam based
for file in  TIGR04484 TIGR04485 TIGR04486 TIGR04555 TIGR04488 TIGR04490
do
cp  ../sulfur_cycling_tigrfam/*_"${file}"_domtblout_filter.txt ./
done

#
soxY	PF13501
soxZ	PF08770

cp ../sulfur_cycling_pfam/soxZ_PF08770_domtblout_filter.txt ./
cp ../sulfur_cycling_pfam/soxY_PF13501_domtblout_filter.txt ./
cat *TIGR* soxZ_PF08770_domtblout_filter.txt soxY_PF13501_domtblout_filter.txt  >Tigr_pfam_soxYZ_seqs.txt

cat Tigr_pfam_soxYZ_seqs.txt|cut -f1 -d$'\t' >Tigr_pfam_soxYZ_seqs_f1.txt
sed -i -e 's/_[0-9]*$//1' Tigr_pfam_soxYZ_seqs_f1.txt
cat Tigr_pfam_soxYZ_seqs_f1.txt |sort|uniq >Tigr_pfam_soxYZ_seqs_f1_uniq.txt

grep -w -f Tigr_pfam_soxYZ_seqs_f1_uniq.txt ../wetlands_db_contigs_to_bins.tsv > Tigr_pfam_soxYZ_seqs_bins_list.txt

cat Tigr_pfam_soxYZ_seqs_bins_list.txt|cut -f2 -d$'\t' |sort|uniq > Tigr_pfam_soxYZ_seqs_bins_uniq.txt
#
sed -i -e 's/\.relabeled//1' Tigr_pfam_soxYZ_seqs_bins_uniq.txt
#
sed -i -e 's/\.fa//1' Tigr_pfam_soxYZ_seqs_bins_uniq.txt
#
##fix naming issue in the wetlands_db_contigs_to_bins.tsv
sed -i -e 's/O3C3D3_metabat_w_DDIG_2-5kb/O3C3D3_metabat_w_DDIG_2.5k/g'  Tigr_pfam_soxYZ_seqs_bins_uniq.txt

sed -i -e 's/M3C4D4_metabat_w_DDIG_2-5kb/M3C4D4_metabat_w_DDIG_2.5kb/g'  Tigr_pfam_soxYZ_seqs_bins_uniq.txt

sed -i -e 's/O3C3D4_metabat_w_DDIG_2-5kb/O3C3D4_metabat_w_DDIG_2.5kb/g'  Tigr_pfam_soxYZ_seqs_bins_uniq.txt #552


grep -w -f Tigr_pfam_soxYZ_seqs_bins_uniq.txt ../gtdb_and_checkm_for_dram.txt >Tigr_pfam_soxYZ_gtdbtk_dram2.txt #552 genomes

#
cat Tigr_pfam_soxYZ_seqs_bins_uniq.txt sox_ko_list_KOfamScan_seqs_bins_list_uniq.txt |sort |uniq >kofamscan_tigr_pfam_bin_SOX_uniq.txt  #559

#
grep -w -f kofamscan_tigr_pfam_bin_SOX_uniq.txt ../gtdb_and_checkm_for_dram.txt >SOX_bins_gtdbtk_dram2.txt #559 genomes

#w 559 genomes with SOX system
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

# other issues
```
##names in the  gtdb_and_checkm_for_dram.txt ！= in wetlands_db_contigs_to_bins.tsv
#M3C4D4_metabat_w_DDIG_2.5kb #w kb ==M3C4D4_metabat_w_DDIG_2-5kb
#O3C3D3_metabat_w_DDIG_2.5k.150 #w/o kb == O3C3D3_metabat_w_DDIG_2-5kb
#O3C3D4_metabat_w_DDIG_2.5kb #w kb  ==O3C3D4_metabat_w_DDIG_2-5kb

#wetlands_db_contigs_to_bins.tsv
#/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins
#changed by Adrienne to 2-5kb style
```

##summary of all data into table
```
/Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/Data_analysis_sulfur_cycling/All_search_hits
cat *filter.txt > TIGR_PFAM_hit_filtered.txt

cat TIGR_PFAM_hit_filtered.txt|cut -f1,4 -d$'\t' >  TIGR_PFAM_hit_filtered_hits.txt

cat TIGR_PFAM_hit_filtered_hits.txt K00392_kofamscan_hits_f12.txt sulfur_cycling_kofamscan.txt >TIGR_PFAM_Kofamscan.txt
# 20892

cat TIGR_PFAM_Kofamscan.txt| cut -f1 -d$'\t' |sort|uniq >TIGR_PFAM_Kofamscan_uniq_genes.txt
#12604
#these are genes, need contigs
sed -e 's/_[0-9]*$//1' TIGR_PFAM_Kofamscan_uniq_genes.txt >TIGR_PFAM_Kofamscan_contigs.txt

cat TIGR_PFAM_Kofamscan_contigs.txt |sort|uniq >TIGR_PFAM_Kofamscan_uniq_contigs.txt
#9702

grep -w -f TIGR_PFAM_Kofamscan_uniq_contigs.txt  wetlands_db_contigs_to_bins.tsv >sulfur_cycling_related_bins_contigs.txt
#9702

#get the bin list
sulfur_cycling_related_bins_contigs.txt

#
cat sulfur_cycling_related_bins_contigs.txt|cut -f2 -d$'\t' |sort|uniq > sulfur_cycling_related_bins_uniq.txt
#2533

sed -i -e 's/\.relabeled//1' sulfur_cycling_related_bins_uniq.txt
#
sed -i -e 's/\.fa//1' sulfur_cycling_related_bins_uniq.txt
#
##fix naming issue in the wetlands_db_contigs_to_bins.tsv
sed -i -e 's/O3C3D3_metabat_w_DDIG_2-5kb/O3C3D3_metabat_w_DDIG_2.5k/g'  sulfur_cycling_related_bins_uniq.txt

sed -i -e 's/M3C4D4_metabat_w_DDIG_2-5kb/M3C4D4_metabat_w_DDIG_2.5kb/g'  sulfur_cycling_related_bins_uniq.txt

sed -i -e 's/O3C3D4_metabat_w_DDIG_2-5kb/O3C3D4_metabat_w_DDIG_2.5kb/g'  sulfur_cycling_related_bins_uniq.txt
#2533

grep -w -f sulfur_cycling_related_bins_uniq.txt gtdb_and_checkm_for_dram.txt >sulfur_cycling_related_bins_gtdbtk_dram2.txt
#2533
#
```

#
```
#merge bins, gtdbtk, contigs, functional genes
awk -F '\t' '{print $0"\t"$1}' TIGR_PFAM_Kofamscan.txt>TIGR_PFAM_Kofamscan_uniq_genes_contigs.txt

sed -i -e 's/_[0-9]*$//1'  TIGR_PFAM_Kofamscan_uniq_genes_contigs.txt

#fix
sed -e 's/\.relabeled//1' sulfur_cycling_related_bins_contigs.txt >sulfur_cycling_related_bins_contigs_fix.txt
sed -i -e 's/\.fa$//1' sulfur_cycling_related_bins_contigs_fix.txt
##fix naming issue to match the wetlands_db_contigs_to_bins.tsv
sed -i -e 's/O3C3D3_metabat_w_DDIG_2-5kb/O3C3D3_metabat_w_DDIG_2.5k/g'  sulfur_cycling_related_bins_contigs_fix.txt
sed -i -e 's/M3C4D4_metabat_w_DDIG_2-5kb/M3C4D4_metabat_w_DDIG_2.5kb/g' sulfur_cycling_related_bins_contigs_fix.txt
sed -i -e 's/O3C3D4_metabat_w_DDIG_2-5kb/O3C3D4_metabat_w_DDIG_2.5kb/g'  sulfur_cycling_related_bins_contigs_fix.txt

#merge them
TIGR_PFAM_Kofamscan_uniq_genes_contigs.txt #gene-name-contigs

sulfur_cycling_related_bins_gtdbtk_dram2.txt #bins info

sulfur_cycling_related_bins_contigs_fix.txt #contigs, bins

#fix contigs name TIGR_PFAM_Kofamscan_uniq_genes_contigs.txt
sed -e 's/O3C3D3_metabat_w_DDIG_2-5kb/O3C3D3_metabat_w_DDIG_2.5k/2'  TIGR_PFAM_Kofamscan_uniq_genes_contigs.txt>TIGR_PFAM_Kofamscan_uniq_genes_contigs_fix.txt
sed -i -e 's/M3C4D4_metabat_w_DDIG_2-5kb/M3C4D4_metabat_w_DDIG_2.5kb/2' TIGR_PFAM_Kofamscan_uniq_genes_contigs_fix.txt
sed -i -e 's/O3C3D4_metabat_w_DDIG_2-5kb/O3C3D4_metabat_w_DDIG_2.5kb/2' TIGR_PFAM_Kofamscan_uniq_genes_contigs_fix.txt

#remove rldA, K01001, 2533, genomes left 
```

```
names of the genome, annotated proteins and bins-contigs are different

```
