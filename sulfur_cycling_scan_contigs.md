#use marker gene hmmprofile to search contigs profile


## update note
```
#30-June-2020, correct script and the metafile 
../hmmsearch_get_seqs_w_proteins.sh  #typo err, cutoof1 ==>cutoff1; cutoof2 ==>cutoff2

sulfur_cycling_biogeoMarker_genes_hmmsearch.txt #16-May-2020the metafile containing TIGRFAM was wrong, part of TIGRFAM != gene names during the generation of the file

needs to redo if needed
```
#redo prodigal annotation of MetaG16 @2500bp
```
cd 
#
pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_M1_C1_D1_reads_and_megahit_full_assembly/Aug_M1_C1_D1_megahit_assembly/Aug_M1_C1_D1_megahit.contigs.fa >AugM1C1D1B.fa

pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_M1_C1_D2_reads_and_megahit_full_assembly/Aug_M1_C1_D2_megahit_assembly/Aug_M1_C1_D2_megahit.contigs.fa >AugM1C1D2B.fa

pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_M1_C1_D3_reads_and_megahit_full_assembly/Aug_M1_C1_D3_megahit_assembly/Aug_M1_C1_D3_megahit.contigs.fa > AugM1C1D3B.fa

pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_M1_C1_D4_reads_and_megahit_full_assembly/Aug_M1_C1_D4_megahit_assembly/Aug_M1_C1_D4_megahit.contigs.fa >AugM1C1D4B.fa

pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_M1_C1_D5_reads_and_megahit_full_assembly/Aug_M1_C1_D5_megahit_assembly/Aug_M1_C1_D5_megahit.contigs.fa >AugM1C1D5B.fa

pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_M2_C1_D1_reads_and_megahit_full_assembly/Aug_M2_C1_D1_megahit_assembly/Aug_M2_C1_D1_megahit.contigs.fa >AugM2C1D1C.fa


pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_M2_C1_D5_reads_and_megahit_full_assembly/Aug_M2_C1_D5_megahit_assembly/Aug_M2_C1_D5_megahit.contigs.fa > AugM2C1D5C.fa


pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_N3_C1_D1_reads_and_megahit_full_assembly/Aug_N3_C1_D1_megahit_assembly/Aug_N3_C1_D1_megahit.contigs.fa > AugN3C1D1A.fa


pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_N3_C1_D5_reads_and_megahit_full_assembly/Aug_N3_C1_D5_megahit_assembly/Aug_N3_C1_D5_megahit.contigs.fa >AugN3C1D5A.fa


pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_OW2_C1_D1_reads_and_megahit_full_assembly/Aug_OW2_C1_D1_megahit_assembly/Aug_OW2_C1_D1_megahit.contigs.fa>AugOW2C1D1C.fa

pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_OW2_C1_D5_reads_and_megahit_full_assembly/Aug_OW2_C1_D5_megahit_assembly/Aug_OW2_C1_D5_megahit.contigs.fa>AugOW2C1D5C.fa

pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_T1_C1_D1_reads_and_megahit_full_assembly/Aug_T1_C1_D1_megahit_assembly/Aug_T1_C1_D1_megahit.contigs.fa>AugT1C1D1A.fa


pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/Aug_T1_C1_D5_reads_and_megahit_full_assembly/Aug_T1_C1_D5_megahit_assembly/Aug_T1_C1_D5_megahit.contigs.fa>AugT1C1D5A.fa


pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/May_M1_C1_D1_reads_and_megahit_full_assembly/May_M1_C1_D1_megahit_assembly/May_M1_C1_D1_megahit.contigs.fa >MayM1C1D1A.fa

pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/May_M1_C1_D3_reads_and_megahit_full_assembly/May_M1_C1_D3_megahit_assembly/May_M1_C1_D3_megahit.contigs.fa> MayM1C1D3A.fa


pullseq -m 2500 -i /home/projects/Wetlands/2018_sampling/OWC_metaG_megahit/May_M1_C1_D6_reads_and_megahit_full_assembly/May_M1_C1_D6_megahit_assembly/May_M1_C1_D6_megahit.contigs.fa >MayM1C1D6A.fa

##
for file in $(cat metaG16_list.txt)
do 
prodigal -p meta -q -i ${file}.fa -a ${file}_prodigal.faa -d ${file}_prodigal.fna
sed -i -e 's/ \+/#/g' ${file}_prodigal.faa
sed -i -e 's/ \+/#/g' ${file}_prodigal.fna
done
```

##transfer to MAC and use MATABOLIC pipline to hmmsearch biogeochemical genes
```
for file in $(cat /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/metaG16_prodigal.txt) 
do 
echo $file
#mkdir /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/${file}_contigs
#mv ${file} /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/${file}_contigs

#METBOLIC requeire sequences names no '#', otherwise error
sed -i -e 's/#/ /g' /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/${file}_contigs/$file
perl METABOLIC-G.pl -kofam-db small -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/${file}_contigs -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/${file}_contigs
done

```
perl METABOLIC-G.pl -in /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugM1C1D1B_prodigal -o /Users/pengfeiliu/A_Wrighton_lab/Wetland_project/Sulfur_Cycling_OWC_wetland/gene-level-analysis/contigs_2500_length/AugM1C1D1B_prodigal -kofam-db small

