
# Handoff: Phase 0 (repo bootstrap and data audit)

Written: 2026-11-14 06:40 UTC. Branch: phase0-bootstrap. Checkpoint commit 5b0cc58 (pushed earlier); the final Phase 0 commit is the head of the branch at PR time (a commit cannot contain its own hash; see git log). Tag: none yet (to be created after the PR is merged).

## What was done
- Repository skeleton, AI_RULES.md (verbatim rules), git-ignore for data/ with data/manifest.csv tracked, environment record.
- Manual download of DepMap Public 26Q3 files and PRISM Repurposing files (24Q2 and 20q2); sha256 for every raw file (table below).
- Descriptive audit only: shapes, ID columns, ID mapping, lineage counts, availability counts, trametinib identifiers, MPAS gene presence and expression-only MPAS descriptives. No association between MPAS/MAPK genes/mutations and any drug response was computed.
- docs/data_contracts.md, docs/mpas_definition.md, docs/history.md (author's disclosure), ADR-001 (candidates, no choice), ADR-002 (acquisition notes and corrections).

## Raw data (sha256 from data/manifest.csv)
| file | bytes | sha256 |
|---|---|---|
| depmap_26Q3/Model.csv | 789,805 | 65eb1895800e71a963a3ab56e5d08c8d006cb0d9e7aa68f7e34a3810a2a3cbc7 |
| depmap_26Q3/OmicsExpressionTPMLogp1HumanProteinCodingGenes.csv | 394,997,160 | 252d1fedc7014953b19ffe5d68151b4df973c8078b12c9e5ccd97e292548d177 |
| depmap_26Q3/OmicsInferredMolecularSubtypes.csv | 703,344 | 71d52a97a5eb93d16a5b45b255219734e148b5ea58d210ff3da6b02d5a2b0659 |
| depmap_26Q3/OmicsProfiles.csv | 882,568 | 4241e537f0b23139c0e525b4d78fb53ee65c5507771a33e2cd5debd78f7179ea |
| depmap_26Q3/OmicsSomaticMutationsMatrixDamaging.csv | 300,957,482 | 736b7a82b4d1f234dc597a409d104186c282f01b0caa56365c27ff987ae49e20 |
| depmap_26Q3/OmicsSomaticMutationsMatrixHotspot.csv | 2,701,956 | 0cbdacdce2355d2dd600d3741a36ae093dbe77d7bee44bde97411b1e96973058 |
| depmap_26Q3/PortalCompounds.csv | 772,558 | a5fdcf7774da39b6f7af187a599051713bb9fb492061ff23945692a6a1e27b88 |
| depmap_26Q3/README.txt | 48,405 | 5672d9e04f18320e9ca1d25747427275dcabb44bffe45eba34d196f1b44dd3ec |
| prism_repurposing/public_24Q2/README.txt | 9,889 | 1ae0f2cb4e23903593f10d2182214363392498f2185d2520c06962ffb156f340 |
| prism_repurposing/public_24Q2/Repurposing_Public_24Q2_Cell_Line_Meta_Data.csv | 157,294 | 13a85da9e45656c455d3911a4491f3636d7d0975a9d2333dc3d0a3e2cd3b0526 |
| prism_repurposing/public_24Q2/Repurposing_Public_24Q2_LFC.csv | 479,352,899 | 824149f9b9f3821eb520b385a5976e1a9977d86b21caf5d22171763800a40523 |
| prism_repurposing/public_24Q2/Repurposing_Public_24Q2_LFC_COLLAPSED.csv | 150,056,831 | 4e9cb6933d6cccc32f83e89383ddc79c79edccfc07aacddc64b9a680ce5bde31 |
| prism_repurposing/public_24Q2/Repurposing_Public_24Q2_Treatment_Meta_Data.csv | 1,510,417 | 6be6422ba804ad0775e78b457677bdf088707b9354746a03e110ae63f5eb2061 |
| prism_repurposing/repurposing_20q2/prism-repurposing-20q2-primary-screen-replicate-collapsed-treatment-info.csv | 1,312,286 | f92e58eb04b0db5c35412b08c50446078de153a2f6a5b862cdb89f532ed29bc5 |
| prism_repurposing/repurposing_20q2/prism-repurposing-20q2-secondary-screen-cell-line-info.csv | 84,730 | 682ab554a9089833494b5b67549ccfcd27303c7d53f25e6a43099e066c924c82 |
| prism_repurposing/repurposing_20q2/prism-repurposing-20q2-secondary-screen-dose-response-curve-parameters.csv | 290,170,269 | 2ac69a21f1d681fe7447689262b82ca6e3dc90bfef0bd96eb5479b96f424e43d |
| prism_repurposing/repurposing_20q2/prism-repurposing-20q2-secondary-screen-readme | 8,046 | 652173e1050dc92a7005809545bfc2ce7e491a22af2af3ce0b62447e31d2e851 |

## Key audit facts (all from pasted audit output; details in docs/data_contracts.md)
- Trametinib: DPC-006587 (PortalCompounds); BRD-K12343256-001-08-9 and -14-7; CTRP:687418; GDSC1:1372; GDSC2:1372.
- No trametinib found in PRISM Public 24Q2 (name or BRD prefix); absence of evidence only (386 null broad_id and 410 null name rows in Treatment_Meta_Data).
- Response in hand: PRISM 20q2 secondary curve parameters (auc, ic50, ec50): HTS002 474 rows / 466 depmap_id (+8 rows without id); MTS010 687 rows / 667 depmap_id (20 ids with two rows, PR300 and PR500).
- 720 depmap_id map to 718 ModelID (2 via ModelIDAlias, both onto models that already have a direct id).
- Lines with a finite value and default-entry expression + both mutation matrices: auc = ec50 = 711 (HTS002 464, MTS010 658); ic50 = 557. ic50 missing in 82/474 (HTS002) and 204/687 (MTS010) rows.
- IsDefaultEntryForModel == "Yes" gives exactly one row per ModelID in expression (2,130), hotspot and damaging (2,395 each).
- Expression-only MPAS on the 712 eligible lines: lineage explains about 53% of variance (eta^2 0.531); pooled within-lineage sd 1.36 versus overall 1.953.

## Decisions
- No analytic decision was made. config/data_freeze.yaml records files and hashes only; analysis_choices are null.
- MPAS is implemented as published: sum of per-gene z-scores divided by sqrt(10) (not the mean).

## Deviations
- None from a preregistration (none exists yet). Process notes in ADR-002: main created once through the web interface; two Colab runtime restarts; a corrected 24Q2 presence check; local file renames with unchanged bytes.

## Open items for Phase 1
- Choose response source, screen (MTS010, HTS002 or a rule combining them), measure (auc, ic50, ec50) and the handling of missing ic50, PR300/PR500 duplicates, alias duplicates, the 8 rows without id and the 7 lines whose rows all failed STR profiling.
- Fix the MPAS z-score reference set and sd convention; decide how lineage enters the baseline and how lineages with n < 5 are handled.
- Decide whether to search official GDSC1/GDSC2/CTRP sources (author decision pending).
- Fix mutation and lineage encodings, the MAPK-gene baseline columns, and whether OmicsInferredMolecularSubtypes is used.
- Power simulation should use the real MPAS lineage structure above.
- Record exact download URLs and dates if known.

## Files the next phase needs
AI_RULES.md, config/data_freeze.yaml, data/manifest.csv, docs/data_contracts.md, docs/mpas_definition.md, docs/history.md, docs/decisions/ADR-001 and ADR-002, this handoff, experiment_log.csv; the raw originals on Google Drive (folder mapk-drug-response-raw).

## Do not
- Compute any association between MPAS, any MAPK gene or mutation, and any drug response before Phase 3.
- Choose the response measure or any threshold after seeing outcome data; every choice goes into the preregistration first.
- Treat the absence of trametinib in 24Q2 as proven, or transfer absolute MPAS thresholds from the paper.
- Re-run the old Cell O (it appends to ADR-002); never commit data/raw or data/processed; never push to main.
