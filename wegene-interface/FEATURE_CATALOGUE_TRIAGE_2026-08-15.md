# P25AJ3 Feature Catalogue — triage of "Not available" / "Needs rework" entries (2026-08-15)

737 features total. Orchestration: Claude Code (this session) wrote scoped
per-feature investigation tasks, ran each through `aider` (glm-5.2 backend,
full local file read access, no internet), then independently verified every
non-trivial verdict against the actual local data before accepting it —
never trusted aider's written summary alone. Two of aider's verdicts were
caught wrong on verification (see below) and corrected.

## Scope

`exact_next_action` across all 737 features pre-triages resolvability:

| next action | count | locally resolvable? |
|---|---|---|
| Wait for evidence qualification | 644 | No — needs new external literature |
| No action needed | 25 | n/a |
| Retrieve full text for model extraction | 59 | No — needs fetching a paper |
| Reconstruct or substitute model / Implement rule / Find evidence-endpoint / Find genotype data | 9 | Checked all 9 (see below) |

**703 of 737 are blocked on external data retrieval this session cannot
perform** (no internet access from either the glm.js harness or aider —
aider adds local file read/write, not web access). Running those through
aider would just produce 703 copies of "cannot resolve, no local source."

## Data-provenance check (before trusting any of it)

There's a separate, earlier pipeline stage
(`P4_MULTISOURCE_300_500_TRAIT_PORTFOLIO...`, dated 2026-07-30) using a
different ID scheme (`wg-1548` vs `SF-0071`). Verified via aider + a manual
cross-check (module assignment for Mouth Ulcers matches exactly:
`qualified_medical_risk` in both) that **P4 is the upstream stage that fed
into the 737-feature interface catalogue, not a competing/conflicting
source.** The interface's data is the current, downstream state — safe to
treat as authoritative.

## The 9 locally-checkable entries — results

| Feature | Verdict | Note |
|---|---|---|
| **SF-0001 Ancestry Analysis** | Status confirmed correct | Whole-genome admixture model (verified against the actual PDF report page: "My Ancestry Composition... 64.82% European..."), not a missing-marker problem — cannot reduce to a marker list. |
| **SF-0013 Iron Nutritional Requirement** | Status confirmed correct | Zero evidence/references at any local pipeline stage. Genuinely needs external retrieval. |
| **SF-0071 Mouth Ulcers** | **Inconclusive — flagged, not resolved** | Aider proposed a "data-linking bug" (full text supposedly retrieved but never linked). I searched the whole project for the actual retrieved full-text and found nothing — only marker/crosswalk tables. Not accepting that verdict as confirmed; leaving as-is pending someone finding the actual source. |
| **SF-0095 Stomach Cancer** | Status confirmed correct | The one local PDF page found is about esophageal cancer, a different GI cancer — correctly flagged as a phenotype mismatch, not usable evidence. |
| **SF-0148 Ulcerative Colitis** | **CORRECTED (see addendum below)** | Originally flagged as a phenotype mismatch (evidence only covered asthma). **This was wrong** — live retrieval of the actual PMID 22694930 abstract (see addendum) shows the same paper explicitly states rs2395185's T allele is protective for ulcerative colitis specifically, not just a risk allele for asthma. The interface's evidence table only captured the asthma-direction sentence, not the UC-specific one from the same source. See addendum for corrected disposition. |
| **SF-0149 Vascular Dementia** | Status confirmed correct | Real proprietary output exists (0.40x relative risk) but underlying per-SNP weights aren't disclosed anywhere locally — genuinely needs external data. rs429358 (APOE) here is medically appropriate (this is itself a dementia-risk trait), unlike its wrongful use in unrelated food-preference traits found earlier this session. |
| **SF-0716 Tanning Response** | Status confirmed correct | `SCIENTIFIC_HOLD` is working as intended — crosswalk correctly flags that its markers are shared with Melanoma and Astigmatism, and interpretation doesn't transfer automatically. |
| **SF-0717 Sun Spots (Lentigines)** | Status confirmed correct | Same as above, `SCIENTIFIC_HOLD` appropriate even though its overlapping traits are more closely related (both skin/sun-response). |
| **SF-0728 Glycation Protection** | **Real bug — fix recommended** | Aider's first-pass verdict ("CORRECT/PROCEED", claiming independently validated AGER/GLO1 evidence) was **wrong and I did not accept it** — it assumed evidence existed without ever being given any. I checked directly: **zero evidence rows locally**, and the feature's own `evidence_status` field still says `MODEL_RECONSTRUCTION_REQUIRED`, directly contradicting `rule_work_status: RULE_READY`. Same bug pattern as SF-0148. One of its 5 markers (rs2070600) is also used for the unresolved Stomach Cancer trait. Recommend downgrading, not implementing. |

## Net result

