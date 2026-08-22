# Results snapshot

Canonical machine-readable export of all numbers reported in the paper
(tables in `tables/`), generated from the LaTeX sources so that the
reported statistics have a durable record independent of evaluation logs.

| File | Contents |
|---|---|
| `t2i_hgr_by_model_category.csv` | Table 4: per-model, per-category HGR (%) at L0-L3 (264 rows) |
| `t2i_boundary_metrics.csv` | Table 3: Boundary Delta (pp), peak transition, SBS per model |
| `defense_by_group.csv` | Table 5: defense methods by group (NSFW/Legal/Social) HGR at L1-L3, SAS, DeltaHGR |
| `filter_eval.csv` | Table 6: filter detection rates by level, FPR@L0, FBR, AUC, sensitivity |
| `attack_asr.csv` | Table 7: ASR (AVG, %) per attack against TRCE+LlavaGuard |
| `dataset_quality.csv` | Table 2: per-category Spearman rho, stealthy rate, PPL@L3, delta-theta |

Derived values quoted in the text (Fig 2 aggregates, SEM error bars,
DeltaB SEM range) are computed from these CSVs.
