# WeGene Structured Files — Schema Report

Generated: 2026-07-10  
Directory: `C:\work\wegene`  
Source PDF (not parsed in this task): `wegene-all-reports-full.pdf` (91.78 MB)

---

## Summary

All six structured files are **derived from the WeGene PDF report** (`wegene-all-reports-full.pdf`). The `trait-catalog.json` is the master structured catalog; the CSV and TXT files are tabular/plain-text exports of the same data. The `snp-extract-cache.json` is an intermediate extraction cache mapping individual PDF page files to extracted SNP/genotype data. The `trait-catalog.md` is a human-readable summary.

| File | Format | Records | Size |
|------|--------|---------|------|
| `trait-catalog.json` | JSON (dict) | 696 traits, 14 categories, 2191 rsIDs | 1.27 MB |
| `snp-extract-cache.json` | JSON (dict) | 696 entries (one per PDF page) | 210.21 KB |
| `trait-catalog.csv` | CSV | 696 rows | 145.83 KB |
| `snp-panel.csv` | CSV | 2191 rows | 112.40 KB |
| `snp-panel.txt` | TXT | 2191 lines | 22.43 KB |
| `trait-catalog.md` | Markdown | 1 doc | 724 B |

---

## 1. `trait-catalog.json`

| Property | Value |
|----------|-------|
| **Format** | JSON (top-level dict) |
| **Object count** | 696 traits, 14 categories, 2191 unique rsIDs |
| **Top-level keys** | `schema_version`, `generated_at`, `purpose`, `source`, `stats`, `categories`, `traits`, `snp_panel` |
| **Contains rsIDs** | ✅ Yes — `traits[].assay.snps[].rsid`, `snp_panel.rsids[]` |
| **Contains gene symbols** | ✅ Yes — `traits[].assay.snps[].gene`, `traits[].assay.genes[]` |
| **Contains chromosome/position** | ❌ No |
| **Contains alleles/genotypes/risk alleles** | ✅ Yes — `traits[].assay.snps[].genotype`, `traits[].assay.snps[].alleles` |
| **Contains disease/trait names** | ✅ Yes — `traits[].name`, `categories[].name`, `categories[].name_ru` |
| **Derived from PDF or independently structured** | Derived from PDF — `source.pdf` = `"wegene-all-reports-full.pdf"`, each trait has `source.pdf` referencing individual page PDFs (e.g., `"00003__249.pdf"`). `source.note` states: "Reference for structure/UX only. SNP panels, scoring and copy must be independently curated." |

### Top-level key details

| Key | Type | Length | Description |
|-----|------|--------|-------------|
| `schema_version` | str | — | `"3.0"` |
| `generated_at` | str | — | ISO timestamp `"2026-07-10T10:31:18.695046+00:00"` |
| `purpose` | str | — | Description of the catalog purpose |
| `source` | dict | 5 keys | `provider`, `locale`, `manifest`, `pdf`, `note` |
| `stats` | dict | 7 keys | `total_traits`, `by_category`, `outcome_types`, `missing_categories`, `traits_with_snps`, `traits_without_snps`, `unique_rsids` |
| `categories` | list[dict] | 14 | Each: `id`, `name`, `name_ru`, `description`, `trait_count`, `outcome_types`, `traits` |
| `traits` | list[dict] | 696 | Each: `id`, `source`, `category`, `name`, `result`, `assay`, `report_copy` |
| `snp_panel` | dict | 4 keys | `total_unique_rsids` (2191), `rsids` (list), `note`, `extracted_at` |

### Trait object keys

