# WeGene SNP Catalog — External Annotation Plan

**Created:** 2026-07-11  
**Status:** Planning only — no annotation has been performed  
**Scope:** 2191 structured_catalog_unvalidated rsIDs + 530 unvalidated_pdf_candidate rsIDs  
**Total rsIDs in v2 catalog:** 2721 (1 excluded false positive `rs1`)

---

## 1. Source Files

| File | Role |
|------|------|
| `C:\work\wegene\normalized\wegene_snp_catalog_PLUS_pdf_review_CANDIDATE_v2.tsv` | Combined candidate catalog (structured + PDF-only) |
| `C:\work\wegene\normalized\wegene_validation_status_note.md` | Validation status terminology and counts |

### 1.1 Candidate Groups

| Group | Count | Source | Validation Status |
|-------|-------|--------|-------------------|
| Structured catalog | 2191 | WeGene structured files (trait-catalog.json, snp-panel.csv, trait-SNP link tables) | `structured_catalog_unvalidated` |
| PDF-only candidates | 530 | PDF report text (regex extraction) | `unvalidated_pdf_candidate` |
| Excluded | 1 | PDF text (matched PubMed URL, not a real SNP) | `likely_false_positive` |
| **Total in v2 catalog** | **2721** | | |

---

## 2. Data Provenance Layers (must be kept separate)

Every column in the final output must be tagged with one of the following provenance labels:

| Label | Meaning |
|-------|---------|
| `wegene_structured` | Extracted from WeGene structured data files (JSON, CSV) |
| `wegene_pdf_text` | Extracted from WeGene PDF report text via regex |
| `wegene_private_genotype` | Private individual genotype reported by WeGene (not public data) |
| `external_public` | Annotated from a public reference database (dbSNP, Ensembl, ClinVar, gnomAD, GWAS Catalog, PharmGKB) |
| `inferred` | Inferred or uncertain — not directly confirmed by any source above |

---

## 3. Annotation Fields

### 3.1 dbSNP Existence Validation

| Field | Source | Description |
|-------|--------|-------------|
| `dbsnp_exists` | external_public (dbSNP) | Boolean: does this rsID exist in dbSNP? |
| `dbsnp_current_rsid` | external_public (dbSNP) | Current rsID if the original has been merged or deprecated |
| `dbsnp_merged_status` | external_public (dbSNP) | One of: `current`, `merged_into_other`, `deprecated`, `not_found` |
| `dbsnp_merged_from` | external_public (dbSNP) | If merged, the original rsID that was merged |
| `dbsnp_accessed_date` | external_public (dbSNP) | Date of dbSNP lookup |

### 3.2 Genomic Location

| Field | Source | Description |
|-------|--------|-------------|
| `chromosome` | external_public (Ensembl) | Chromosome (1-22, X, Y, MT) |
| `position_grch37` | external_public (Ensembl) | Genomic position on GRCh37 |
| `position_grch38` | external_public (Ensembl) | Genomic position on GRCh38 |
| `genome_build_primary` | external_public (Ensembl) | Which build is used as primary (`GRCh38` preferred) |
| `ensembl_accessed_date` | external_public (Ensembl) | Date of Ensembl lookup |

### 3.3 Alleles

| Field | Source | Description |
|-------|--------|-------------|
| `ref_allele` | external_public (Ensembl/dbSNP) | Reference allele on the forward strand |
| `alt_allele` | external_public (Ensembl/dbSNP) | Alternate allele(s) on the forward strand |
| `allele_source` | external_public | Which database provided the allele |
| `strand_orientation` | external_public (Ensembl) | `+` or `-` relative to reference genome |

### 3.4 Gene Annotation

| Field | Source | Description |
|-------|--------|-------------|
| `gene_symbol_wegene` | wegene_structured or wegene_pdf_text | Gene symbol as reported by WeGene |
| `gene_symbol_validated` | external_public (HGNC) | Gene symbol validated against HGNC |
| `gene_symbol_match` | inferred | Boolean: does WeGene symbol match HGNC? |
| `hgnc_id` | external_public (HGNC) | HGNC identifier |
| `entrez_gene_id` | external_public (NCBI) | Entrez Gene ID |
| `ensembl_gene_id` | external_public (Ensembl) | Ensembl Gene ID (ENSG...) |
| `gene_annotation_source` | external_public | Which database confirmed the gene |

