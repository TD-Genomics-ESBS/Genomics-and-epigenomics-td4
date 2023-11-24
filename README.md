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
source .bashrc
conda activate /home/merlat/miniconda3/envs/trnascan
```

To see options: 
```bash	
tRNAscan-SE -help
```

```bash	
tRNAscan-SE -B -o sulcia.trnascan sulcia.tfa
```

> :question: How many genes are found? Are all aa represented? Is there redundancy (several tRNA genes with the same anti-codon)?

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

### Localization of protein genes

