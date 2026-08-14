# WeGene SNP & Trait Cross-Check Report

Generated: 2026-07-10  
Directory: `C:\work\wegene`  
Files cross-checked: `trait-catalog.json`, `snp-extract-cache.json`, `trait-catalog.csv`, `snp-panel.csv`, `snp-panel.txt`

---

## 1. Unique rsID Counts

| File | Unique rsIDs |
|------|-------------|
| `trait-catalog.json` (from `traits[].assay.snps[].rsid`) | 2191 |
| `trait-catalog.json` (from `snp_panel.rsids[]`) | 2191 |
| `snp-extract-cache.json` (from all entries' `snps[].rsid`) | 2191 |
| `trait-catalog.csv` (from `snps` column, semicolon-separated) | 2191 |
| `snp-panel.csv` (from `rsid` column) | 2191 |
| `snp-panel.txt` (one per line) | 2191 |

**Result:** All five files contain exactly the same 2191 unique rsIDs. No discrepancies.

---

## 2. snp-panel.csv vs snp-panel.txt

| Metric | Value |
|--------|-------|
| snp-panel.csv rsIDs | 2191 |
| snp-panel.txt rsIDs | 2191 |
| Identical sets | ✅ Yes |
| In CSV only | 0 |
| In TXT only | 0 |
| Duplicates in CSV | 0 |
| Duplicates in TXT | 0 |

**Result:** The two files are perfectly identical in content — same 2191 rsIDs, no duplicates in either.

---

## 3. Traits by SNP Count

| SNP count bucket | Trait count |
|------------------|-------------|
| 0 SNPs | 24 |
| 1 SNP | 123 |
| 2–5 SNPs | 421 |
| >5 SNPs | 128 |
| **Total** | **696** |

**Note:** The `trait-catalog.md` summary file states "Traits without SNPs: 16", but the actual count from `trait-catalog.json` is **24**. This is a discrepancy in the summary document, not in the data files.

---

## 4. Top 20 rsIDs by Linked Trait Count

| Rank | rsID | Linked traits (JSON) | Linked traits (CSV) |
|------|------|---------------------|---------------------|
| 1 | rs429358 | 11 | 11 |
| 2 | rs10489 | 10 | 10 |
| 3 | rs39812 | 10 | 10 |
| 4 | rs1801133 | 9 | 9 |
| 5 | rs79704 | 8 | 8 |
| 6 | rs11859 | 8 | 8 |
| 7 | rs671 | 7 | 7 |
| 8 | rs1229984 | 7 | 7 |
| 9 | rs12191 | 6 | 6 |
| 10 | rs12190 | 6 | 6 |
| 11 | rs13785 | 6 | 6 |
| 12 | rs83813 | 6 | 6 |
| 13 | rs39751 | 5 | 5 |
| 14 | rs12143 | 5 | 5 |
| 15 | rs58542926 | 4 | 4 |
| 16 | rs2187668 | 4 | 4 |
| 17 | rs1800896 | 4 | 4 |
| 18 | rs7574865 | 4 | 4 |
| 19 | rs58777 | 4 | 4 |
| 20 | rs86322 | 4 | 4 |

**Result:** JSON and CSV counts are perfectly consistent. The most connected rsID is `rs429358` (APOE locus, linked to 11 traits).

---

## 5. Top 20 Genes by Linked Trait Count

| Rank | Gene | Linked traits |
|------|------|--------------|
| 1 | II | 72 |
| 2 | DD | 27 |
| 3 | AC | 23 |
| 4 | APOE | 17 |
| 5 | MTHFR | 10 |
| 6 | CDH13 | 10 |
| 7 | ADH1B | 9 |
| 8 | FGF21 | 9 |
| 9 | ALDH2 | 7 |
| 10 | FUT2 | 6 |
| 11 | COL2A1 | 6 |
| 12 | STAT4 | 6 |
| 13 | TAS2R38 | 6 |
| 14 | FTO | 5 |
| 15 | ABO | 5 |
| 16 | TYR | 5 |
| 17 | SLC39A8 | 4 |
| 18 | IL13 | 4 |
| 19 | LPP | 4 |
| 20 | GSTP1 | 4 |

**⚠️ Data quality issue:** The top three "genes" — `II` (72 traits), `DD` (27 traits), and `AC` (23 traits) — are **not valid HGNC gene symbols**. These appear to be artifacts from PDF text extraction (likely Roman numeral fragments, blood group notations, or truncated text). They should be filtered or corrected.

**Total unique gene symbols:**
- `trait-catalog.json`: 1131 (includes the spurious entries above)
- `snp-panel.csv`: 314 (parsed from semicolon-separated `genes` column)

The large discrepancy (1131 vs 314) is because `trait-catalog.json` collects genes from both `assay.genes[]` and `snps[].gene`, while `snp-panel.csv` only has a `genes` column that is frequently empty.

---

## 6. rsIDs in trait-catalog.json but Missing from snp-panel.csv

| Metric | Value |
|--------|-------|
| Count | **0** |

**Result:** No rsIDs are missing. All rsIDs in the JSON are present in the CSV panel.

---

## 7. rsIDs in snp-panel.csv but Not Referenced by Any Trait

| Metric | Value |
|--------|-------|
| Count | **0** |

**Result:** Every rsID in the panel is referenced by at least one trait in the JSON. No orphan rsIDs.

---

## 8. Traits Where Genes Exist but rsIDs Are Missing

| Metric | Value |
|--------|-------|
| Count | **0** |

**Result:** No traits have gene annotations without corresponding SNP rsIDs. Every trait that lists genes also has at least one SNP.

---

## 9. Traits Where Genotype Exists but Gene Is Missing

| Metric | Value |
|--------|-------|
| Count | **251** SNPs across multiple traits |

This is the most significant data quality issue: 251 individual SNP entries have a genotype (e.g., `TT`, `CT`, `GG`) but no gene symbol annotation.

### Examples (first 20)

| Trait ID | Trait name | rsID | Genotype | Gene |
|----------|-----------|------|----------|------|
| wg-253 | Vitamin B12 Nutritional Requirement | rs10515 | TT | None |
| wg-253 | Vitamin B12 Nutritional Requirement | rs12377 | CT | None |
| wg-250 | Vitamin E Nutritional Requirement | rs699407 | AT | None |
| wg-42 | Age-related Macular Degeneration (AMD) | rs3793917 | TT | None |
| wg-52 | Alcohol Dependence | rs2673136 | CT | None |
| wg-1647 | Alcoholic Fatty Liver Disease (AFLD) | rs738409 | CT | None |
| wg-1521 | Allergic Diseases | rs1444789 | CC | None |
| wg-57 | Allergic Rhinitis | rs2155219 | TT | None |
| wg-57 | Allergic Rhinitis | rs7775228 | CT | None |
| wg-99 | Alopecia Areata | rs9275572 | GG | None |
| wg-60 | Amyotrophic Lateral Sclerosis (ALS) | rs10260404 | CT | None |
| wg-1699 | Anorexia Nervosa | rs13100344 | CC | None |
| wg-53 | Asthma | rs12479210 | GG | None |
| wg-1768 | Astigmatism | rs7677751 | CC | None |
| wg-1768 | Astigmatism | rs12032649 | AA | None |
| wg-39 | Atrial Fibrillation | rs16997168 | TT | None |
| wg-39 | Atrial Fibrillation | rs3853445 | CC | None |
| wg-1587 | Autism | rs16976358 | CC | None |
| wg-1956 | Bladder Cancer | rs1495741 | TT | None |
| wg-1769 | Cataract | rs7615568 | CG | None |

**Impact:** ~11.5% of all SNP entries (251 of ~2191 unique) lack gene annotation despite having a genotype. These would require external annotation (e.g., dbSNP lookup) to populate the gene field.

---

## 10. Duplicate / Inconsistent Mappings Between JSON and CSV

### 10a. snp-panel.csv trait_ids vs JSON trait IDs

| Metric | Value |
|--------|-------|
| Consistent | 2191 |
| Inconsistent | 0 |

**Result:** Every rsID in `snp-panel.csv` has exactly the same set of linked trait IDs as in `trait-catalog.json`. Perfectly consistent.

### 10b. Duplicate rsIDs in snp-panel.csv

| Metric | Value |
|--------|-------|
| Duplicate rsIDs | 0 |

**Result:** No duplicates. Each rsID appears exactly once.

### 10c. Duplicate rsIDs in snp-panel.txt

| Metric | Value |
|--------|-------|
| Duplicate rsIDs | 0 |

**Result:** No duplicates.

### 10d. trait-catalog.csv vs JSON: snps column consistency

| Metric | Value |
|--------|-------|
| Match | 696 |
| Mismatch | 0 |

**Result:** Every row in `trait-catalog.csv` has the same set of rsIDs as the corresponding trait in `trait-catalog.json`. Perfectly consistent.

### 10e. snp-extract-cache vs JSON: rsID consistency per report_id

| Metric | Value |
|--------|-------|
| Match | 688 |
| Mismatch | 0 |
| Traits without report_id (not checked) | 8 |

**Result:** For all 688 traits with a `report_id`, the rsIDs in the cache match the JSON exactly. The remaining 8 traits (ancestry/haplogroup) have `report_id: null` and no SNPs, so there is nothing to check.

### 10f. Genes consistency: cache vs JSON per report_id

| Metric | Value |
|--------|-------|
| Match | 678 |
| Mismatch | 10 |

**10 gene mismatches** — all cases where the JSON has additional genes not present in the cache:

| report_id | Trait name | Cache genes | JSON-only genes |
|-----------|-----------|-------------|-----------------|
| 1726 | Brugada Syndrome | (none) | KCNH2 |
| 2000 | Developmental and Epileptic Encephalopathy (DEE) | (none) | KCNQ2 |
| 1745 | Familial Atrial Fibrillation | (none) | KCNA5 |
| 2066 | Holoprosencephaly (HPE) | (none) | CDON |
| 1727 | Loeys Dietz Syndrome | (none) | TGFBR2 |
| 1556 | Ability To Resist Senile Plaques | (none) | PPARGC1 |
| 1959 | Hepatitis Virus Infection | (none) | CFB |
| 1948 | Smoking × Prostate Cancer | (none) | CYP1A1 |
| 2343 | Crisps Preference | (none) | MFHAS |
| 2369 | Marzipan Preference | (none) | ABO |

**Interpretation:** In these 10 cases, the JSON catalog was enriched with gene annotations that were not present in the raw PDF extraction cache. This suggests manual curation or external annotation was applied to the JSON after extraction. The gene `MFHAS` (rank 2343) may be a typo — the correct symbol is likely `MFHAS1`.

---

## 11. Proposed Normalized Schema

### 11.1 `traits` table

| Column | Type | Description | Source |
|--------|------|-------------|--------|
| `trait_id` | TEXT PK | Unique trait ID (e.g., `wg-249`) | `traits[].id` |
| `name` | TEXT | Trait name (e.g., "Alcohol Metabolism") | `traits[].name` |
| `category` | TEXT FK | Category ID | `traits[].category` |
| `outcome` | TEXT NULL | Result outcome (e.g., "Moderate") | `traits[].result.outcome` |
| `outcome_type` | TEXT NULL | Result type (e.g., `metabolism_level`) | `traits[].result.outcome_type` |
| `value` | REAL NULL | Numeric value if applicable | `traits[].result.value` |
| `unit` | TEXT NULL | Unit of measurement | `traits[].result.unit` |
| `recommendation` | TEXT NULL | Recommendation text | `traits[].result.recommendation` |
| `raw_label` | TEXT NULL | Original label from PDF | `traits[].result.raw_label` |
| `scoring_model` | TEXT NULL | Scoring method (e.g., `genotype_lookup`) | `traits[].assay.scoring_model` |
| `evidence_level` | TEXT NULL | Evidence strength | `traits[].assay.evidence_level` |
| `source_provider` | TEXT | Data provider (e.g., "WeGene") | `traits[].source.provider` |
| `source_url` | TEXT NULL | Original report URL | `traits[].source.url` |
| `source_report_id` | INT NULL | WeGene report ID | `traits[].source.report_id` |
| `source_pdf` | TEXT NULL | Source PDF page filename | `traits[].source.pdf` |

### 11.2 `snps` table

| Column | Type | Description | Source |
|--------|------|-------------|--------|
| `rsid` | TEXT PK | dbSNP rsID (e.g., `rs671`) | `snps[].rsid` |
| `gene_symbol` | TEXT FK NULL | Gene symbol (e.g., `ALDH2`) | `snps[].gene` |
| `chromosome` | TEXT NULL | Chromosome (e.g., "12") | **Missing — needs external annotation** |
| `position` | INT NULL | Genomic position (GRCh38) | **Missing — needs external annotation** |
| `genome_build` | TEXT NULL | Genome build (e.g., "GRCh38") | **Missing — needs external annotation** |
| `ref_allele` | TEXT NULL | Reference allele | **Missing — needs external annotation** |
| `alt_allele` | TEXT NULL | Alternate allele(s) | **Missing — needs external annotation** |
| `risk_allele` | TEXT NULL | Risk/effect allele | **Missing — needs external annotation** |
| `clinical_significance` | TEXT NULL | ClinVar significance | **Missing — needs external annotation** |
| `population_frequency` | REAL NULL | Population MAF | **Missing — needs external annotation** |

### 11.3 `trait_snp_links` table

| Column | Type | Description | Source |
|--------|------|-------------|--------|
| `trait_id` | TEXT FK | → `traits.trait_id` | `traits[].id` |
| `rsid` | TEXT FK | → `snps.rsid` | `snps[].rsid` |
| `genotype` | TEXT NULL | Example genotype (e.g., `GG`) | `snps[].genotype` |
| `alleles` | TEXT NULL | Allele info | `snps[].alleles` |
| `effect` | TEXT NULL | Effect annotation | `snps[].effect` |
| **PK** | | (`trait_id`, `rsid`) | |

### 11.4 `genes` table

| Column | Type | Description | Source |
|--------|------|-------------|--------|
| `gene_symbol` | TEXT PK | HGNC gene symbol (e.g., `ALDH2`) | `assay.genes[]`, `snps[].gene` |
| `hgnc_id` | TEXT NULL | HGNC identifier | **Missing — needs external annotation** |
| `entrez_id` | TEXT NULL | NCBI Entrez Gene ID | **Missing — needs external annotation** |
| `ensembl_id` | TEXT NULL | Ensembl gene ID | **Missing — needs external annotation** |
| `chromosome` | TEXT NULL | Chromosome location | **Missing — needs external annotation** |
| `start_pos` | INT NULL | Gene start position | **Missing — needs external annotation** |
| `end_pos` | INT NULL | Gene end position | **Missing — needs external annotation** |

### 11.5 `categories` table (supplementary)

| Column | Type | Description | Source |
|--------|------|-------------|--------|
| `category_id` | TEXT PK | Category ID (e.g., `nutrition_metabolism`) | `categories[].id` |
| `name` | TEXT | English name | `categories[].name` |
| `name_ru` | TEXT | Russian name | `categories[].name_ru` |
| `description` | TEXT NULL | Category description | `categories[].description` |

---

## 12. Missing Data Requiring External Annotation

The following fields are **not present** in any of the WeGene structured files and would require external annotation databases (dbSNP, HGNC, ClinVar, gnomAD, etc.):

| Missing field | Why it matters | Suggested source |
|---------------|---------------|------------------|
| **Chromosome** | Required for genomic coordinate mapping, LD analysis, and variant clustering | dbSNP / Ensembl / UCSC |
| **Position** | Required for genomic coordinate mapping and LD analysis | dbSNP / Ensembl / UCSC |
| **Genome build** | Required to interpret positions correctly (e.g., GRCh37 vs GRCh38) | Must be specified when annotating |
| **Reference allele** | Required to determine strand orientation and variant normalization | dbSNP / Ensembl / UCSC |
| **Alternate allele(s)** | Required to define the variant and match against genotyping arrays | dbSNP / Ensembl / UCSC |
| **Risk allele** | Required to interpret genotype results and determine risk direction | GWAS Catalog / ClinVar / literature |
| **Clinical significance** | Required for health-risk traits to assess pathogenicity | ClinVar / ClinGen |
| **Population frequency** | Required for risk assessment and population-specific interpretation | gnomAD / 1000 Genomes |

### Additional data quality issues requiring remediation

| Issue | Count | Remediation |
|-------|-------|------------|
| SNPs with genotype but no gene | 251 | Annotate via dbSNP rsID → gene lookup |
| Spurious "gene" symbols (II, DD, AC, etc.) | ~3 entries affecting 122 trait links | Filter non-HGNC symbols; re-extract from PDF or annotate from dbSNP |
| Possible gene typo (`MFHAS` → `MFHAS1`) | 1 | Verify against HGNC and correct |
| `trait-catalog.md` states 16 traits without SNPs; actual count is 24 | 1 doc | Update the markdown summary |
| `report_copy` fields all null | 696 traits | Populate from PDF if needed |
| `evidence_level` and `references` all null | 696 traits | Annotate from GWAS Catalog / literature if needed |
| Gene mismatch between cache and JSON (JSON has extra genes) | 10 traits | Reconcile — JSON appears to be manually curated; verify correctness |

---

## Summary

| Check | Result |
|-------|--------|
| rsID count consistency across all 5 files | ✅ All 2191 — identical |
| snp-panel.csv ↔ snp-panel.txt | ✅ Identical |
| snp-panel.csv ↔ JSON trait links | ✅ Consistent (0 mismatches) |
| trait-catalog.csv ↔ JSON snps column | ✅ Consistent (0 mismatches) |
| snp-extract-cache ↔ JSON rsIDs | ✅ Consistent (0 mismatches) |
| snp-extract-cache ↔ JSON genes | ⚠️ 10 mismatches (JSON has extra genes) |
| Duplicate rsIDs | ✅ None in any file |
| Orphan rsIDs (in panel but not in traits) | ✅ None |
| Traits with genes but no rsIDs | ✅ None |
| SNPs with genotype but no gene | ⚠️ 251 entries |
| Spurious gene symbols | ⚠️ `II`, `DD`, `AC` (not valid HGNC) |
| Chromosome/position data | ❌ Missing entirely |
| Reference/alternate/risk alleles | ❌ Missing entirely |
| Clinical significance | ❌ Missing entirely |
| Population frequency | ❌ Missing entirely |