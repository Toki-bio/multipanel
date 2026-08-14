# Medical Trait Catalog — Summary Report

## ⚠️ WARNING

**No medical interpretation has been validated.**
This is a candidate medical review catalog, not a sellable clinical product.
All rsIDs are externally unvalidated until dbSNP/Ensembl/ClinVar/gnomAD/GWAS/PharmGKB review is completed.

## Overview

- Total medical/sensitive traits: 578
- Total unique rsIDs across all medical traits: 1949

## Counts by Medical Module

| Medical Module | Count |
|---------------|-------|
| carrier_or_monogenic | 16 |
| complex_disease_risk | 430 |
| dermatology_or_cosmetic_medical | 18 |
| nutrition_deficiency_or_metabolism | 41 |
| other_medical_sensitive | 31 |
| pharmacogenomics | 26 |
| reproductive_genetics | 16 |

## Counts by Evidence Type Needed

| Evidence Type | Traits Needing This |
|---------------|---------------------|
| ClinVar review | 519 |
| GWAS Catalog review | 565 |
| PharmGKB review | 26 |
| CPIC review | 26 |
| gnomAD frequency | 565 |
| HGNC gene validation | 565 |

## Counts by Report Type

| Report Type | Count |
|-------------|-------|
| clinician_review_required | 488 |
| research_only | 31 |
| possible_consumer_with_strong_disclaimer_after_validation | 59 |
| exclude_until_evidence_review | 0 |

## Key Metrics

- Traits needing ClinVar review: 519
- Traits needing GWAS review: 565
- Traits needing PharmGKB/CPIC review: 26
- Traits needing clinician review: 488
- Traits that might become consumer-reportable after validation: 59
- Traits with high medical claim risk: 526
- Traits with high regulatory sensitivity: 488

## Critical Warnings

1. **No medical interpretation has been validated.** All rsIDs are externally unvalidated.
2. **No clinical, diagnostic, or treatment recommendations** can be made.
3. **No trait in this catalog is approved for customer-facing reporting.**
4. **All traits require external annotation** (dbSNP, Ensembl, ClinVar, gnomAD, GWAS Catalog, PharmGKB, HGNC).
5. **All traits require professional review** before any reporting.
6. **Private genotype data has not been used** in this catalog.
7. **This is a candidate catalog** for research and review purposes only.

## Files Created

1. `medical_trait_catalog_CANDIDATE.tsv` — Full medical catalog with all columns
2. `medical_trait_catalog_by_module.md` — Traits grouped by medical module
3. `medical_trait_catalog_validation_requirements.md` — External evidence requirements by module
4. `medical_trait_catalog_not_customer_ready.md` — Traits not ready for customer reporting
5. `medical_trait_catalog_summary_report.md` — This summary