sed -i -e 's/#/ /g' AugM1C1D1B_prodigal.faa




## redo all scan with marker genes, 2020-June-30；not run
```
#all data analysis
cd /home/projects/Wetlands/sulfur_cycling_analysis

cd /home/projects/Wetlands/sulfur_cycling_analysis/hmmsearch_hits_assembly

## OWC_MetaG_contigs_fix_header_Apr2020.txt ====> all prodigal file were removed!!!

nano OWC_all_MetaG_contigs.txt



#call /home/projects/Wetlands/2018_sampling/hmmsearch_get_seqs_w_proteins.sh [sample_file1.txt] [hmmprofile_file2.txt] [threads]
screen -r 1core
../hmmsearch_get_seqs_w_proteins.sh ../OWC_all_MetaG_contigs.txt ../sulfur_cycling_biogeoMarker_genes_hmmsearch.txt 4 


all_3211_genes_DRAM	/home/projects/Wetlands/sulfur_cycling_analysis/all_3211_genes_DRAM_aa.faa	/home/projects/Wetlands/sulfur_cycling_analysis/all_3211_genes_DRAM.fna

#all prodigal files deleted???, need to re-run the prodigal step

```



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

#fix prodigal header, add file name to header
```
grep -v '#' OWC_MetaG_contigs_fix_header_Apr2020.txt > OWC_MetaG_contigs_fix_header_list.txt

screen -r reformat_prodigal
#
IFS=$'\n'
for sample in $(cat OWC_MetaG_contigs_fix_header_list.txt)
do
echo ${sample}
#get sample name
name=$(echo ${sample}| cut -f1 -d$'\t' )

#get sample prodigal.faa
protein=$(echo ${sample}| cut -f2 -d$'\t')

#get sample DNAseqs
DNAseqs=$(echo ${sample}| cut -f3 -d$'\t')

echo ${name}
sed -i -e "s/>\(.*\)/>"${name}"_\1/g" ${protein}
sed -i -e "s/>\(.*\)/>"${name}"_\1/g" ${DNAseqs}

echo ${protein}
head -1 ${protein}
echo ${DNAseqs}
head -1 ${DNAseqs}
done
unset IFS
```
#checking
```
IFS=$'\n'
for sample in $(cat file1.txt)

do
#echo ${sample}
#get sample name
name=$(echo ${sample}| cut -f1 -d$'\t' )

#get sampunset IFSle prodigal.faa
protein=$(echo ${sample}| cut -f2 -d$'\t')

#get sample DNAseqs
DNAseqs=$(echo ${sample}| cut -f3 -d$'\t')
echo ${protein}
head -1 ${protein}
echo ${DNAseqs}
head -1 ${DNAseqs}
done 
unset IFS

```

#pipeline
```
#profile metadata text: include profile name, profile GA, TC, filtering length
# file include sample name and all path to protein and nt file

#out, (1), protein reads, (2), nt reads meeting cutoff

/home/projects/Wetlands/2018_sampling/scripts/fetch_contigs_to_mcrA_seqs.sh

#call /home/projects/Wetlands/2018_sampling/hmmsearch_get_seqs_w_proteins.sh [sample_file1.txt] [hmmprofile_file2.txt] [threads]
./hmmsearch_get_seqs_w_proteins_test.sh file1.txt file2.txt 1

/home/projects/Wetlands/sulfur_cycling_analysis/hmmsearch_hits_assembly

../hmmsearch_get_seqs_w_proteins.sh ../file1.txt ../file2.txt 6 &>hmmsearch.log


##notes, need to redo since file1 TIGRfam!= gene names
```


#kofam is relative easy, will give a


## rps3 archive log
```
fix sequences name header

#keep the sample name and contigs info
#1, remove all content behind '#' in the header
for file in *.faa
do
sed -e 's/^>\(k.*_[0-9]*\)#.*$/>\1/g' $file>"${file%.*}"_f.temp
sed -i -e 's/\*$//g' "${file%.*}"_f.temp
awk -v a="${file%%.*}" '/^>/{print ">",a,$0; next}{print}' "${file%.*}"_f.temp > "${file%.*}"_f.faa
#or sed -e "s/>\(.*\)/>"$var"_\1/g" $file >"${file%.*}"_rename.fasta

sed -i -e 's/ \+//g' "${file%.*}"_f.faa
sed -i -e 's/>/__/2' "${file%.*}"_f.faa
done
#
cat *_f.faa >OWC2018_megahit_rps3_raw_aa.fasta
#7733
##for fna

for file in *.fna
do
sed -e 's/^>\(k.*_[0-9]*\)#.*$/>\1/g' $file>"${file%.*}"_f.temp2
sed -i -e 's/\*$//g' "${file%.*}"_f.temp2
awk -v a="${file%%.*}" '/^>/{print ">",a,$0; next}{print}' "${file%.*}"_f.temp2 > "${file%.*}"_f.fna
sed -i -e 's/ \+//g' "${file%.*}"_f.fna
sed -i -e 's/>/__/2' "${file%.*}"_f.fna
done
#
cat *_f.fna >OWC2018_megahit_rps3_raw_nt.fasta
#7733

```
