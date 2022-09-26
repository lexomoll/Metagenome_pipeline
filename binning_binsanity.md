In my humble opinion, Metabat isn't the best binning tool. It's biased towards prokaryotes, which I don't love.
For my project, I actually ended up using a binning tool called Binsanity

There's some really great tutorials over at the Binsanity [github](https://github.com/edgraham/BinSanity) that will walk you through how to use their tools in a way that's best for your data. But here's what I did :)

#first, move all three mapping files to their own directory called compiled_maps

## generate coverage profile 

`Binsanity-profile -i ../spades_compiled/contigs.fasta -s ../compiled_maps -c compiled_binsanity_coverage`

this will subsequently be used as the input


## bin bin bin
#using transformed file from the previous step

`Binsanity-wf -f ../spades_compiled -l contigs.fasta -c compiled_binsanity_coverage.cov.x100.lognorm -o compiled_BinsanityWF`


#transfer checkm output from server to local to view the results more easily

`scp username@IP address:server_directories/BinSanityWf_checkm_lineagewf-results.txt local_directories/binsanity/`
