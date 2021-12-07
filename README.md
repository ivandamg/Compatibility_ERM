# Compatibility_ERM

1.  Trimm and quality filter with BBMap suite https://sourceforge.net/projects/bbmap/
bbduk https://jgi.doe.gov/data-and-tools/bbtools/bb-tools-user-guide/bbduk-guide/


a. open interactive session in cluster, load bbmap
        Sinteractive -c 16 -m 128G -t 02:00:00
        module load gcc bbmap/38.63
        
        mkdir 01_trimmed
        bbduk.sh in=B1_1_L4_R1_001_2avKbOqY7ITX.fastq.gz in2=B1_1_L4_R2_001_bdo0VZLZah6p.fastq.gz out=01_trimmed/B1_1_clean1.fastq.gz out2=01_trimmed/B1_1_clean2.fastq.gz ref=/work/FAC/FBM/DEE/isanders/popgen_to_var/IM/ZZ_Soft/bbmap/resources/adapters.fa ktrim=r k=23 mink=11 hdist=1 tpe tbo qtrim=r trimq=24
        
        
        
2. Map to DAOM genome
script: 
        for FILE in *_clean1.fastq.gz; do echo ${FILE}; sbatch -o $(echo ${FILE} | cut -d'_' -f1,2)_bbmap.stdout.txt -J B$(echo ${FILE} | cut -d'_' -f1,2) -e $(echo ${FILE}| cut -d'_' -f1,2)_bbmap.stderr.txt ../0003_bbmap_curnagleB1_v2.sh ${FILE}; sleep 1; done
        
        # this is inside script
        bbmap.sh in1=${1} in2=$(echo ${1} | cut -d'_' -f1,2)_clean2.fastq.gz out=$(echo ${1} | cut -d'_' -f1,2)_mapped_DAOM197198K.sam ref=/work/FAC/FBM/DEE/isanders/popgen_to_var/IM/01_Coinoc_v2/03_MappedDAOM/R_irregularis_DAOM197198_Kameoka_v2.fna vslow k=8 maxindel=200 minratio=0.1
        
        
3. Transform bam

        samtools sort -m4G -@4 -o $(echo ${1} | cut -d'_' -f1,2)_mapped_DAOM197198K_Sorted.bam $(echo ${1} | cut -d'_' -f1,2)_mapped_DAOM197198K.sam
        samtools view -bq 30 $(echo ${1} | cut -d'_' -f1,2)_mapped_DAOM197198K_Sorted.bam > $(echo ${1} | cut -d'_' -f1,2)_mapped_DAOM197198K_Sorted_Q30.bam
                rm $(echo ${1} | cut -d'.' -f1)_mapped_DAOM197198K.sam
                rm $(echo ${1} | cut -d'.' -f1)_mapped_DAOM197198K_Sorted.bam
                
4. Map to B1 genome


5. Feature counts for DAOM

     /work/FAC/FBM/DEE/isanders/popgen_to_var/IM/ZZ_Soft/subread-2.0.3-source/bin/featureCounts -T 8 -a /work/FAC/FBM/DEE/isanders/popgen_to_var/IM/01_Coinoc_v2/03_MappedDAOM/DAOM197198_Rhiir2_v2.gff -t CDS -g ID -o Counts_REFDAOM_COL2215.txt  Unmapped_Mesculenta_COL2215_B1DAOM197198_3_mapped_DAOM197198_Sorted_Q30.bam Unmapped_Mesculenta_COL2215_B1_1_mapped_DAOM197198_Sorted_Q30.bam Unmapped_Mesculenta_COL2215_B1_2_mapped_DAOM197198_Sorted_Q30.bam Unmapped_Mesculenta_COL2215_B1_3_mapped_DAOM197198_Sorted_Q30.bam Unmapped_Mesculenta_COL2215_DAOM197198_1_mapped_DAOM197198_Sorted_Q30.bam Unmapped_Mesculenta_COL2215_DAOM197198_2_mapped_DAOM197198_Sorted_Q30.bam Unmapped_Mesculenta_COL2215_DAOM197198_3_mapped_DAOM197198_Sorted_Q30.bam Unmapped_Mesculenta_COL2215_Mock_1_mapped_DAOM197198_Sorted_Q30.bam Unmapped_Mesculenta_COL2215_Mock_2_mapped_DAOM197198_Sorted_Q30.bam -p --countReadPairs

6. Feature counts for B1
