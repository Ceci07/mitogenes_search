# mitogenes_search

## Mitochondrial genomes metadata and sequences

Metadata for complete mitochondrial genomes of Nematoda and Platyhelminthes were retrieved from NCBI Nucleotide using the NCBI E-utilities API in JSON format and subsequently converted to TSV files.   
FASTA files were retrived from the NCBI database using "Nematoda[Organism] AND mitochondrion[Title] AND "complete genome"[Title]" and "Platyhelminthes[Organism] AND mitochondrion[Title] AND "complete genome"[Title]".  

## Mitochondrial genes prediction

```
mkdir -p mitogenomes_split

seqkit split \
    -i \
    -O mitogenomes_split \
    helminths.fasta
```

[Mitos2]() and ```refseq89m``` database were used to detect and extract mitochondrial genes

```
for f in mitogenomes_split/*.fasta; do

    sample=$(basename "$f" .fasta)

    mkdir -p "mitogenomes_split/genome/$sample"

    singularity exec mitos2.sif \
        runmitos \
        -i "$f" \
        -o "mitogenomes_split/genome/$sample" \
        -r refseq89m/ \
        -c 5 (nematodes) 9 (platyhelminthes)

done
```