### 3.5 Clinical / Functional Significance

| Field | Source | Description |
|-------|--------|-------------|
| `clinvar_significance` | external_public (ClinVar) | Clinical significance (pathogenic, benign, uncertain, etc.) |
| `clinvar_accession` | external_public (ClinVar) | ClinVar accession ID |
| `clinvar_review_status` | external_public (ClinVar) | ClinVar review status (0–4 stars) |
| `clinvar_accessed_date` | external_public (ClinVar) | Date of ClinVar lookup |

### 3.6 Population Frequency

| Field | Source | Description |
|-------|--------|-------------|
| `gnomad_maf` | external_public (gnomAD) | Minor allele frequency from gnomAD |
| `gnomad_population` | external_public (gnomAD) | Population group (AFR, AMR, ASJ, EAS, FIN, NFE, SAS, etc.) |
| `gnomad_ac` | external_public (gnomAD) | Allele count |
| `gnomad_an` | external_public (gnomAD) | Allele number |
| `gnomad_version` | external_public (gnomAD) | gnomAD version (e.g., v3.1.2, v4.0) |
| `gnomad_accessed_date` | external_public (gnomAD) | Date of gnomAD lookup |

### 3.7 GWAS Associations

| Field | Source | Description |
|-------|--------|-------------|
| `gwas_trait` | external_public (GWAS Catalog) | Associated trait/phenotype |
| `gwas_p_value` | external_public (GWAS Catalog) | Reported p-value |
| `gwas_odds_ratio` | external_public (GWAS Catalog) | Reported odds ratio or beta |
| `gwas_study_accession` | external_public (GWAS Catalog) | Study accession ID |
| `gwas_accessed_date` | external_public (GWAS Catalog) | Date of GWAS Catalog lookup |

### 3.8 Pharmacogenomics

| Field | Source | Description |
|-------|--------|-------------|
| `pharmgkb_annotation` | external_public (PharmGKB) | PharmGKB annotation if applicable |
| `pharmgkb_id` | external_public (PharmGKB) | PharmGKB variant ID |
| `pharmgkb_accessed_date` | external_public (PharmGKB) | Date of PharmGKB lookup |

### 3.9 Risk / Effect Allele (only if explicitly supported)

| Field | Source | Description |
|-------|--------|-------------|
| `risk_allele` | external_public | Risk/effect allele **only** if explicitly stated by ClinVar, GWAS Catalog, or PharmGKB |
| `risk_allele_source` | external_public | Which database provided the risk allele call |
| `risk_allele_confidence` | inferred | `high`, `medium`, `low`, or `not_available` |
| `risk_allele_note` | inferred | Caveats or limitations |

> **Important:** Risk/effect alleles will **not** be inferred from WeGene trait text or PDF text. They will only be recorded when a reliable external source explicitly states which allele is the risk/effect allele.

---

## 4. Provenance Separation Rules

### 4.1 WeGene Structured Extraction (2191 rsIDs)

- **Source:** WeGene structured files (trait-catalog.json, snp-panel.csv, trait-SNP link tables)
- **Provenance tag:** `wegene_structured`
- **Fields from this source:** rsID, gene_symbol_wegene, trait association text
- **Status:** Internally consistent across WeGene files but **not externally validated**
- **Action:** All external annotation fields will be populated from public databases

### 4.2 PDF-Only Candidate Extraction (530 rsIDs)

- **Source:** WeGene PDF report text (regex pattern matching)
- **Provenance tag:** `wegene_pdf_text`
- **Fields from this source:** rsID, gene (if parseable from PDF text), page number
- **Status:** **Unvalidated candidates** — not confirmed as real SNPs
- **Action:** Must first validate existence in dbSNP before any further annotation. If not found in dbSNP, mark as `dbsnp_not_found` and exclude from downstream annotation.

### 4.3 Private Individual Genotype

