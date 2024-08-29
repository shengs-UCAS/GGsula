```
bcftools merge ${vcf10} sula.gatk.vcf.gz -Oz -o all_gatk.bcf.vcf.gz

gatk SelectVariants -V all_bcf_gatk.vcf -O all_bcf_gatk_snp.vcf --select-type-to-include SNP

gatk VariantFiltration \
-V sula.gatk.snp.aut.vcf.gz \
        -filter "QD < 2.0" --filter-name "QD" \
        -filter "SOR > 3.0" --filter-name "SOR" \
        -filter "QUAL < 30.0" --filter-name "QUAL" \
        -filter "FS > 60.0" --filter-name "FS" \
        -filter "MQ < 40.0" --filter-name "MQ" \
        -filter "MQRankSum < -12.5" --filter-name "MQRankSum" \
        -filter-name "ReadPosRankSum_filter" -filter "ReadPosRankSum < -8.0" \
-O  sula.gatk.snp.aut.filt.vcf.gz

vcftools --gzvcf ${vcf.gz} --max-missing 0.8 --maf 0.05 --recode --recode-INFO-all --out 30_missft

python vcf2phylip.py --input g12.sula.merge.vcf.gz --outgroup RJFj650 -f

FastTree -fastest g12.sula.merge.min4.fasta > tree
```