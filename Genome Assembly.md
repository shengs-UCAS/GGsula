```

ccs -j 10 {$subreads.bam} s2.hifi.fastq.gz

hifiasm --primary -o test -t 32 chr11-2M.fa.gz 2> test.log2

nextDenovo config.cfg
nextPolish polish.cfg
```

### config.cfg
```
[General]
job_type = local
job_prefix = denovo_s2
task = all # 'all', 'correct', 'assemble'
rewrite = yes
deltmp = yes
rerun = 3
parallel_jobs = 10
input_type = raw
read_type = ont
input_fofn = ./input.fofn
workdir = ./rundir

[correct_option]
read_cutoff = 1k
genome_size = 1g
pa_correction = 5
sort_options = -m 100g -t 10
minimap2_options_raw =  -t 10
correction_options = -p 10

[assemble_option]
minimap2_options_cns =  -t 10
nextgraph_options = -a 1
```

## replace ONT sequences  by Pacbio sequences
```
python3 ../bin/replaceBlock1.py chicken.diff.1coords.98.500k.0.6 ../0.assembly/nanopore.asm.fa ../data/hifi.asm.fa >replaced.fa
```
