# Variant Calling Analysis in *Saccharomyces cerevisiae*

## Project Overview

This repository documents a complete variant calling and annotation workflow for two evolved yeast strains (IMW004 and IMW005) derived from a JEN1-deletion background of *Saccharomyces cerevisiae*. The project is based on whole genome sequencing (WGS) data and aims to identify mutations potentially responsible for the reacquisition of lactate metabolism.

This analysis was conducted as part of the **Bioinformatics II** course taught by **Dra. C. Daniela Robles Espinoza** at LIIGH, UNAM.

> The biological problem and experimental setup were adapted from [MiModD Variant Calling Tutorial – Example 2](https://mimodd.readthedocs.io/en/latest/tutorial_example2.html).

## Objectives

* Identify **strain-specific mutations** in IMW004 and IMW005 by comparison to their parental strain.
* Retain **high-confidence variants** using filtering criteria: `DP > 30` and `QUAL > 50`.
* Functionally annotate variants using **Ensembl VEP**.
* Detect **genes recurrently mutated** in both evolved strains with potential biological relevance.
* Retrieve additional **gene-level information** using the **Ensembl REST API**.

## Methodology

1. **Preprocessing & Mapping**
   Sequence quality was assessed using `samtools stats` and graphical outputs. Reads were aligned to the *S288C* reference genome.

2. **Variant Calling**
   Variants were called with `bcftools mpileup` and `bcftools call`, using haploid ploidy (`--ploidy 1`).

3. **Private Variant Identification**
   Unique variants were identified using `bcftools isec -C`, comparing evolved strains to the parental background.

4. **Variant Filtering**
   Variants were filtered using:

   ```
   bcftools view -i 'DP>30 && QUAL>50'
   ```

5. **Functional Annotation with VEP**
   Annotated using the yeast cache and the following command:

   ```
   vep --cache --dir_cache /home/drobles/.vep/ \
       -i SRR445716_unique.flt.vcf \
       -o SRR445716_unique.flt.vep.vcf \
       --vcf --species "saccharomyces_cerevisiae"
   ```

6. **Gene Analysis**
   Python scripts extracted mutations affecting protein function (`missense`, `stop_gained`, `frameshift`, `splice_acceptor`, `splice_donor`) and identified shared mutated genes.

7. **Gene Metadata Retrieval**
   The Ensembl REST API was used to obtain structured information on candidate genes.

## Results

* The gene **ADY2 (YCR010C)** was found mutated in both evolved strains, consistent with the findings from Kok et al. (2012).
* The mutations in **ADY2** are different in each strain, suggesting convergent evolution of function.
* An additional gene, **YDR534C (FIT1)**, also showed potentially functional mutations in both strains.

## Files in this Repository

| File                           | Description                                    |
| ------------------------------ | ---------------------------------------------- |
| `SRR445716_unique.flt.vep.vcf` | Annotated VCF for IMW004                       |
| `SRR445717_unique.flt.vep.vcf` | Annotated VCF for IMW005                       |
| `mutations_combined.tsv`       | Filtered functional variants from both strains |
| `gene_info_from_ensembl.tsv`   | Gene-level metadata retrieved from Ensembl     |
| `Variant_Calling_Sam.html`     | Final HTML report rendered from RMarkdown      |

## Platforms & Tools Used

* Cluster (dna.lavis.unam.mx)
* `samtools`, `bcftools`, `vep` (Ensembl VEP release 99.2)
* `R`
* Python 3, Ensembl REST API

## References

1. Kok J, et al. (2012). Laboratory evolution of new lactate transporter genes in a jen1Δ mutant of *Saccharomyces cerevisiae* and their identification as *ADY2* alleles by whole-genome resequencing and transcriptome analysis. *FEMS Yeast Research*, 12(4): 359–374. [PubMed link](https://pubmed.ncbi.nlm.nih.gov/22257278/)
2. MiModD Tutorial – Example 2. [https://mimodd.readthedocs.io/en/latest/tutorial\_example2.html](https://mimodd.readthedocs.io/en/latest/tutorial_example2.html)

## Author

**Samantha M. Pacheco Gómez**
*Bioinformatics II – ENES Juriquilla UNAM, May 2025*
