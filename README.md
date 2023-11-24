# Genomics and Epigenomics TD4

## Annotation of a complete prokaryotic genome

*Sulcia muelleri* is an intracellular endosymbiotic bacterium. It lives inside an insect with another symbiotic bacterium. *S. muelleri* genome is extremely reduced with a size of 245,530 bp.

### Server connection

1. Open a terminal window to connect to the LBGI server. Your login is your last name in lower case. The command is:
```bash
ssh -Y yourlogin@tp.lbgi.fr
```

The password is the same for everyone: *TPgenomics2022*

Remark: the password is not displayed when you type.
1. Copy the genome sequence in your directory. The sequence is available in fasta format at: `/data/TDannotation/sulcia.tfa`.

```bash	
mkdir TDannotation
cd TDannotation
cp /data/TDannotation/sulcia.tfa .
```

### Localization of non coding RNA genes

#### Localization of tRNA genes

You will use the tRNAscan-SE program. To use it, activate a conda environment:
```bash
/bin/miniconda3/bin/conda init bash 
source ~/.bashrc
conda activate /home/merlat/.conda/envs/trnascan
```

To see options: 
```bash	
tRNAscan-SE -help
```

```bash	
tRNAscan-SE -B -o sulcia.trnascan sulcia.tfa
```

> :question: How many genes are found? Are all aa represented? Is there redundancy (several tRNA genes with the same anti-codon)?

Use the commands more to print the file:
```bash
more sulcia.trnascan
```

#### Search for rRNA genes by similarity

We want to find the approximate positions of rRNA genes on the *S. muelleri* genome by similarity using BLASTN and rRNA sequences from *Sulcia* or closely related species. Thus, we have to construct a BLAST database from the Sulcia genomic sequence.

#####  Creation of the S. muelleri genome BLAST database

We will use the makeblastdb command. To see available options:
```bash
makeblastdb –help
```

```bash
makeblastdb -in sulcia.tfa -dbtype nucl -out SulciaDb
```

#####  Similarity searches using BLASTN

Sequence of rRNA genes are available (in fasta format) in `/data/TDannotation`:
- 5S of Sulcia muelleri
- 23S of Myroides odoratus (m62807) 
- 16S of Coleomegilla maculata endosymbiont (y13889)

```bash
cp /data/TDannotation/5S_Sulcia.tfa .
cp /data/TDannotation/23S_Myroides.tfa .
cp /data/TDannotation/16S_Coleo.tfa .
```

For each rRNA, you will perform a BLASTN search using the blastn command.
To see the list of parameters:
```bash
blastn -help
```

```bash
blastn -db SulciaDb -query 5S_Sulcia.tfa -out 5S.blastn
blastn -db SulciaDb -query 16S_Coleo.tfa -out 16S.blastn
blastn -db SulciaDb -query 23S_Myroides.tfa -out 23S.blastn
```

Use more to display them.

``` bash
more 5S.blastn
more 16S.blastn
more 23S.blastn
```

### Localization of protein genes

#### Abinitio prediction using GLIMMER

We will use glimmer:
``` bash
run-glimmer2 -h
```

``` bash
run-glimmer2 sulcia.tfa
```

4 files will be created by glimmer:
- *tmp.coord* and *tmp.train* contain coordinates and sequences of reference genes from the training set
- *tmp.model* contains the model built by glimmer (binary format, do not open it!)
- the last file *g2.coord* contains genes predicted by glimmer.
  

You can display *tmp.coord* and *tmp.train*.

```
more tmp.coord
more tmp.train
```


> :question: How many genes have been used to build the model (file tmp.coord)?

> :question:  What do you think about the number of predicted genes? The coding density?

> :question: The GC content is 22%. How this percentage may influence the quality of glimmer predictions?

#### Correction of glimmer predictions by similarity searches

##### Over-predictions and error in initiation codon definition

Some genes predicted by glimmer exhibit important overlaps. Some examples of these genes have been translated (protein_sequences.txt available on moodle). Consider genomic context and similarity searches (NCBI BLAST, refseq_protein database, excluding Candidatus Sulcia) to validate or invalidate the existence or propose a correction for the following overlapping genes:
SMU001/SMU002, SMU023/SMU024.
1 1478 1308 [-3 L= 171] [ShadowedBy #2] 2 712 3321 [+1 L=2610] [Contains #1]
23 27794 27372 [-3 L= 423]
24 27770 28498 [+2 L= 729] [DelayedBy #23 L=33]

##### Under-predictions

Check if the following long intergenic regions (available on moodle) contain a protein coding gene (using NCBI BLAST pages, refseq_protein database and excluding Candidatus Sulcia): SMU043/SMU044, SMU204/SMU206, SMU183/SMU184
1. Functional annotation
Assign a function of the following protein (available on moodle) sequences using NCBI BLAST (refseq_protein database and excluding Candidatus Sulcia):
SMU138 et SMU140

> :question: What additional research would be needed in this case?