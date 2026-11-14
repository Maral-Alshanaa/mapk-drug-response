# ADR-001: Candidate drug-response datasets and measures for trametinib

- Status: proposed (no selection made)
- Date: 2026-11-14
- Phase: 0

## Context
Phase 0 audit found trametinib under these identifiers: DPC-006587 (PortalCompounds); BRD-K12343256-001-08-9 and -14-7; CTRP:687418; GDSC1:1372; GDSC2:1372.
The final choice of source and measure belongs to Phase 1 and must be fixed in the preregistration before any outcome is seen.

## Candidates
| Candidate | Measure(s) | Status in hand |
|---|---|---|
| PRISM 20q2 secondary, screen MTS010 | auc, ic50, ec50 | downloaded; 667 depmap_id (665 in Model.csv direct); README recommends MTS010 when available |
| PRISM 20q2 secondary, screen HTS002 | auc, ic50, ec50 | downloaded; 466 depmap_id (+8 rows without id) |
| PRISM Public 24Q2 | LFC (single dose 2.5 uM, LFC_cb median-collapsed) | downloaded; no trametinib found by name or BRD prefix (see data_contracts caveat) |
| PRISM 20q2 primary (HTS, 2.5 uM) | replicate-collapsed LFC | treatment info only; LFC matrix not downloaded |
| GDSC1 / GDSC2 / CTRP | IC50 / AUC (definitions to be read from the sources) | not downloaded; official sources not yet searched |

## Measure considerations (from the audit, not decisions)
- auc and ec50 have no missing rows; ic50 is missing in 82/474 (HTS002) and 204/687 (MTS010) rows. Reason for missing ic50 is not documented in the README and was not investigated; complete-case use of ic50 reduces the analysable set from 711 to 557 lines.
- MTS010 and HTS002 overlap in 413 depmap_id and differ in lineage composition (Lymphoid and Myeloid appear only in MTS010).
- Replicate handling (PR300/PR500, alias duplicates) must be fixed before analysis.

## Decision
None in Phase 0. Phase 1 will choose source, screen, measure and handling rules, with criteria stated before any outcome is seen.