| Key | Type | Description |
|-----|------|-------------|
| `id` | str | Unique trait ID (e.g., `"wg-249"`) |
| `source` | dict | `provider`, `locale`, `url`, `report_id`, `pdf` |
| `category` | str | Category ID (e.g., `"nutrition_metabolism"`) |
| `name` | str | Trait name (e.g., `"Alcohol Metabolism"`) |
| `result` | dict | `outcome`, `outcome_type`, `value`, `unit`, `recommendation`, `raw_label`, `is_new_label` |
| `assay` | dict | `snps[]`, `scoring_model`, `evidence_level`, `references[]`, `genes[]` |
| `report_copy` | dict | `summary_template`, `interpretation_template`, `action_template` (all null) |

### SNP object keys (within `assay.snps[]`)

| Key | Type | Description |
|-----|------|-------------|
| `rsid` | str | dbSNP rsID (e.g., `"rs671"`) |
| `gene` | str\|null | Gene symbol (e.g., `"ALDH2"`) |
| `alleles` | str\|null | Allele info (null in examples) |
| `genotype` | str\|null | Example genotype (e.g., `"GG"`) |
| `effect` | str\|null | Effect annotation (null in examples) |

### Representative examples (truncated)

**Example 1** (trait without SNPs — ancestry):
```json
{"id": "wg-ancestry-analysis", "source": {"provider": "WeGene", "locale": "en", "url": "https://www.wegene.com/en/report/ancestry/", "report_id": null, "pdf": "00695__Ancestry_Analysis.pdf"}, "category": "ancestry", "name": "Ancestry Analysis", "result": {"outcome": null, "outcome_type": "unknown", "value": null, "unit": null, "recommendation": null, "raw_label": "Ancestry Analysis", "is_new_label": false}, "assay": {"snps": [], "scoring_model": null, "evidence_level": null, "references": []}, "report_copy": {"summary_template": null, "interpretation_template": null, "action_template": null}}
```

**Example 2** (trait with SNPs — Alcohol Metabolism):
```json
{"id": "wg-249", "source": {"provider": "WeGene", "locale": "en", "url": "https://www.wegene.com/en/report2/detail/249", "report_id": 249, "pdf": "00003__249.pdf"}, "category": "nutrition_metabolism", "name": "Alcohol Metabolism", "result": {"outcome": "Moderate", "outcome_type": "metabolism_level", "value": null, "unit": null, "recommendation": "hard to get dizzy and alcohol flush", "raw_label": "Alcohol Metabolism Moderate, hard to get dizzy and alcohol flush", "is_new_label": false}, "assay": {"snps": [{"rsid": "rs671", "gene": "ALDH2", "alleles": null, "genotype": "GG", "effect": null}, {"rsid": "rs1229984", "gene": null, "alleles": null, "genotype": null, "effect": null}, {"rsid": "rs2066702", "gene": null, "alleles": null, "genotype": null, "effect": null}], "scoring_model": "genotype_lookup", "evidence_level": null, "references": [], "genes": ["ALDH2", "ADH1B"]}, "report_copy": {"summary_template": null, "interpretation_template": null, "action_template": null}}
```

**Example 3** (trait with SNPs — Caffeine Metabolism):
```json
{"id": "wg-22", "source": {"provider": "WeGene", "locale": "en", "url": "https://www.wegene.com/en/report2/detail/22", "report_id": 22, "pdf": "00002__22.pdf"}, "category": "nutrition_metabolism", "name": "Caffeine Metabolism", "result": {"outcome": "Strong", "outcome_type": "metabolism_level", "value": null, "unit": null, "recommendation": null, "raw_label": "Caffeine Metabolism Strong", "is_new_label": false}, "assay": {"snps": [{"rsid": "rs762551", "gene": "CYP1A2", "alleles": null, "genotype": "AA", "effect": null}], "scoring_model": "genotype_lookup", "evidence_level": null, "references": [], "genes": ["CYP1A2"]}, "report_copy": {"summary_template": null, "interpretation_template": null, "action_template": null}}
```

### Stats summary

| Stat | Value |
|------|-------|
| Total traits | 696 |
| Traits with SNPs | 672 |
| Traits without SNPs | 16 |
| Unique rsIDs | 2191 |

