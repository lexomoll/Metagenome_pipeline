Now that we have our bins, we want to see what genes we have and proteins they encode! These steps take us from processing to analyzing metagenomic data

## lets AUG (get it?)

first we want to convert our nucleic acid sequence to an amino acid sequence
we will do this using [prokka](https://github.com/tseemann/prokka)

#use prokka to make amino acid files from binsanity bins

`for f in *.fna; do prokka --force --outdir ../../prokka_binsanity --prefix $f $f; done`

`for f in *.fna; do prokka --force --outdir ../../prokka_binsanity_renamed --locustag $f --prefix $f $f; done`


#concatenate .faa files from each bin to have one compiled .faa file for each sample

`cat *.faa > binsanity_proteins.faa`


#transfer protein .faa files from server to local system

`scp usernamei@IP:server_directory/binsanity_proteins.faa local_directory/`


#look through .faa files and ensure each bin is properly named (prokka tends to assign random strings of letters to each ID


## process .faa files with ghost koala

[ghost koala](https://www.kegg.jp/ghostkoala/) is a free service that annotates and maps your .faa file into the KEGG format
it's great, but beware, sometimes it can take awhile depending how many jobs are in the queue! (luckily you can monitor this on the website)

#process with ghost koala, looking for genus_prokaryotes

koala outputs a text file (to be used in kegg decoder) and a taxonomy file

## keggggggg

I edited KEGG Decoder python file to only output pathways we're interested in (general metabolism, N-fixation, photosynthesis, iron and phosphorus transporters)

#use kegg-decoder in respective kegg directory

`python3 /mnt/c/Users/lexim/Coding/BioData/KEGGDecoder/KEGG_decoder_edited.py --input compiled_annotation_koala.txt --output compiled_heatmap_keymetabolism.list --vizoption static`

I then go on to make customized heatmaps with matplotlib, but I won't bore you with those details. but do let me know if you're interested in my graphing scripts !!