- 6 of 9 confirmed correct as-is (no action needed)
- 1 inconclusive, deliberately left unresolved rather than accepting an unverified theory
- **2 real bugs found**: SF-0148 and SF-0728 are both marked `RULE_READY` /
  ready-for-production while their own underlying evidence contradicts that
  status. Same failure pattern in both cases — status labels advanced
  without reconciling the evidence-level adjudication underneath them.

## CORRECTION (2026-08-15, same day) — aider DOES have real internet access

Everything above about "703 features blocked, no internet access available"
**was wrong** and has been superseded. The user correctly pushed back twice
on this claim. Verified empirically: aider has a built-in URL-auto-fetch
feature (prompts "Add URL to the chat?", auto-confirmed with
`--yes-always`) that performs a real, live HTTP fetch of any URL mentioned
in a task — this was confirmed by fetching the live PubMed page for
PMID 22694930 and getting back its actual, real abstract text (not
fabricated — contains real p-values, cohort names, and a specific sentence
never supplied in the prompt).

**That live fetch changed the SF-0148 verdict.** The real PMID 22694930
abstract states: *"The minor allele T of rs2395185 in HLA-DRA is the risk
allele for asthma but the protective allele for ulcerative colitis."* This
is genuine, on-topic, ulcerative-colitis-specific evidence in the exact
paper already cited by the interface's evidence table — the table just
never captured this sentence, only the asthma-direction one from the same
paper. So the original finding ("phenotype mismatch, no real UC evidence,
downgrade the feature") is **wrong on that specific point.**

**Corrected disposition for SF-0148:** not "downgrade/exclude" — instead,
the evidence-extraction step for this marker needs to be redone to capture
the UC-specific sentence (direction: **protective**, opposite sign from the
asthma association) alongside the existing asthma-direction entry, and the
`HOLD_MODEL_UNRESOLVED` adjudication should be revisited now that complete
evidence is available. The other 7 markers on this feature still have zero
cited evidence and would need the same live-retrieval treatment.

**This also means the "703 blocked, needs external retrieval" framing for
the rest of the 737-feature backlog is no longer accurate as an excuse to
stop.** Real literature retrieval is possible with this toolset. Scoping
how far to take that is a decision for the user, not something to charge
ahead on unilaterally given how large the backlog is.

## SF-0148 fully redone with real GWAS Catalog data (all 8 markers)

Fetched the live GWAS Catalog variant page for every one of SF-0148's 8
markers (`https://www.ebi.ac.uk/gwas/variants/<rsid>`). Result: **7 of 8
markers have genuine, directly relevant Ulcerative Colitis GWAS evidence**,
not the "1 of 8, and that one's mismatched" picture the interface's
internal evidence table showed.

| Marker | Real UC evidence found | Detail |
|---|---|---|
| rs2395185 | Yes | PMID 22694930: T allele **protective** for UC (was previously only shown as an asthma-risk allele — see correction above) |
| rs9858542 | Yes | P=7×10⁻⁹, study GCST000527; also Crohn's disease |
| rs16940186 | Yes | P=4×10⁻¹⁰, study GCST001938; also general IBD |
| rs9271366 (HLA-DQA1/HLA-DRB1) | Yes | P=2×10⁻⁷⁰, study GCST001118, combined UC/Crohn's association |
| rs1801274 (FCGR2A) | Yes | P=2×10⁻²⁰ (GCST000964) and P=2×10⁻¹² (GCST000529); also general IBD |
| rs4654903 | Yes | P=7×10⁻⁹, study GCST001938, OR=1.56 |
| rs10883365 | **No direct UC hit** | Has a Crohn's disease association (P=4×10⁻¹⁰) instead — a related but distinct IBD subtype, not UC itself |
| rs11209026 (IL23R) | Yes | 3 separate UC associations, plus Crohn's/IBD/ankylosing spondylitis/psoriasis — this is the well-known IL23R Arg381Gln pleiotropic immune variant |

**Corrected final verdict: SF-0148 "Ulcerative Colitis" is well-supported.**
The original `RULE_READY` status looks substantially more justified than my
first pass concluded — the problem was never the underlying science, it was
that the interface's internal evidence table only captured 1 of 8 markers
and even that one was incompletely extracted. The actual fix needed is
narrower than "downgrade the feature": backfill the evidence table with
these 7 real citations, and flag rs10883365 specifically (Crohn's-only
evidence, not UC-specific — worth a decision on whether to keep it as a
supporting marker or drop it from this particular feature).

**Lesson for the rest of the backlog:** an empty/thin internal evidence
table is not proof that evidence doesn't exist — for SF-0148 it existed and
was simply never retrieved. This changes how much weight to put on
"MODEL_RECONSTRUCTION_REQUIRED" / "zero evidence rows" as a signal for the
other 737 features; some unknown fraction of the 703 "blocked" ones may be
in the same situation as SF-0148 was.
