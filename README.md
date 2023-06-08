# New-Variant-FIltering-Pipeline
#avinput command
./convert2annovar.pl --format vcf4 filename.vcf --outfile filename.avinput --includeinfo --withzyg
#multi anno command
./table_annovar.pl filename.avinput humandb/ --buildver hg19 --outfile filename_result_annovar --protocol
refGene,cytoBand,genomicSuperDups,exac03,esp6500siv2_all,1000g2015aug_all,avsnp150,dbnsfp42a,clinvar_20210501,gwasCatalog,dbscsnv11,gme,gnomad2
--operation g,r,r,f,f,f,f,f,f,r,f,f,f,f --nastring 0 --otherinfo
#to filter function refgenes
grep -vw
"ncRNA_exonic\|ncRNA_exonic;splicing\|ncRNA_intronic\|ncRNA_splicing\|intronic\|intergenic\|downstream\|UTR3\|UTR5\|synonymous\|upstream"
filename_result_annovar.hg19_multianno.txt > filename_result_annovar.hg19_func.refgene.txt
#to filter population frequency
###please find the column ExAC_ALL, esp6500siv2_all, 1000g2015aug_all in which these column are present
awk -F "\t" '(NR==1) || ($13 < 0.05) && ($21 < 0.05) && ($22 < 0.05)' filename_result_annovar.hg19_func.refgene.txt >
filename_result_annovar.hg19_popfreq.txt
#sift polyphen cadd filter
awk -F "\t" '$26=="D" || $35=="D" || $35=="P" || $95>15' filename_result_annovar.hg19_popfreq.txt > filename_result_annovar.hg19_sift.txt
#gene list filter
grep -wFf genelist.txt filename_result_annovar.hg19_sift.txt > filename_result_annovar.hg19_genefilter.txt
