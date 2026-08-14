# Medical Trait Catalog — Validation Requirements

**No external annotation has been performed. All rsIDs are externally unvalidated.**

This document describes the external evidence required before any medical/sensitive trait can be reported.

## Complex Disease Risk (430 traits)

| Evidence Source | Purpose |
|----------------|---------|
| dbSNP/Ensembl validation | Confirm rsID existence, genomic location, alleles |
| ClinVar review | Check for pathogenic/likely pathogenic variants; review clinical significance |
| GWAS Catalog | Verify trait-SNP associations; check effect sizes, p-values, replication |
| gnomAD | Population allele frequencies; check for population-specific variants |
| HGNC | Validate gene symbols against official nomenclature |
| Literature review | Systematic review of disease association studies; meta-analyses |
| Lab/assay validation | Genotyping platform validation; analytical validity |
| Consent and reporting workflow | Informed consent for disease risk reporting; genetic counseling protocol; regulatory approval |

## Carrier Status / Monogenic Disease (16 traits)

| Evidence Source | Purpose |
|----------------|---------|
| dbSNP/Ensembl validation | Confirm rsID existence, genomic location, alleles |
| ClinVar review | Pathogenicity classification (ACMG/AMP); review clinical significance |
| gnomAD | Carrier frequency estimation; population-specific carrier rates |
| HGNC | Validate gene symbols |
| Literature review | Review of monogenic disease literature; penetrance data |
| Lab/assay validation | Analytical and clinical validity; variant calling confirmation |
| Consent and reporting workflow | Genetic counseling before and after testing; informed consent; ACMG/AMP variant reporting; regulatory approval |

## Pharmacogenomics (26 traits)

| Evidence Source | Purpose |
|----------------|---------|
| dbSNP/Ensembl validation | Confirm rsID existence, genomic location, alleles |
| PharmGKB | Drug-gene associations; annotation levels; clinical annotations |
| CPIC | Clinical practice guidelines; dosing recommendations |
| ClinVar review | Clinical significance for pharmacogenomic variants |
| gnomAD | Population allele frequencies for drug metabolism variants |
| HGNC | Validate gene symbols (especially CYP family, SLCO, etc.) |
| Literature review | Clinical pharmacology literature; drug-specific studies |
| Lab/assay validation | Genotyping platform validation for pharmacogenomic variants |
| Consent and reporting workflow | Informed consent for drug response reporting; clinical pharmacology review; regulatory approval |

## Reproductive Genetics (16 traits)

| Evidence Source | Purpose |
|----------------|---------|
| dbSNP/Ensembl validation | Confirm rsID existence, genomic location, alleles |
| ClinVar review | Pathogenicity classification for reproductive variants |
| gnomAD | Population carrier frequencies; population-specific risks |
| HGNC | Validate gene symbols |
| Literature review | Reproductive genetics literature; penetrance and expressivity data |
| Lab/assay validation | Analytical validity; variant confirmation |
| Consent and reporting workflow | Genetic counseling; ethical review; informed consent for reproductive testing; regulatory approval |

## Nutrition Deficiency or Metabolism (41 traits)

| Evidence Source | Purpose |
|----------------|---------|
| dbSNP/Ensembl validation | Confirm rsID existence, genomic location, alleles |
| GWAS Catalog | Verify trait-SNP associations for nutritional traits |
| gnomAD | Population allele frequencies |
| HGNC | Validate gene symbols |
| Literature review | Nutritional science literature; deficiency association studies |
| Lab/assay validation | Genotyping platform validation |
| Consent and reporting workflow | Informed consent; disclaimer that results are not diagnostic; regulatory review for deficiency claims |

## Substance Response (0 traits)

| Evidence Source | Purpose |
|----------------|---------|
| dbSNP/Ensembl validation | Confirm rsID existence, genomic location, alleles |
| GWAS Catalog | Verify trait-SNP associations for substance response |
| gnomAD | Population allele frequencies |
| HGNC | Validate gene symbols |
| Literature review | Addiction medicine literature; substance use genetics |
| Lab/assay validation | Genotyping platform validation |
| Consent and reporting workflow | Informed consent; ethical review for addiction-related reporting; regulatory review |

## Dermatology or Cosmetic Medical (18 traits)

| Evidence Source | Purpose |
|----------------|---------|
| dbSNP/Ensembl validation | Confirm rsID existence, genomic location, alleles |
| GWAS Catalog | Verify trait-SNP associations for skin/appearance traits |
| gnomAD | Population allele frequencies |
| HGNC | Validate gene symbols |
| Literature review | Dermatology literature; cosmetic science review |
| Lab/assay validation | Genotyping platform validation |
| Consent and reporting workflow | Disclaimer that results are not medical diagnosis; cosmetic safety review; regulatory review for medical claims |

## Other Medical/Sensitive (31 traits)

| Evidence Source | Purpose |
|----------------|---------|
| dbSNP/Ensembl validation | Confirm rsID existence, genomic location, alleles |
| ClinVar review | Check for clinical significance |
| GWAS Catalog | Verify trait-SNP associations |
| gnomAD | Population allele frequencies |
| HGNC | Validate gene symbols |
| Literature review | Domain-specific literature review |
| Lab/assay validation | Genotyping platform validation |
| Consent and reporting workflow | Informed consent; manual scientific review; regulatory review |

## General Requirements (All Modules)

1. **No medical interpretation has been validated.** All rsIDs are externally unvalidated.
2. **dbSNP/Ensembl validation** is the first step for all rsIDs.
3. **HGNC gene validation** is required for all gene symbols.
4. **No risk alleles** will be inferred from WeGene text. Only external sources can assign risk alleles.
5. **Private genotype data** is not used for any annotation.
6. **No clinical, diagnostic, or treatment recommendations** can be made.
7. **All reporting** requires informed consent, regulatory review, and professional oversight.
