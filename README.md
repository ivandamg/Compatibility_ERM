# Compatibility_ERM

1. b. Trimm and quality filter with TrimGalore https://github.com/FelixKrueger/TrimGalore

#Default parameters
mkdir 01_trimmed
module add UHTS/Quality_control/fastqc/0.11.7; module add UHTS/Quality_control/cutadapt/2.5;
a=0;for i in $(ls *_R1_*_*.fastq.gz); do echo $(echo $i | cut -d'_' -f1,2); ../../Soft/TrimGalore-0.6.0/trim_galore --paired --fastqc --gzip --output_dir ./01_trimmed/ $i $(echo $i |cut -d'_' -f1,2,3)_R2_*_*.fastq.gz;done
