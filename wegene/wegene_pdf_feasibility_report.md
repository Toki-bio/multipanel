# WeGene PDF Feasibility Report

Generated: 2026-07-10
PDF: `C:\work\wegene\wegene-all-reports-full.pdf`

---

## 1. Page Count

**4206 pages**

---

## 2. PDF Metadata

| Key | Value |
|-----|-------|
| /Producer | pdf-lib (https://github.com/Hopding/pdf-lib) |
| /ModDate | D:20260709141609Z |
| /Creator | pdf-lib (https://github.com/Hopding/pdf-lib) |
| /CreationDate | D:20260709141609Z |


---

## 3. Embedded Text Extraction

**Text extractable: ✅ Yes**

Library used: `pypdf` (the only available PDF library)

| Library | Status |
|---------|--------|
| PyPDF2 | ❌ Missing |
| pypdf | ✅ Available |
| pdfplumber | ❌ Missing |
| fitz (PyMuPDF) | ❌ Missing |
| pdfminer | ❌ Missing |

**Note:** `pdfplumber` and `fitz` (PyMuPDF) would provide better table extraction
capabilities. If table extraction is needed, installing one of these is recommended.

---

## 4. Chinese Text Extractability

**Chinese text extractable: ❌ No**

No Chinese characters detected in sampled pages. The PDF may use
embedded images for Chinese text, or Chinese text may use a non-standard
font encoding that pypdf cannot decode.


---

## 5. First 2 Pages — Text Sample

### Page 1 (first 500 characters)
```
Your Genes, Your Savings: Save up to 25% on genetic test kits.
Lactose Metabolism
Lactose only exists in mammalian milk in nature, which is very important for
children's intellectual development. Lactose metabolism is closely related to
lactose intolerance, a condition in which the body does not produce lactase
that breaks down lactose.
My Lactose Metabolism
Weak
Your  is weak. After
drinking a lot of milk on an empty
stomach,  such as
gastrointestinal discomfort may occur
There is a mutation in
```

### Page 2 (first 500 characters)
```
Not suitable 
Some people with lactose intolerance
drink 200 ml of milk a day without any
discomfort, and this ability can also be
trained
Phosphomannomutase 2-Congenital Disorder of Gly
Calcium Nutritional Requirement (report2/detail/247
Cow's Milk (report2/detail/2258)
The symptoms of gastrointestinal discomfort
caused by drinking milk and eating dairy
products are mainly due to the lack of lactase
in the intestine that can decompose lactose.
The undecomposed lactose is continuously
fermented 
```

---

## 6. Table Extractability

**Table-like structures detected: 1 indicators found**

Table-like patterns (tab-separated fields, pipe-delimiters, or key-value pairs)
were detected in the sampled pages. However, `pypdf` has limited table
extraction capabilities. For reliable table extraction, `pdfplumber` or
`fitz` (PyMuPDF) would be needed.


---

## 7. Information Missing from Structured Files — Present in PDF?

The following fields are missing from the WeGene structured files
(`trait-catalog.json`, `snp-panel.csv`, etc.). This check samples pages
across the PDF to determine if this information appears to be present
in the PDF text.

| Field | Present in PDF? | Evidence |
|-------|-----------------|----------|
| Risk alleles | ❌ Not detected | Searched for: risk, 风险, 突变, mutant, alternate, alt allele |
| Clinical explanations | ✅ Yes | Searched for: clinical, 临床, explanation, 解释, significance, 意义 |
| References | ✅ Yes | Searched for: reference, 参考, 文献, citation, PMID, doi, http |
| Disease/trait descriptions | ✅ Yes | Searched for: disease, 疾病, trait, 性状, syndrome, 综合征 |
| Scoring models | ✅ Yes | Searched for: scoring, 评分, model, 模型, polygenic, 多基因 |
| Chromosome/position | ✅ Yes | Searched for: chromosome, 染色体, chr, position, 位置, bp |
| Population frequency | ✅ Yes | Searched for: frequency, 频率, MAF, minor allele, 人群, population |
| Report interpretation text | ✅ Yes | Searched for: interpretation, 解读, result, 结果, conclusion, 建议 |
| Genotype data | ✅ Yes | Searched for: genotype, 基因型, GG, AA, CC, TT |
| rsIDs | ✅ Yes | Searched for: rs\d+ pattern |
| Gene symbols | ❌ Not detected | Searched for: gene/基因 + symbol pattern |

---

## 8. Sampling Methodology

- Pages sampled: first 2 pages + every Nth page (up to 20 additional pages)
- N = max(1, page_count // 20)
- Text extraction: `pypdf.PdfReader.pages[i].extract_text()`
- Pattern matching: regex on combined sampled text
- **Limitation:** Only a subset of pages was sampled. Content patterns may
  be present on unsampled pages but not detected. A full extraction would
  be needed for definitive answers.

---

## 9. Conclusions and Recommendations

### Feasibility Summary

| Check | Result |
|-------|--------|
| PDF library available | ✅ `pypdf` only |
| Page count | 4206 |
| Embedded text | ✅ Extractable |
| Chinese text | ❌ Not extractable |
| Table extraction | ⚠️ Limited (pypdf only; pdfplumber/fitz recommended) |
| Risk alleles in PDF | ❌ Not detected in sample |
| Clinical explanations | ✅ Likely present |
| References | ✅ Likely present |
| Chromosome/position | ✅ Likely present |
| Population frequency | ✅ Likely present |

### Recommendations

1. **For full text extraction**: `pypdf` can extract text page-by-page, but
   Chinese text extraction may be unreliable.
   Consider installing `pdfplumber` or `fitz` for better results.

2. **For table extraction**: `pypdf` has no native table extraction. Install
   `pdfplumber` (best for tables) or `fitz` (PyMuPDF) if structured table
   extraction is needed.

3. **For missing fields**: The PDF appears to contain
   additional information not captured in the structured files. A full
   extraction and parsing pass would be needed to confirm and capture this data.

4. **For chromosome/position data**: Detected in PDF sample — full extraction may recover these fields.

5. **For risk alleles**: Not detected in sampled pages. May require external annotation (GWAS Catalog/ClinVar) instead.
