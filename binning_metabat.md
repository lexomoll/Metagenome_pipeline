Here, I create contigs and try out binning with Metabat

## Create contigs using SPAdes

Check SPAdes out [here](https://github.com/ablab/spades)

This tool is great, but it takes forever to run (or at least, it did for me). I recommend using **screen** to keep your code running while you sleep :)

I kept running into this error: "you cannot specify any data types except a single paired-end library in metaSPAdes mode!"
Remedied this by running code without `--meta` option
	
`python3 /home/lexi/miniconda3/envs/spades/bin/spades.py --pe1-1 ../pumice1/pumice1_FP.fastq --pe1-2 ../pumice1/pumice1_RP.fastq --pe2-1 ../pumice2/pumice2_FP.fastq --pe2-2 ../pumice2/pumice2_RP.fastq --pe3-1 ../pumice3/pumice3_FP.fastq --pe3-2 ../pumice3/pumice3_RP.fastq -o ./spades_compiled`


## time to map our reads

BBTools has lots of different functions, and there's a super handy guide [here](https://jgi.doe.gov/data-and-tools/software-tools/bbtools/bb-tools-user-guide/bbmap-guide/)

#map reads back to assembly using bbmap

`bbmap.sh in1=/home/lexi/Pumice/pumice1/pumice1_FP.fastq in2=/home/lexi/Pumice/pumice1/pumice1_RP.fastq ref=../spades_compiled/contigs.fasta out=p1_compiled_mapped.sam covstats=covstats.txt basecov=basecov.txt bincov=bincov.txt`


## BAM !

Now we have to convert file types and do some file maintenance to get them in the right format for binning

[tool reference](https://github.com/samtools/samtools)

#convert .sam to .bam with SAMtools, in samtools conda env

`samtools view -bS p1_compiled_mapped.sam > p1_compiled_mapped.bam`


#kept getting error while binning that an MD tag was missing
#fixed this using samtools calmd function to properly format

`samtools calmd -u p1_compiled_mapped.bam /home/lexi/Pumice/compiled_contigs/spades_compiled/contigs.fasta > p1_compiled_mapped_nm.bam`


#sort and index bam files using samtools

`samtools sort p1_compiled_mapped_nm.bam > p1_compiled_mapped_sort.bam`
`samtools index p1_compiled_mapped_sort.bam`


## cowabunga it's binnin time

[Metabat](https://bitbucket.org/berkeleylab/metabat/src/master/) tends to be biased towards prokaryotes, but we'll try it anyways

#bin with Metabat

`/home/lexi/miniconda3/envs/metabat/bin/runMetaBat.sh --unbinned -v -m 1500 ./spades_compiled/contigs.fasta ./p1_compiled/p1_compiled_mapped_sort.bam ./p2_compiled/p2_compiled_mapped_sort.bam ./p3_compiled/p3_compiled_mapped_sort.bam > compiled_metabat_log.txt`


## lets check our work

I'm a huuuuge fan of [checkm](https://github.com/Ecogenomics/CheckM) because it provides such a nice visual output of your bins

#use checkm to pull out marker genes

`checkm lineage_wf -x fa compiled_metabat_bins/ ./compiled_markers`


#output qa table to a text file for easy reading
#done in markers directory

`checkm qa ./lineage.ms . -f compiled_metabins.txt`


#output marker genes to fasta file

`checkm qa -o 9 ./lineage.ms . -f markergenes_compiled_.fasta`


#break up marker fasta file into files for individual bins (do this in order to BLAST marker genes)
#use grep to do this, print matches and the following line to new files
#do this for all bins

`grep -A 1 ">bin.1 " markergenes_compiled.fasta > bin1_markers_compiled.fasta`
