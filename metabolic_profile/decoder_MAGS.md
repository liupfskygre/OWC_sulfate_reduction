## using KEGG decoder to profile the MAGs involved in sulfur cycling

#check this for old reference
https://github.com/liupfskygre/OWC_sulfate_reduction/blob/master/metaG/DRAM_liquor.md

```
https://github.com/bjtully/BioData/tree/master/KEGGDecoder

KEGG-decoder --input (-i) <KOALA_OUTPUT.txt> --output (-o) <FUNCTION_OUT.list> --vizoption (-v) <static/interactive/tanglegram>


```

#convert DRAM OUT compatable to KEGG-decoder
```

#covert to two columns 
awk '{for(a=2;a<=NF;a++){printf "%s %s%c\n",$1,$a,a==NF ? "" : ";"}}' File.txt >File_2_Columns.txt
awk '{for(a=2;a<=NF;){printf"%s%c\n",$1" "$a,a++-NF?";":""}}' File.txt >File_2_Columns.txt
'BEGIN{ORS=RS=";\n"} {for(a=2; a<=NF;) print $1,$(a++)}'

```
#BSN033_list.txt
```
cd /home/projects/Wetlands/sulfur_cycling_analysis


/home/projects/Wetlands/sulfur_cycling_analysis/MAGs_metabolic_profiling/Desulfobacterota_BSN033

awk -F '\t' '$8!=""' Desulfobacterota_BSN033_annotation.tsv  >Desulfobacterota_BSN033_annotation_w_KO.tsv

cat Desulfobacterota_BSN033_annotation_w_KO.tsv|cut -f2,8 -d$'\t' >Desulfobacterota_BSN033_annotation_w_KO28.tsv


head Desulfobacterota_BSN033_annotation_w_KO28.tsv
sed -i -e 's/\,/\t/g' Desulfobacterota_BSN033_annotation_w_KO28.tsv
awk -F '\t' 'BEGIN{ORS=RS="\n"} {for(a=2; a<=NF;) print $1"\t"$(a++)}' Desulfobacterota_BSN033_annotation_w_KO28.tsv >Desulfobacterota_BSN033_annotation_w_KO28_fix.tsv

#sed -i -e 1b -e '/fasta/d' Desulfobacterota_BSN033_annotation_w_KO28_fix.tsv
sed -i -e '/fasta/d' Desulfobacterota_BSN033_annotation_w_KO28_fix.tsv

sed -i -e 's/_/;/g' Desulfobacterota_BSN033_annotation_w_KO28_fix.tsv

awk '{print $1"_"NR"\t"$2}' Desulfobacterota_BSN033_annotation_w_KO28_fix.tsv > Desulfobacterota_BSN033_annotation_w_KO28_fix_wN.tsv

KEGG-decoder --input Desulfobacterota_BSN033_annotation_w_KO28_fix_wN.tsv --output NSN033_FUNCTION_OUT.list --vizoption interactive



```