### Category breakdown

| Category | Trait count |
|----------|-------------|
| ancestry | 8 |
| nutrition_metabolism | 16 |
| health_risk | 127 |
| genetic_disease | 318 |
| genetic_characteristics | 10 |
| genetic_tendency | 23 |
| psychological_traits | 22 |
| skin_characteristics | 16 |
| infection_risk | 12 |
| lifestyle_guidance | 17 |
| dietary_preference | 102 |
| exposure_risk | 25 |
| pharmacogenetics | 0 |
| uncategorized | 0 |

---

## 2. `snp-extract-cache.json`

| Property | Value |
|----------|-------|
| **Format** | JSON (dict keyed by report_id string) |
| **Object count** | 696 entries (one per PDF page/report) |
| **Entry keys** | `pdf`, `report_id`, `genes`, `snps`, `rsid_count` |
| **SNP sub-keys** | `rsid`, `gene`, `genotype` |
| **Contains rsIDs** | ✅ Yes — `snps[].rsid` |
| **Contains gene symbols** | ✅ Yes — `snps[].gene`, `genes[]` |
| **Contains chromosome/position** | ❌ No |
| **Contains alleles/genotypes/risk alleles** | ✅ Yes — `snps[].genotype` (e.g., `"GG"`, `"AA"`) |
| **Contains disease/trait names** | ❌ No — only `report_id` and `pdf` filename |
| **Derived from PDF or independently structured** | Derived from PDF — each entry maps to an individual page PDF (e.g., `"00001__5.pdf"`). This is the intermediate extraction cache from parsing the PDF pages. |

### Representative examples (truncated)

**Example 1** (key `"5"` — Lactose Metabolism):
```json
{
  "pdf": "00001__5.pdf",
  "report_id": 5,
  "genes": ["MCM6"],
  "snps": [
    {"rsid": "rs4988235", "gene": "MCM6", "genotype": "GG"},
    {"rsid": "rs182549", "gene": "MCM6", "genotype": "CC"}
  ],
  "rsid_count": 2
}
```

**Example 2** (key `"22"` — Caffeine Metabolism):
```json
{
  "pdf": "00002__22.pdf",
  "report_id": 22,
  "genes": ["CYP1A2"],
  "snps": [
    {"rsid": "rs762551", "gene": "CYP1A2", "genotype": "AA"}
  ],
  "rsid_count": 1
}
```

**Example 3** (key `"249"` — Alcohol Metabolism, inferred from report_id):
```json
{
  "pdf": "00003__249.pdf",
  "report_id": 249,
  "genes": ["ALDH2"],
  "snps": [
    {"rsid": "rs671", "gene": "ALDH2", "genotype": "GG"}
  ],
  "rsid_count": 1
}
```

---

## 3. `trait-catalog.csv`

| Property | Value |
|----------|-------|
| **Format** | CSV (with header row) |
| **Row count** | 696 |
| **Columns** | `id`, `category`, `name`, `outcome_type`, `example_outcome`, `example_value`, `unit`, `recommendation`, `source_report_id`, `source_url`, `snps`, `genes`, `genotypes`, `scoring_model`, `evidence_level` |
| **Contains rsIDs** | ✅ Yes — `snps` column (semicolon-separated, e.g., `"rs671;rs1229984;rs2066702"`) |
| **Contains gene symbols** | ✅ Yes — `genes` column (semicolon-separated, e.g., `"ALDH2;ADH1B"`) |
| **Contains chromosome/position** | ❌ No |
| **Contains alleles/genotypes/risk alleles** | ✅ Yes — `genotypes` column (format: `rsid=genotype`, e.g., `"rs671=GG"`) |
| **Contains disease/trait names** | ✅ Yes — `name` column |
| **Derived from PDF or independently structured** | Derived from PDF — exported from `trait-catalog.json`, which is derived from the PDF. `source_url` and `source_report_id` reference WeGene report pages. |

