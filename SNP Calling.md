```
bwa mem -t 10 ${ref_genome} ${fq1_file} ${fq2_file}| samtools sort -@ 10 -m 10G > ${bwa_head}.bam
samtools rmdup ${bwa_head}.bam ${bwa_head}.rmdup.bam
samtools index ${bwa_head}.rmdup.bam ${bwa_head}.rmdup.bam.bai

###IndelRealignment###
java -Xmx${smem}g -jar GenomeAnalysisTK.jar -T RealignerTargetCreator \
-nt ${nthread} -R ${ref_genome} \
--known ${known_indel}\
-I ${mark_dup_bam} \
-o ${inter_val}

java -Xmx${smem}g -jar GenomeAnalysisTK.jar \
-T IndelRealigner \
-nt ${nthread} \
-R ${ref_genome} \
-known ${known_indel} \
-I ${mark_dup_bam} \
-targetIntervals ${inter_val} \
-o ${realin_bam}

#Base calibration
gatk BaseRecalibrator \
-I ${realin_bam} \
-R ${ref_genome} \
--known-sites ${know_var} \
-O cali.table

#Adjusting the base quality
gatk ApplyBQSR \
-I ${realin_bam} \
-R ${ref_genome} \
--bqsr-recal-file cali.table \
-O ${bqsr_bam}

# #Mutation detection, Generate gvcf file
gatk HaplotypeCaller \
-R ${ref_genome} \
-I ${bqsr_bam} \
-O ${gvcf} \
-ERC GVCF \
--output-mode EMIT_ALL_ACTIVE_SITES #Full site output, parameters that must be added#

java -jar ${gatk.jar} CombineGVCFs -R ${ref_genome} --variant ${gvcf1} --variant ${gvcf2} -O sula.gatk.g.vcf.gz
java -jar ${gatk.jar} GenotypeGVCFs -R ${ref_genome} -V sula.gatk.g.vcf.gz -O sula.gatk.vcf.gz

```
