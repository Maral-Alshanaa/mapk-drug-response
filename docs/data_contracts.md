# Data contracts

Status: Phase 0 draft (2026-11-14 06:01 UTC). Every statement comes from descriptive audit output (no association,
no response values). Nothing here is an analysis decision. File hashes: data/manifest.csv.

## 1. Sources (manual downloads)
- DepMap Public 26Q3 (announced 2026-10-05). Primary Files: Model, OmicsProfiles, OmicsExpressionTPMLogp1HumanProteinCodingGenes,
  OmicsSomaticMutationsMatrixHotspot / Damaging, OmicsInferredMolecularSubtypes; Supplemental: PortalCompounds; plus README.
- PRISM Repurposing Public 24Q2 (Cell_Line_Meta_Data, Treatment_Meta_Data, LFC, LFC_COLLAPSED, README).
- PRISM Repurposing 20q2 (primary-screen replicate-collapsed treatment info; secondary-screen cell-line info,
  dose-response curve parameters, README). The primary-screen LFC matrix is NOT downloaded.
- GDSC1 / GDSC2 / CTRP response files: NOT downloaded (only registered IDs in PortalCompounds).

## 2. DepMap 26Q3 files
| File | Rows x cols | Key / notes |
|---|---|---|
| Model.csv | 2,554 x 48 | ModelID (ACH-*) unique. OncotreeLineage null in 26; CCLEName null in 432; ModelIDAlias non-null in 93 |
| OmicsProfiles.csv | 6,121 x 18 | ProfileID; not used so far |
| OmicsExpressionTPMLogp1HumanProteinCodingGenes.csv | 2,314 x 19,224 | meta columns: Unnamed: 0, SequencingID, ModelConditionID, ModelID, IsDefaultEntryForMC, IsDefaultEntryForModel; 19,218 gene columns named "SYMBOL (EntrezID)" |
| OmicsSomaticMutationsMatrixHotspot.csv | 3,807 x 171 | 165 gene columns; values 0/1/2 |
| OmicsSomaticMutationsMatrixDamaging.csv | 3,807 x 19,738 | 19,657 "SYMBOL (EntrezID)" columns + 75 columns named "SYMBOL (Unknown)"; values 0/1/2 |
| OmicsInferredMolecularSubtypes.csv | 2,476 x 58 | derived calls (e.g. BRAF p.V600E); comparison only |
| PortalCompounds.csv | 7,129 x 12 | TRAMETINIB = DPC-006587 |

Row-level facts (descriptive):
- IsDefaultEntryForModel == "Yes" gives exactly one row per ModelID in all three matrices:
  expression 2,130 models (184 "No" rows); hotspot and damaging 2,395 models each (1,412 "No" rows). All ModelIDs exist in Model.csv.
- Expression: no NaN, no negative, no infinite values; range 0 to 18.19; 18.39% exactly zero; 5 genes with zero variance.
- Mutation matrices: no NaN. 0 = none; 1 = sum of allele fractions <= 0.95; 2 = > 0.95 (README definition).
- All of BRAF, NRAS, KRAS, HRAS, NF1, MAP2K1 have exactly one column in both mutation matrices.

## 3. Trametinib identifiers
- PortalCompounds: CompoundName TRAMETINIB; synonyms GSK1120212, MEKINIST; targets MAP2K1;MAP2K2;
  SampleIDs: BRD:BRD-K12343256-001-08-9, BRD:BRD-K12343256-001-14-7, CTRP:687418, GDSC1:1372, GDSC2:1372.
- PRISM 24Q2: no row matching the name or the BRD-K12343256 prefix in Treatment_Meta_Data or LFC_COLLAPSED
  (caveat: 386 rows have null broad_id and 410 null name in Treatment_Meta_Data). Absence of evidence, not proof.
- PRISM 20q2 primary treatment info: trametinib, BRD-K12343256-001-08-9, dose 2.5, screen HTS (LFC matrix not downloaded).

## 4. Response data in hand: PRISM 20q2 secondary, dose-response curve parameters (753,778 x 20)
| screen_id | broad_id | rows | distinct depmap_id | null depmap_id |
|---|---|---|---|---|
| HTS002 | BRD-K12343256-001-08-9 | 474 | 466 | 8 |
| MTS010 | BRD-K12343256-001-14-7 | 687 | 667 | 0 |

- Measures: auc, ic50, ec50 (all float64). NaN: auc 0, ec50 0; ic50 82/474 (HTS002) and 204/687 (MTS010).
- README states MTS010 is recommended when available (770 cell lines, 192 oncology compounds). README lists a "convergence"
  column that is NOT present in the file.
