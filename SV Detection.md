```
# Align ONT reads to the reference genome using NGMLR
ngmlr -t 10 -r ${ref_genome} -q ${ont_file} -o ${sample}/test.sam -x ont

# Sort the SAM file and convert it to BAM format using Samtools
samtools sort -@ 10 -O BAM test.sam -o test.bam

# Perform structural variant calling using SVIM
svim alignment my_sample my_alignments.bam my_genome.fa

# Detect structural variants using Sniffles
sniffles -m ${bam_file} --threads 10 -v sniff.vcf.gz

# Perform structural variant calling using SVIM again
svim alignment ${sample} ${bam_file} ${ref_genome}

# Align two genomes using Nucmer
nucmer --maxmatch -l 100 -c 500 -p OUT ${ref_genome} ${que_genome}

# Analyze structural variants using Assemblytics
Assemblytics OUT.delta assembly_sv 10000 50 10000

# Merge structural variant calls using SURVIVOR
SURVIVOR merge sample_files 1000 2 1 1 0 50 sample_merged.vcf
```