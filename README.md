# mapk-drug-response

Question: does the published MAPK Pathway Activity Score (MPAS; Wagle et al. 2018,
npj Precision Oncology, doi 10.1038/s41698-018-0051-4) add out-of-sample predictive
value for MEK-inhibitor (trametinib) sensitivity in cancer cell lines beyond
mutation + lineage and a broader expression baseline?

Data: public DepMap / PRISM (GDSC / CTRP only if chosen in Phase 0/1).
The hypothesis comes from the published MPAS paper, not from any earlier analysis
by the author (see docs/history.md).

Phases: 0 repo + data audit; 1 preregistration + power simulation;
2 implementation + synthetic tests; 3 gated controls run; 4 main analysis;
5 sensitivity, exploratory, final report.

Rules: see AI_RULES.md. Current status: see PROJECT_STATE.md.
