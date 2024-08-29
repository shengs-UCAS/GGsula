```
# Convert VCF file to PLINK format
vcftools --gzvcf gf4.vcf.gz --plink --out B

# Set chromosome set to 33 and recode the PLINK file with allele coding 1, 2, 3
plink --chr-set 33 --file B --recode --allele1234 --out B2

# Convert PLINK files to EIGENSTRAT format using fcgene
fcgene --map B2.map --ped B2.ped --oformat eigensoft --group-label sp.info --out B3

# Adjust SNP positions by adding 1 to the first column and save to a new file
awk '{print$1+1"\t"$2"\t"$3"\t"$4"\t"$5"\t"$6}' B3.pedsnp > B4.pedsnp

# Convert the adjusted PLINK files to EIGENSTRAT format using convertf
convertf -p par.PED.EIGENSTRAT

# Run qpDstat analysis and save the output to dstat.out
qpDstat -p parqpDstat1 > dstat.out
```