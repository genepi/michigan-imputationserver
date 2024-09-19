# Pipeline Overview

Our pipeline performs the following steps:

## Quality Control

*  Create chunks of 20 Mb each.
*  For each 20Mb chunk, we perform the following checks:

    **On Chunk level:**

    *   Determine the number of valid variants: A variant is considered valid if it is included in the reference panel. At least 3 variants must be included, though some reference panels have different limits (e.g., HRC requires 20 SNPs).
    *   Determine amount of variants found in the reference panel: At least 50 % of the variants must be be included in the reference panel.
    *  Determine the sample call rate: At least 50% of the variants must be called for each sample.  

    Chunk exclusion: if (#variants < 3 || overlap < 50% || sampleCallRate < 50%)

    **On Variant level:**

    *   Check alleles: Only A,C,G and T are allowed
    *   Calculate alternative allele frequency (AF): Mark all variants with an AF > 0.5.
    *   Calculate the SNP call rate
    *   Calculate the chi-square for each variant (reference panel vs. study data).
    *   Determine allele switches: Compare the reference and alternate alleles of the reference panel with the study data.
    *   Determine strand flips: After eliminating possible allele switches, flip and compare the reference and alternate alleles from the reference panel with the study data.
    *   Determine allele switches in combination with strand flips: Apply the two rules above together.

    Variant exclusion: Variants are excluded in case of: [a] invalid alleles occur (!(A,C,G,T)), [b] duplicates (DUP filter or (pos - 1 == pos)), [c] indels, [d] monomorphic sites, [e] allele mismatch between reference panel and study, [f] SNP call rate < 90%, [g] more than 100 strand or allele switches.

    **On Sample level:**

    *  A chunk is excluded if any sample within that chunk has a call rate < 50%. Only complete chunks are excluded, not individual samples (see “On Chunk Level” above).


* Perform a liftOver step if the build of the input data does not match the build of the reference panel (e.g., hg37 vs. hg38).

## Phasing

* For each chunk, execute one of the following phasing algorithms (using an overlap of 5 Mb). For example, for chr20:1-20,000,000 and reference population EUR:

**Eagle2**
````sh
./eagle \
   --vcfRef HRC.r1-1.GRCh37.chr20.shapeit3.mac5.aa.genotypes.bcf \
   --vcfTarget chunk_20_0000000001_0020000000.vcf.gz \
   --geneticMapFile genetic_map_chr20_combined_b37.txt \
   --outPrefix chunk_20_0000000001_0020000000.phased \
   --chrom 20 \
   --bpStart 1 \
   --bpEnd 25000000 \
   --allowRefAltSwap \
   --vcfOutFormat z \
   --keepMissingPloidyX \
   --numThreads ${task.cpus} 
````
!!! important  "Target-only sites for unphased data are excluded from the final output."

    
## Imputation

* For each chunk, execute Minimac to impute the phased data (using a window of 500 kb):

````sh
    ./Minimac4 \
        --region 20:1-20000000 \
        --overlap 500000 \
        --output ${chunkfile_name}.dose.vcf.gz \
        --output-format vcf.gz \
        --format GT,DS,GP \
        --min-ratio $minimac_min_ratio \
        --all-typed-sites \
        --meta \
        --min-recom 0.00001 \
        --min-r2 0.8 \
        --sites out.info.gz \
        --empirical-output out.empiricalDose.vcf.gz \
        --threads 1 \
        --decay 0 \
        HRC.r1-1.GRCh38.chr1.shapeit3.mac5.aa.genotypes.msav \
        chunk_1_0000000001_0020000000.phased.vcf 
````

## HLA Imputation Pipeline

In addition to intergenic SNPs, HLA imputation outputs five different types of markers: (1) binary marker for classical HLA alleles; (2) binary marker for the presence/absence of a specific amino acid residue; (3) HLA intragenic SNPs, and (4) binary markers for insertion/deletions, as described in the typical output below. The goal is to minimize prior assumption on which types of variations will be causal and test all types of variations simultaneously in an unbiased fashion. However, the users are always free to restrict analyses to specific marker subsets.

!!! note
    For binary encodings, A = Absent, T = Present.


| Type   |      Format      |  Example |
|----------|-------------|------|
| Classical HLA alleles |  HLA\_[GENE]\*[ALLELE]| HLA_A\*01:02 (two-field allele) <br> HLA_A\*02 (one-field allele) |
| HLA amino acids |  AA_[GENE]\_[AMINO ACID POSITION]\_[GENOMIC POSITION]\_[EXON]\_[RESIDUE] | AA_B_97_31324201_exon3_V (amino acid position 97 in HLA-B, genomic position 31324201 (GrCh37) in exon 3, residue = V (Val) ) |
| HLA intragenic SNPs |  SNPS_[GENE]\_[GENE POSITION]\_[GENOMIC POSITION]\_[EXON/INTRON] | SNPS_C_2666_31237183_intron6 (SNP at position 2666 of the gene body, genomic position 31237183 in intron 6)|
| Insertions/deletions |  INDEL_[TYPE]\_[GENE]\_[POSITION]| INDEL_AA_C_300x301_31237792 Indel between amino acids 300 and 301 in HLA-C, at genomic position 31237792) |

<br> 
We note that our current implementation of the reference panel is limited to the G-group resolution (DNA sequences that determine the exons 2 and 3 for class I and exon 2 for class II genes), and amino acid positions outside the binding groove were taken as its best approximation. When converting G-group alleles to the two-field resolution, we first approximated G-group alleles to their corresponding allele at the four-field resolution based on the ordered allele list in the distributed IPD-IMGT/HLA database (version 3.32.0). We explicitly include exonic information in the HLA-TAPAS output.


For more information about HLA imputation and help, please visit [https://github.com/immunogenomics/HLA-TAPAS](https://github.com/immunogenomics/HLA-TAPAS).


## Compression and Encryption

* Merge all chunks of a chromosome into one single VCF file (.vcf.gz).
* Encrypt the data with a one-time password

## Chromosome X Pipeline

In addition to the standard QC, the following per-sample checks are performed for chrX:

* Ploidy Check: Verifies if all variants in the non-PAR region are either haploid or diploid.
* Mixed Genotypes Check: Verifies if the proportion of mixed genotypes (e.g., 1/.) is < 10%.

For phasing and imputation, chrX is divided into three independent chunks (PAR1, non-PAR, PAR2). These chunks are then automatically merged by the Michigan Imputation Server 2 and returned as a single complete chromosome X file. 

