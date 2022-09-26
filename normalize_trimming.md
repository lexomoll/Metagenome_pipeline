#unzip files

#repeat for all replicates (4 each)
gunzip P1_S69_L001_R1_001.fastq.gz  
gunzip P1_S69_L001_R2_001.fastq.gz
gunzip P2_S81_L001_R1_001.fastq.gz
gunzip P2_S81_L001_R2_001.fastq.gz
gunzip P3_S93_L001_R1_001.fastq.gz
gunzip P3_S93_L001_R2_001.fastq.gz

#concatenate files
#repeat for R1 and R2 of each sample
cat P1_S69_L001_R1_001.fastq P1_S69_L002_R1_001.fastq P1_S69_L003_R1_001.fastq P1_S69_L004_R1_001.fastq > P1_S69_R1.fastq


# normalize kmers using bbnorm
#working in conda env bbmap
bbnorm.sh in1=P1_S69_R1.fastq in2=P1_S69_R2.fastq out1=P1_norm_R1.fastq out2=P1_norm_R2.fastq target=30 min=3 kmer=21
bbnorm.sh in1=P2_S81_R1.fastq in2=P2_S81_R2.fastq out1=P2_norm_R1.fastq out2=P2_norm_R2.fastq target=30 min=3 kmer=21
bbnorm.sh in1=P3_S93_R1.fastq in2=P3_S93_R2.fastq out1=P3_norm_R1.fastq out2=P3_norm_R2.fastq target=30 min=3 kmer=21


# trim sequences for quality
#working in conda env trimm
trimmomatic PE P1_norm_R1.fastq P1_norm_R2.fastq pumice1_FP.fastq pumice1_FU.fastq pumice1_RP.fastq pumice1_RU.fastq LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
trimmomatic PE P2_norm_R1.fastq P2_norm_R2.fastq pumice2_FP.fastq pumice2_FU.fastq pumice2_RP.fastq pumice2_RU.fastq LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
trimmomatic PE P3_norm_R1.fastq P3_norm_R2.fastq pumice3_FP.fastq pumice3_FU.fastq pumice3_RP.fastq pumice3_RU.fastq LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36


#check to make sure everything looks swell
#run on all trimmed files at once
fastqc pumice*
