# ADR-002: Data acquisition notes and Phase 0 corrections

- Status: accepted (records facts; no analytic deviation, since no preregistration exists yet)
- Date: 2026-11-14
- Phase: 0

## Notes
1. Branch main was created once through the GitHub web interface (initial README) before cloning, because an empty repository has no base branch for a pull request. No other commit was made to main.
2. The DepMap catalogue endpoint returned HTTP 403 to automated requests; files were downloaded manually from the portal and copied from Google Drive to the git-ignored data/raw.
3. Local renames, bytes unchanged: 'Model .csv' -> Model.csv (sha256 verified); 'README (1).txt' -> README.txt in the 24Q2 folder; identical duplicate 'README (2).txt' removed locally.
4. Audit correction: the first PRISM 24Q2 presence check (Cell G) matched on broad_ids derived from an empty name match, so its zero was uninformative. It was redone with the BRD-K12343256 prefix (Cell H); result still zero.
5. Alias IDs: two PRISM 20q2 ids map via ModelIDAlias to models that already have direct ids (see data_contracts section 5).
6. Known data issues recorded without exclusion decisions: 8 trametinib rows without depmap_id; 7 lines whose only rows failed STR profiling; README "convergence" column absent from the file.
7. No response value was read, summarised or plotted in Phase 0; only counts of missing values.
8. Colab runtime restarted twice during Phase 0. In one session the raw files were copied before the skeleton (including .gitignore) was written, so git status listed them as untracked; no commit was made at that time. The working tree was rebuilt in the order: clone, skeleton, copy from Drive, local renames, git-ignore assertion. Manifest hashes were checked against the hashes of the first audit (all matched).
9. MPAS definition recorded from the paper's Methods (docs/mpas_definition.md). A pasted summary that described the formula as a mean of z-scores was corrected: the paper divides the sum of z-scores by sqrt(10), not by 10.
10. Expression-only MPAS descriptives (Cell Q) were computed with two z-score reference sets for description only; no choice is made here. Lineage explains about 53% of MPAS variance among the 712 eligible lines.
