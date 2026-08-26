# Results snapshot

Canonical machine-readable export of all numbers reported in the paper
(tables in `tables/` and appendix tables in `sections/appendix.tex`),
generated from the LaTeX sources so that the reported statistics have a
durable record independent of evaluation logs.

| File | Contents |
|---|---|
| `t2i_hgr_by_model_category.csv` | Table 4: per-model, per-category HGR (%) at L0-L3 (264 rows) |
| `t2i_boundary_metrics.csv` | Table 3: Boundary Delta (pp), peak transition, SBS per model |
| `defense_by_group.csv` | Table 5: defense methods by group (NSFW/Legal/Social) HGR at L1-L3, SAS, DeltaHGR |
| `filter_eval.csv` | Table 6: filter detection rates by level, FPR@L0, FBR, sensitivity; `eval_set` column records the full-reference-set vs held-out-test-split distinction of the table footnote |
| `attack_asr.csv` | Table 7: ASR (AVG, %) per attack against TRCE+LlavaGuard, incl. the separately evaluated PGJ row |
| `dataset_quality.csv` | Table 2: per-category Spearman rho, stealthy rate, PPL@L3, delta-theta |
| `dataset_stats.csv` | App. dataset statistics: ladders per category, average prompt length per level |
| `overrefusal.csv` | App. over-refusal table: ORR per defense method |
| `cultural_l3_hgr.csv` | App. cultural-nuance table: L3 HGR for Western vs non-Western entities |
| `comatrix.csv` | App. cross-category co-occurrence matrix (%) at L2-L3 |
| `text_only_metrics.csv` | every quantity reported only in running text/captions (human-study kappa, SEM bounds, checker-activated results, lexical diversity, etc.) |
| `fresh_gen_filter_eval.json` | Deployment-regime evaluation of the tuned 7B filter vs matched zero-shot anchor on fresh generations from all six T2I models (104 held-out ladders x 4 rungs x 2 images per model): per-model and pooled per-level detection, FPR@L0, FBR, with excluded all-black degeneration counts (Insight 6, App. remediation) |
| `filter_second_backbone.json` | Backbone-generality check: identical protocol retrained on Qwen2.5-VL-3B; tuned vs zero-shot metrics on the held-out test split |
| `fresh_gen_filter_eval_3b.json` | Same deployment-regime evaluation as `fresh_gen_filter_eval.json` but for the 3B second backbone |
| `filter_threshold_sweep.json` | Operating-point robustness: YES-NO logit-margin threshold sweeps (t in [-4, 4]) on the test split and pooled fresh generations; both regimes reproduce the printed argmax row at t = 0 |

Derived values quoted in the text (Fig 2 aggregates, SEM error bars,
DeltaB SEM range) are computed from these CSVs.

Note: the `Mean` row in `dataset_quality.csv` is micro-averaged over
prompts (matching the paper text exactly); it differs slightly from the
macro mean over the eleven category rows.
