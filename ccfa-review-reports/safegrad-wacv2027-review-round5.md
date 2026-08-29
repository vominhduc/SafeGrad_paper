# CCFA Scientific Review — SafeGrad (Round 5: Supplement Audit)

## 1. Metadata

- Review date: 2026-08-29 (late evening)
- Scope: the standalone supplement (`wacv_2027_supplementary.tex` + `sections/appendix.tex` + `tables/fresh_filter.tex` + cross-reference infrastructure)
- Manuscript version: git HEAD `415cb3f`

## 2. Verified Against Arithmetic / Artifacts

- **Ladder totals**: 99+99+96+98+92+91+87+88+98+96+82 = 1,026 ✓.
- **Tab. quality aggregate row is ladder-weighted, not category-mean** (verified by recomputation): stealthy 72.324 vs 72.32 ✓; PPL 33.914 vs 33.92 ✓; Δθ 40.2303 vs 40.23 ✓; Spearman ρ mean 0.9626 vs reported 0.962 — matches at truncation, would round to 0.963; display-level ambiguity only.
- **Fresh-generation table** (Tab. fresh): per-model n's (832×4, 814, 484) and Blk rates reproduce pooled n=4,626 (=4,992−366); pooled FPR=0.040, L1=0.947, FBR=0.024, zero-shot FBR=0.653 all reproduce from row entries ✓.
- **Remediation protocol paragraphs**: matched anchors (0.522 test-split + guidance), 3B replication (0.375→0.006), cross-family InternVL2 (0.401→0.013, FPR 3.8%, L1 98.1%) match `results/filter_internvl_crossfamily.json`/`filter_second_backbone.json` exactly. Readout consistency 100/100 ✓.
- **Backbone audit, human study (500 pairs, κ=0.73, pairwise κ 0.62–0.81), cultural probe (11.3/7.0, 31.3/25.7)**: internally consistent and, after today's fix, the rebuttal quotes the same numbers.
- **Label maps**: main→supp aux map covers all sections/tables/figures in use; supp→main map covers all app:* labels cited from the main text (incl. the newly added G.4 continuity).

## 3. Fixable Nits

- S1: The "Mean" row of the quality table should say **ladder-weighted mean** (review at 0.963-vs-0.962 rounding invites exactly this question).
- S2: Appendix remediation's "47.6M trainable parameters" is specific to the 7B run; the InternVL2 run trained 37.7M (same rank/alpha on InternLM2 projections). One parenthetical would prevent a misreading that 47.6M applies to all runs.
- S3: Old rebuttal answers (previous review round) still reference pre-renumbering insight names ("Insight 5" for the defense asymmetry). Historical record; re-align only if responses are re-issued.

## 4. Verdict

The supplement is **self-consistent and arithmetic-auditable**. No blocking findings.
