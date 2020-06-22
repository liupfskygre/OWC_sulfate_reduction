## using KEGG decoder to profile the MAGs involved in sulfur cycling

```
https://github.com/bjtully/BioData/tree/master/KEGGDecoder

KEGG-decoder --input (-i) <KOALA_OUTPUT.txt> --output (-o) <FUNCTION_OUT.list> --vizoption (-v) <static/interactive/tanglegram>


```

#convert DRAM OUT compatable to KEGG-decoder
```


```
#BSN033_list.txt
```
/home/projects/Wetlands/All_genomes/OWC_MAGs_dRep_19Sept19/OWC_MAGs_19Sept19_dRep_/relabeled_dereplicated_genomes/relabeled_bins
cd /home/projects/Wetlands/sulfur_cycling_analysis


# the DRAM is run on each bins, not all
# each bins get an annotation.tsv file

#get the annotation.tsv for all genomes 
for file in *_DRAMOUT
do 
cat ${file}/annotations.tsv >> all_3211_annotations.tsv
printf "\n" >>all_3211_annotations.tsv
done

```