### Representative examples (truncated)

**Example 1** (trait without SNPs):
```json
{"id": "wg-ancestry-analysis", "category": "ancestry", "name": "Ancestry Analysis", "outcome_type": "unknown", "example_outcome": "", "example_value": "", "unit": "", "recommendation": "", "source_report_id": "", "source_url": "https://www.wegene.com/en/report/ancestry/", "snps": "", "genes": "", "genotypes": "", "scoring_model": "", "evidence_level": ""}
```

**Example 2** (trait with SNPs — Alcohol Metabolism):
```json
{"id": "wg-249", "category": "nutrition_metabolism", "name": "Alcohol Metabolism", "outcome_type": "metabolism_level", "example_outcome": "Moderate", "example_value": "", "unit": "", "recommendation": "hard to get dizzy and alcohol flush", "source_report_id": "249", "source_url": "https://www.wegene.com/en/report2/detail/249", "snps": "rs671;rs1229984;rs2066702", "genes": "ALDH2;ADH1B", "genotypes": "rs671=GG", "scoring_model": "genotype_lookup", "evidence_level": ""}
```

**Example 3** (trait with SNPs — Caffeine Metabolism):
```json
{"id": "wg-22", "category": "nutrition_metabolism", "name": "Caffeine Metabolism", "outcome_type": "metabolism_level", "example_outcome": "Strong", "example_value": "", "unit": "", "recommendation": "", "source_report_id": "22", "source_url": "https://www.wegene.com/en/report2/detail/22", "snps": "rs762551", "genes": "CYP1A2", "genotypes": "rs762551=AA", "scoring_model": "genotype_lookup", "evidence_level": ""}
```

---

## 4. `snp-panel.csv`

| Property | Value |
|----------|-------|
| **Format** | CSV (with header row) |
| **Row count** | 2191 |
| **Columns** | `rsid`, `genes`, `trait_count`, `trait_ids`, `trait_names` |
| **Contains rsIDs** | ✅ Yes — `rsid` column (one per row) |
| **Contains gene symbols** | ✅ Yes — `genes` column (often empty; populated for some rsIDs) |
| **Contains chromosome/position** | ❌ No |
| **Contains alleles/genotypes/risk alleles** | ❌ No |
| **Contains disease/trait names** | ✅ Yes — `trait_names` column (semicolon-separated if multiple) |
| **Derived from PDF or independently structured** | Derived from PDF — built from the union of all SNPs across `trait-catalog.json` traits, which is derived from the PDF. |

### Representative examples

**Example 1**:
```json
{"rsid": "rs10004", "genes": "", "trait_count": "1", "trait_ids": "wg-1971", "trait_names": "H. pylori Infection"}
```

**Example 2**:
```json
{"rsid": "rs1000597", "genes": "", "trait_count": "1", "trait_ids": "wg-75", "trait_names": "UPDATE Renal Calculi"}
```

**Example 3**:
```json
{"rsid": "rs10012946", "genes": "", "trait_count": "1", "trait_ids": "wg-66", "trait_names": "Type 2 Diabetes (T2D)"}
```

---

## 5. `snp-panel.txt`

| Property | Value |
|----------|-------|
| **Format** | Plain text (one rsID per line) |
| **Line count** | 2191 |
| **Contains rsIDs** | ✅ Yes — entire file is a list of rsIDs |
| **Contains gene symbols** | ❌ No |
| **Contains chromosome/position** | ❌ No |
| **Contains alleles/genotypes/risk alleles** | ❌ No |
| **Contains disease/trait names** | ❌ No |
| **Derived from PDF or independently structured** | Derived from PDF — extracted from the union of all SNPs in `trait-catalog.json` / `snp-extract-cache.json`, both derived from the PDF. |

### Representative examples

**First 5 lines**:
```
rs10004
rs1000597
rs10012946
rs10033464
rs1004787
```