- MTS010: 647 depmap_id with one row, 20 with two rows (row_name PR300_* and PR500_*). 413 depmap_id are shared between HTS002 and MTS010.
- passed_str_profiling == False: 8 rows (HTS002), 9 rows (MTS010).

## 5. ID mapping (descriptive; final rules in prereg)
- 720 distinct depmap_id -> 718 ModelID: 718 direct matches; 2 via ModelIDAlias (ACH-000010 -> ACH-000015, ACH-001078 -> ACH-000007),
  both targets already present under their direct ID, so mapping adds no new model but adds duplicate MTS010 rows.
- After mapping, rows per (ModelID, screen): 1,109 groups with 1 row, 22 groups with 2 rows (20 PR300/PR500 + 2 alias). 8 rows have no depmap_id.
- PRISM 24Q2 uses ACH-* ids inside row_id ("ACH-...::pool::culture::screen"); its cell-line meta has depmap_id and ccle_name.

## 6. Counts: lines with a trametinib row, mapped to ModelID, with default-entry expression and both mutation matrices
- ModelIDs with a row: 718. With expression: 712. With hotspot / damaging: 715 each. With all three: 711.
- Lines with a finite value AND all three omics (default entries): auc = ec50 = 711 (HTS002 464, MTS010 658); ic50 = 557 (HTS002 386, MTS010 465).
- 7 lines have an auc value but no row with passed_str_profiling True.

| OncotreeLineage | auc / ec50 | ic50 |
|---|---|---|
| Lung | 116 | 90 |
| Lymphoid | 61 | 35 |
| CNS/Brain | 55 | 44 |
| Esophagus/Stomach | 50 | 46 |
| Skin | 44 | 44 |
| Bowel | 37 | 33 |
| Ovary/Fallopian Tube | 36 | 30 |
| Pancreas | 36 | 35 |
| Breast | 31 | 18 |
| Myeloid | 31 | 24 |
| Head and Neck | 29 | 28 |
| Uterus | 27 | 17 |
| Kidney | 25 | 17 |
| Bladder/Urinary Tract | 24 | 23 |
| Bone | 24 | 10 |
| Liver | 21 | 19 |
| Soft Tissue | 19 | 15 |
| Peripheral Nervous System | 13 | 9 |
| Pleura | 10 | 5 |
| Thyroid | 9 | 7 |
| Biliary Tract | 6 | 5 |
| Prostate | 4 | 2 |
| Ampulla of Vater | 1 | 1 |
| Cervix | 1 | 0 |
| Fibroblast | 1 | 0 |

## 7. Not yet verified / pending
- (resolved: see section 8 and docs/mpas_definition.md)
- Official sources and file names for GDSC1/GDSC2/CTRP response data.
- Dose range / definition of auc in each PRISM 20q2 secondary screen.
- Exact download URLs and dates of the manual downloads.

## 8. MPAS (expression only; no response data involved)
- Definition: docs/mpas_definition.md. All ten genes have exactly one column each in the expression matrix:
  PHLDA1 (22822), SPRY2 (10253), SPRY4 (81848), DUSP4 (1846), DUSP6 (1848), CCND1 (595), EPHA2 (1969), EPHA4 (2043), ETV4 (2118), ETV5 (2119).
  Minimum per-gene sd over the 2,130 default-entry lines: 1.56.
- Eligible lines (a trametinib row in 20q2 secondary, mapped to ModelID, with default-entry expression): 712.
- MPAS = sum(z)/sqrt(10) on the 712 lines. Reference = all 2,130 default lines (A): mean 0.154, sd 1.953, min -5.625, median 0.377, max 4.636.
  Reference = the 712 lines (B): mean 0.000, sd 2.034. Spearman(A, B) = 0.9999.
- Lineage structure: eta^2 = 0.531 (A) / 0.529 (B); pooled within-lineage sd 1.36 (A) / 1.42 (B) versus overall 1.953 / 2.034.
  Lineage means (A): Skin 2.989 (sd 0.798, n 44), Pancreas 1.620 (0.959, 36), Lymphoid -3.138 (1.355, 61), Myeloid -2.267 (1.404, 31).
  Largest sd (A): Breast 2.079 (n 31), Peripheral Nervous System 1.731 (13), Lung 1.675 (116).
- Lineages with n < 5 among the 712: Prostate 4, Fibroblast 2, Cervix 1, Ampulla of Vater 1.
