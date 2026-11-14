
# MPAS definition (as published)

Source: Wagle et al. 2018, npj Precision Oncology 2:7, doi 10.1038/s41698-018-0051-4 (PMC5871852), Methods section "MAPK Pathway Activity Score".
Verified against the full text on 2026-11-14 06:31 UTC.

Genes (10): PHLDA1, SPRY2, SPRY4, DUSP4, DUSP6, CCND1, EPHA2, EPHA4, ETV4, ETV5.
The Results paragraph of the paper lists only nine of these names (PHLDA1 omitted) while stating ten; the Methods list has ten.

Formula (as written in the paper): MPAS = (sum over the 10 genes of z_i) / sqrt(n), with z_i the z-score of gene i's expression and n = 10.
This is NOT the mean of the z-scores (that would divide by n); it is the sum scaled by sqrt(10).

Not specified by the paper (to be fixed in the preregistration, not in Phase 0):
- the reference sample set for the z-scores (all samples vs within lineage); the paper says RNA-seq data were normalized across samples;
- the standard deviation convention (ddof).

Platform note: the paper used RNA-seq (reads per counts per million) or a Nanostring panel; the DepMap 26Q3 matrix is log2(TPM+1).
The paper states that absolute MPAS values depend on assay and normalization and should be read relative to other samples in the same context; no absolute threshold is transferred.

DepMap column names: "SYMBOL (EntrezID)", one column per gene (verified by Cell Q before use).
