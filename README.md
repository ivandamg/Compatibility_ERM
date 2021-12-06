# Compatibility_ERM

1.  Trimm and quality filter with BBMap suite https://sourceforge.net/projects/bbmap/
bbduk https://jgi.doe.gov/data-and-tools/bbtools/bb-tools-user-guide/bbduk-guide/


a. open interactive session in cluster, load bbmap
        Sinteractive -c 16 -m 128G -t 02:00:00
        module load gcc bbmap/38.63
        
        mkdir 01_trimmed
        bbduk.sh in=B1_1_L4_R1_001_2avKbOqY7ITX.fastq.gz in2=B1_1_L4_R2_001_bdo0VZLZah6p.fastq.gz out=01_trimmed/B1_1_clean1.fastq.gz out2=01_trimmed/B1_1_clean2.fastq.gz ref=/work/FAC/FBM/DEE/isanders/popgen_to_var/IM/ZZ_Soft/bbmap/resources/adapters.fa ktrim=r k=23 mink=11 hdist=1 tpe tbo qtrim=r trimq=24
        
        
        
#Default parameters
mkdir 01_trimmed
module add UHTS/Quality_control/fastqc/0.11.7; module add UHTS/Quality_control/cutadapt/2.5;


a=0;for i in $(ls *_R1_*_*.fastq.gz); do echo $(echo $i | cut -d'_' -f1,2); ../../Soft/TrimGalore-0.6.0/trim_galore --paired --fastqc --gzip --output_dir ./01_trimmed/ $i $(echo $i |cut -d'_' -f1,2,3)_R2_*_*.fastq.gz;done