**Last 5 lines**:
```
rs9959497
rs997154
rs99726
rs9972653
rs9982601
```

---

## 6. `trait-catalog.md`

| Property | Value |
|----------|-------|
| **Format** | Markdown |
| **Records** | 1 document (summary) |
| **Contains rsIDs** | ✅ Yes — in examples (e.g., `rs4988235`, `rs762551`) |
| **Contains gene symbols** | ✅ Yes — in examples (e.g., `MCM6`, `CYP1A2`) |
| **Contains chromosome/position** | ❌ No |
| **Contains alleles/genotypes/risk alleles** | ✅ Yes — in examples (e.g., `GG`, `CC`, `AA`) |
| **Contains disease/trait names** | ✅ Yes — in examples (e.g., "Lactose Metabolism", "Caffeine Metabolism") |
| **Derived from PDF or independently structured** | Derived from PDF — human-readable summary of the catalog generated from the same pipeline. |

### Full content

```
# WeGene Trait Catalog + SNP Panel

Generated: 2026-07-10T10:31:18.695046+00:00
Total traits: **696**
Traits with SNPs: **672**
Unique rsIDs (lab panel): **2191**
Traits without SNPs: **16** (empty/broken PDF pages)

## Files for testing
- `snp-panel.txt` — list of unique rsIDs to genotype
- `snp-panel.csv` — rsID → genes / linked traits
- `trait-catalog.json` — each trait has `assay.snps[]` with rsid, gene, example genotype
- `trait-catalog.csv` — same in spreadsheet form

## Examples
- **Lactose Metabolism**: rs4988235 (MCM6, GG), rs182549 (MCM6, CC)
- **Caffeine Metabolism**: rs762551 (CYP1A2, AA)
- **Iron Nutritional Requirement**: rs12718 (IKZF1, TT), rs18009 (HNF4A, CC), rs4820268 (?), rs855791 (?)
```

---

## Cross-file relationships

```
wegene-all-reports-full.pdf (91.78 MB, not parsed)
    │
    ├──► snp-extract-cache.json (696 entries, one per PDF page)
    │       └─ snps[]: rsid, gene, genotype
    │
    ├──► trait-catalog.json (master catalog)
    │       ├─ traits[696]: id, source, category, name, result, assay.snps[], report_copy
    │       ├─ categories[14]: id, name, name_ru, description, trait_count, traits[]
    │       ├─ snp_panel.rsids[2191]: flat list of all rsIDs
    │       └─ stats: aggregate counts
    │
    ├──► trait-catalog.csv (696 rows, spreadsheet export of traits)
    │       └─ snps, genes, genotypes as semicolon-separated strings
    │
    ├──► snp-panel.csv (2191 rows, rsID → traits mapping)
    │       └─ rsid, genes, trait_count, trait_ids, trait_names
    │
    ├──► snp-panel.txt (2191 lines, flat rsID list)
    │
    └──► trait-catalog.md (human-readable summary)
```

### Key observations

1. **No chromosome/position data** exists in any of the structured files. All genomic loci are identified by rsID only.
2. **Genotypes are example genotypes** — they represent the individual's result from the WeGene report, not reference/risk alleles.
3. **16 traits have no SNPs** — these correspond to "empty/broken PDF pages" (ancestry, haplogroup, and similar non-SNP traits).
4. **`snp-extract-cache.json` is the raw extraction** from individual PDF pages, while `trait-catalog.json` is the curated/enriched version with category assignments, outcome types, and recommendations.
5. **Gene symbols are often missing** in `snp-panel.csv` (the `genes` column is frequently empty), but are populated in `trait-catalog.json` and `snp-extract-cache.json`.
6. **`report_copy` fields are all null** — summary, interpretation, and action templates were not populated.
7. **`evidence_level` and `references` are null/empty** for all traits — no literature references are included.