- **Source:** WeGene report (structured or PDF)
- **Provenance tag:** `wegene_private_genotype`
- **Fields from this source:** observed_genotype_private_present, genotype call
- **Status:** This is **private individual data**, not public annotation
- **Action:** Retain in a separate column clearly labeled as private genotype. Do not mix with public allele frequency data.

### 4.4 External Public Annotation

- **Source:** dbSNP, Ensembl, HGNC, ClinVar, gnomAD, GWAS Catalog, PharmGKB
- **Provenance tag:** `external_public`
- **Fields from this source:** All fields in Section 3 above
- **Status:** To be populated during the annotation phase (not yet started)

### 4.5 Inferred or Uncertain Fields

- **Provenance tag:** `inferred`
- **Examples:** gene_symbol_match (boolean comparison), risk_allele_confidence, risk_allele_note
- **Status:** Derived by comparison or judgment, not directly from a single source

---

## 5. Annotation Workflow (Planned — Not Yet Executed)

### Phase 1: dbSNP Existence Validation

1. For each of the 2721 rsIDs, query dbSNP to confirm existence
2. Record: `dbsnp_exists`, `dbsnp_current_rsid`, `dbsnp_merged_status`
3. For merged rsIDs, update to the current rsID
4. For rsIDs not found in dbSNP, mark `dbsnp_not_found` and exclude from further annotation
5. Record `dbsnp_accessed_date` and dbSNP build/version

### Phase 2: Genomic Location & Alleles

1. Query Ensembl (or dbSNP) for chromosome, position (GRCh37 and GRCh38), ref/alt alleles
2. Record `chromosome`, `position_grch37`, `position_grch38`, `ref_allele`, `alt_allele`
3. Record `ensembl_accessed_date` and Ensembl release version

### Phase 3: Gene Validation

1. Compare WeGene-reported gene symbols against HGNC
2. Record `gene_symbol_validated`, `hgnc_id`, `entrez_gene_id`, `ensembl_gene_id`
3. Flag mismatches between WeGene and HGNC

### Phase 4: Clinical Significance

1. Query ClinVar for clinical significance
2. Record `clinvar_significance`, `clinvar_accession`, `clinvar_review_status`

### Phase 5: Population Frequency

1. Query gnomAD for allele frequencies
2. Record `gnomad_maf`, `gnomad_population`, `gnomad_ac`, `gnomad_an`, `gnomad_version`

### Phase 6: GWAS Associations

1. Query GWAS Catalog for trait associations
2. Record `gwas_trait`, `gwas_p_value`, `gwas_odds_ratio`, `gwas_study_accession`

### Phase 7: Pharmacogenomics

1. Query PharmGKB for pharmacogenomic annotations
2. Record `pharmgkb_annotation`, `pharmgkb_id`

### Phase 8: Risk Allele Assignment

1. **Only** if ClinVar, GWAS Catalog, or PharmGKB explicitly states a risk/effect allele
2. Record `risk_allele`, `risk_allele_source`, `risk_allele_confidence`
3. If no explicit source, set `risk_allele = not_available`

### Phase 9: Report Generation

1. Generate summary report with validation rates, annotation coverage, and data quality notes
2. Document all source database versions and access dates

---

## 6. Proposed Output Files

### 6.1 Main Annotated Catalog

**File:** `C:\work\wegene\normalized\wegene_snps_validated_annotated.tsv`

**Description:** All 2191 structured_catalog_unvalidated rsIDs with full external annotation.

**Key columns:**

```
rsid
dbsnp_exists
dbsnp_current_rsid
dbsnp_merged_status
chromosome
position_grch37
position_grch38
genome_build_primary
ref_allele
alt_allele
gene_symbol_wegene                    [provenance: wegene_structured]
gene_symbol_validated                 [provenance: external_public]
hgnc_id
entrez_gene_id
ensembl_gene_id
clinvar_significance
clinvar_accession
clinvar_review_status
gnomad_maf
gnomad_population
gnomad_version
gwas_trait
gwas_p_value
gwas_odds_ratio
pharmgkb_annotation
risk_allele
risk_allele_source
risk_allele_confidence
observed_genotype_private_present     [provenance: wegene_private_genotype]
validation_status
source_database_versions
annotation_access_date
```

### 6.2 Trait-SNP Links (Validated)

