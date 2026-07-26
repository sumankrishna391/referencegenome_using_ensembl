#Building a HISAT2 Reference Genome Index using ensembl

## Objective

To build a HISAT2-compatible reference genome index from a downloaded FASTA file before RNA-Seq read alignment.

---

## Software Required

* Ubuntu (WSL/Linux)
* HISAT2 (v2.2.1 or later)

Verify installation:

```bash
hisat2 --version
```

---

# Step 1. Download the Reference Genome

Open the Ensembl database.

```
https://www.ensembl.org
```

Navigate to

```
Ensembl
    ↓
More
    ↓
Downloads
    ↓
FTP Download
```

---

# Step 2. Select the Organism

Choose the organism of interest.

Examples

* Human
* Mouse
* Zebrafish
* Atlantic salmon

In this example,

```
Organism:
Atlantic Salmon
(Salmo salar)
```

---

# Step 3. Download the Genome FASTA File

Select

```
DNA
   ↓
FASTA
```

Download the reference genome FASTA file.

Example

```
Salmo_salar_gca021399835v1.USDA_NASsal_1.1.dna.toplevel.fa.gz
```
<img width="1252" height="817" alt="Screenshot 2026-07-22 114019" src="https://github.com/user-attachments/assets/843247af-d545-44ea-9171-5cb1aa775301" />
---

# Step 4. Read the README File

Before downloading, always read the README file provided by Ensembl.

The README explains

* Assembly version
* Chromosome naming
* File contents
* Available annotations

---

# Step 5. Extract the FASTA File

After downloading, unzip the compressed file.

```bash
gunzip Salmo_salar_gca021399835v1.USDA_NASsal_1.1.dna.toplevel.fa.gz
```

This produces

```
Salmo_salar_gca021399835v1.USDA_NASsal_1.1.dna.toplevel.fa
```

---

# Step 6. Open Ubuntu (WSL)

Navigate to the directory containing the FASTA file.

Example

```bash
cd /mnt/d
```

or

```bash
cd /mnt/c
```

depending on where the FASTA file is stored.

---

# Step 7. Build the HISAT2 Index

Run

```bash
hisat2-build \
Salmo_salar_gca021399835v1.USDA_NASsal_1.1.dna.toplevel.fa \
genome_index
```

where

* First argument = FASTA file
* Second argument = prefix for the index files

---

# Step 8. Wait for Index Construction

HISAT2 converts the FASTA reference genome into indexed binary files.

During this step, progress messages such as

```
Reading reference sizes

Joining reference sequences

Building DifferenceCoverSample

Getting block 1 of 7

Getting block 2 of 7

...

Writing index files
```

will be displayed.

Large genomes (e.g., Atlantic salmon or human) may take several hours depending on CPU and RAM.
<img width="1542" height="910" alt="image" src="https://github.com/user-attachments/assets/d3f52353-2ca6-43bc-b7c5-6eba1f645aab" />
---

# Step 9. Verify Index Files

After completion,

```bash
ls genome_index*
```

Expected output

```
genome_index.1.ht2
genome_index.2.ht2
genome_index.3.ht2
genome_index.4.ht2
genome_index.5.ht2
genome_index.6.ht2
genome_index.7.ht2
genome_index.8.ht2
```

The presence of all eight `.ht2` files confirms successful index construction.

---

# Output

The reference genome is now converted into a HISAT2-compatible index and can be used for RNA-Seq alignment.

Example alignment command:

```bash
hisat2 \
-x genome_index \
-U reads.fastq \
-S aligned.sam
```

---

## Notes

* HISAT2 requires indexed reference genomes (`.ht2` files) for alignment; raw FASTA files cannot be used directly.
* Building the index is performed only once for each reference genome.
* The generated index can be reused for multiple RNA-Seq datasets, eliminating the need to rebuild it for every analysis.
* Large genomes require substantial computational resources, and index generation time depends on genome size and available hardware.




