# Creating the GTF reference
The initial GTF was downloaded from Ensembl, version 82.

`curl ftp://ftp.ensembl.org/pub/release-83/gtf/homo_sapiens/Homo_sapiens.GRCh38.82.gtf.gz > Homo_sapiens.GRCh38.82.gtf.gz`

`gunzip Homo_sapiens.GRCh38.82.gtf.gz`

Then the file was split into individual files, each representing one gene.

`awk '{gsub(/"|;/, "", $10); print >> ($10".gtf")}' Homo_sapiens.GRCh38.82.gtf`

The file `Homo_sapiens.GRCh38.82.gtf` was deleted.

`rm Homo_sapiens.GRCh38.82.gtf`

Note, that this strips the quotes and semicolon from the gene_id entry in each
gene-centered GTF file:

**gene_id ENSG00000142733**

instead of the correct

**gene_id "ENSG00000142733";**

Therefore, `sed` was used to correct for this.

`find . -type f -name "*.gtf" | xargs sed -i "" 's/\(ENSG[0-9]\{11\}\)/"\1";/'`

Some notes on the commands:


1. On Mac OS X sed inplace replacement has to provide an argument.
If no backup is desired, then `sed -i ""`

2. On Linux there should be no space between the "-i" flag and the backup
file extenstion, i.e. "-i.bak" instead of "-i .bak". However, if no
backup is desired, sed -i works.

3. `sed -i 's/\(ENSG[0-9]\{11\}\)/"\1";/' *.gtf` produces and error:
**Argument list too long** with that many files in a directory.
