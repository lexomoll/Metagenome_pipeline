### want to understand the genetic potential of a microbial community?

this repository will walk you through how I process <b>metagenomic data</b>

these scripts were generated for my master's project studying the microbial ecology of pumice rafts in the Pacific Ocean (pub coming soon!!), but can generally be used for any metagenomic dataset

the pipeline is organized as follows:

### [normalize and trimm](normalize_trimming.md)

here I normalize kmers and trim sequences for quality

### [bin into mags](binning.md)

this walks you through contig creation, mapping reads, binning with Metabat and with Binsanity, and checking your work with checkm

spoilers: I don't end up continuing with my Metabat bins, but I wanted to share anyways in case anyone is interested :)

comparing the MAGs generated from each binning tool, there were pros and cons for each. but we ultimately decided to go with binsanity MAGs because we were able to put together a much higher # of total MAGs that had higher completeness and less contamination

### [annotate](annotation.md)

using the newly created MAGs, we can use KEGG to annotate and assess the genetic potential of our metagenomes

thanks for following along !!

**feel free to reach out to me with any questions on [mastodon](https://ecoevo.social/@permallica) or on the [bird site](https://twitter.com/permallica) or by email at lmollica@uoguelph.ca**
