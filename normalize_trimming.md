#unzip files

#repeat for all replicates
`gunzip P1_S69_L001_R1_001.fastq.gz`
`gunzip P1_S69_L001_R2_001.fastq.gz`
`gunzip P2_S81_L001_R1_001.fastq.gz`
`gunzip P2_S81_L001_R2_001.fastq.gz`
`gunzip P3_S93_L001_R1_001.fastq.gz`
`gunzip P3_S93_L001_R2_001.fastq.gz`

#concatenate files
#repeat for R1 and R2 of each sample
`cat P1_S69_L001_R1_001.fastq P1_S69_L002_R1_001.fastq P1_S69_L003_R1_001.fastq P1_S69_L004_R1_001.fastq > P1_S69_R1.fastq`

from here on out, I only show code for one replicate but am actually doing the same thing on all 3

## normalize kmers using bbnorm

[here](https://jgi.doe.gov/data-and-tools/software-tools/bbtools/bb-tools-user-guide/bbnorm-guide/) is a very user-friendly guide to using BBNorm and all other BBMap tools
this is where I start using conda environments, which I highly recommend! 

I'm working in conda env named bbmap - assume all tools I use were installed with conda in their own environments

`bbnorm.sh in1=P1_S69_R1.fastq in2=P1_S69_R2.fastq out1=P1_norm_R1.fastq out2=P1_norm_R2.fastq target=30 min=3 kmer=21`


## trim sequences for quality
#working in conda env trimm
`trimmomatic PE P1_norm_R1.fastq P1_norm_R2.fastq pumice1_FP.fastq pumice1_FU.fastq pumice1_RP.fastq pumice1_RU.fastq LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36`


#check to make sure everything looks swell

`fastqc pumice*`