**File:** `C:\work\wegene\normalized\wegene_trait_snp_links_validated.tsv`

**Description:** WeGene trait-SNP associations with validated rsIDs and gene symbols.

**Key columns:**

```
rsid
dbsnp_current_rsid
trait_wegene                          [provenance: wegene_structured]
gene_symbol_wegene                    [provenance: wegene_structured]
gene_symbol_validated                 [provenance: external_public]
trait_category_wegene                 [provenance: wegene_structured]
validation_status
```

### 6.3 PDF-Only rsID Validation Results

**File:** `C:\work\wegene\pdf_extracted\wegene_pdf_only_rsids_validation_results.tsv`

**Description:** 530 PDF-only candidate rsIDs with dbSNP validation results.

**Key columns:**

```
rsid
dbsnp_exists
dbsnp_current_rsid
dbsnp_merged_status
gene_from_pdf_text                    [provenance: wegene_pdf_text]
page_from_pdf                         [provenance: wegene_pdf_text]
genotype_from_pdf                     [provenance: wegene_private_genotype]
chromosome
position_grch38
ref_allele
alt_allele
gene_symbol_validated
clinvar_significance
gnomad_maf
gwas_trait
risk_allele
risk_allele_source
validation_status
annotation_notes
```

### 6.4 Annotation Report

**File:** `C:\work\wegene\wegene_external_annotation_report.md`

**Description:** Summary report documenting:

- Total rsIDs queried vs. validated
- dbSNP existence rate (structured vs. PDF-only)
- Merge/deprecation rate
- Gene symbol match rate (WeGene vs. HGNC)
- ClinVar annotation coverage
- gnomAD frequency coverage
- GWAS Catalog coverage
- PharmGKB coverage
- Risk allele assignment rate
- Source database versions and access dates
- Limitations and caveats
- Recommendations for downstream use

---

## 7. Source Database Versions (to be recorded during annotation)

| Database | Version/Build | Access Date | URL |
|----------|---------------|-------------|-----|
| dbSNP | TBD | TBD | https://www.ncbi.nlm.nih.gov/snp/ |
| Ensembl | TBD | TBD | https://www.ensembl.org/ |
| HGNC | TBD | TBD | https://www.genenames.org/ |
| ClinVar | TBD | TBD | https://www.ncbi.nlm.nih.gov/clinvar/ |
| gnomAD | TBD | TBD | https://gnomad.broadinstitute.org/ |
| GWAS Catalog | TBD | TBD | https://www.ebi.ac.uk/gwas/ |
| PharmGKB | TBD | TBD | https://www.pharmgkb.org/ |

> All versions and access dates will be filled in during the annotation phase. No annotation has been performed yet.

---

## 8. Limitations and Caveats

1. **No annotation has been performed.** This document is a plan only.
2. **PDF-only candidates are unvalidated.** 530 rsIDs from PDF text have not been confirmed as real SNPs. Some may be false positives (like the excluded `rs1`).
3. **Private genotypes are individual data.** The `observed_genotype_private_present` field reflects a single individual's genotype from the WeGene report, not population-level data.
4. **Risk alleles require explicit external support.** No risk allele will be inferred from WeGene trait text or PDF text.
5. **Gene symbols may not match.** WeGene-reported gene symbols may differ from HGNC official symbols. Mismatches will be flagged.
6. **Merged rsIDs.** Some rsIDs may have been merged into newer rsIDs in dbSNP. The current rsID will be recorded.
7. **Genome build.** Both GRCh37 and GRCh38 positions will be recorded where available. GRCh38 will be the primary build.
8. **Annotation coverage will be incomplete.** Not all rsIDs will have ClinVar, gnomAD, GWAS Catalog, or PharmGKB annotations. Missing data will be marked as `not_available` or left blank with a note.

---

## 9. Next Steps (After Plan Approval)

1. Obtain access to dbSNP, Ensembl, HGNC, ClinVar, gnomAD, GWAS Catalog, and PharmGKB (via API, bulk download, or local database)
2. Execute Phase 1–9 as described in Section 5
3. Generate the four output files described in Section 6
4. Review and validate results
5. Update `validation_status` field for all rsIDs based on external annotation results

---

**End of Plan**