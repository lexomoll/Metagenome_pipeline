this repository will walk you through how I process metagenomic data
the code here is specific to my master's project ( pub coming soon !! )

the pipeline is organized as follows:

### [normalize_trimming](https://github.com/lexomoll/pumice_metagenome/blob/main/normalize_trimming.md)

here I normalize kmers and trim sequences for quality

### [binning](https://github.com/lexomoll/pumice_metagenome/blob/main/binning_metabat.md)

this walks you through contig creation, mapping reads, binning with Metabat and with Binsanity, and checking your work with checkm

spoilers: I don't end up continuing with my Metabat bins, but I wanted to share anyways in case anyone is interested :)

comparing the MAGs generated from each binning tool, there were pros and cons for each. but we ultimately decided to go with binsanity MAGs because we were able to put together a much higher # of total MAGs that had higher completeness and less contamination

### [annotation](https://github.com/lexomoll/pumice_metagenome/blob/main/annotation.md)

using the newly created MAGs, we can use KEGG to annotate and assess the genetic potential of our metagenomes

thanks for following along !!

**feel free to reach out to me with any questions on [twitter](https://twitter.com/leximollica) or by email at lmollica@uoguelph.ca**
