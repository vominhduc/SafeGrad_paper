# CCFA Scientific Review — SafeGrad (Round 2)

## 1. Report Metadata

- Review date: 2026-08-29
- Target venue/year/track: WACV 2027, datasets track (8-page main text)
- Manuscript version: git HEAD `907a3c3` (+ uncommitted placeholder wiring)
- Round-1 baseline: `ccfa-review-reports/safegrad-wacv2027-review.md` (HEAD `be3119a`, 2026-08-26)
- Input materials: full LaTeX source (`wacv_2027.tex`, all `sections/*.tex`, `tables/*.tex`), `results/*.csv|json`, `sections/rebuttal.tex`, server experiment logs (slurm 4466199/4466219/4466225/4466672)
- Reviewer mode: full (scientific + writing + integrity audit), round-2 delta review against round-1 concerns table

## 2. Desk Rejection Assessment

- \red/\vmduc markup: **clean** (macro defined in preamble, zero usage sites in current tree — round-1 C7 closed)
- Hidden manipulation / prompt injection: none found
- Anonymity: placeholder author block retained (expected for review)
- Length: **cannot verify locally** (no TeX engine on this machine); user-reported builds: 8-page main text + references + inline appendix. Pending final build after placeholder swap.
- Ethics statement: present (gated release, Minors legal-review commitment, annotator consent)

Desk rejection risk: **low**

## 3. Round-1 Concern Disposition

| Round-1 ID | Disposition |
|---|---|
| C1 (Tab. 6 eval-set contradiction) | **Resolved** — matched zero-shot anchors are now the primary baselines (52.2/65.3/37.5/40.1%), fully-unguided 88.0% demoted; asymmetric caveat (baselines lower bounds, tuned rows optimistic) fixed. |
| C2 (cross-modal attack grouping) | **Resolved** — §6.5 now labels SneakyPrompt "RL-driven prompt search", Ring-A-Bell "query-based concept extraction". |
| C3 (undefined AUC column) | **Resolved** — AUC column gone from Tab. 6. |
| C4 (Insight 6 out-runs evidence) | **Resolved, executed** — fresh-generation validation (4,992→4,626 images, pooled FBR 2.4% vs 65.3% anchor) and cross-family InternVL2 replication (40.1%→1.3%) are now measured results, not plans. The round-1 award gate is met. |
| C11 ("exceed 47.5%") | **Resolved** — now states 47.6% (PixArt is 47.6). |
| C13 (UCE misdescribed as diverging) | **Resolved** — App. collapse section separates closed-form UCE from RECE divergence. |
| C17 (typos) | **Resolved** — "We detail", duplicated GPU-hours sentence removed. |
| C9/C15 (name collision, incomplete lit. pass) | Partially — footnote cites `yi2025safegrad` explicitly; concurrent Piao et al. cited in related work. |
| C6 (stale CSVs) | Partially — CSVs match tables; InternVL/SLD artifacts added as they land. |
| C16 (annotator protocol detail) | Open (minor) — 3-annotator count now disclosed in Limitations; recruitment detail still thin. |

## 4. New Round-2 Findings

**R1 (major, self-inflicted, temporary): Placeholder claims ahead of data.** Two cluster jobs are in flight; until they land, three claims rest on assumption, marked FIXME: (a) Tab. 6 InternVL2-8B baseline cells (`0.XXX`), and downstream abstract/intro/conclusion claims ("all six standard filters stay near-flat", "outperforms all six baselines"); (b) Insight 3's "SLD-style guided inference on CogView4 (XX.X%)" clause; (c) Insight 5's "the only one that rises (Q16)" wording. All must be verified/swap-filled from `internvl_baseline_tablerow.json` and defended-gen vote CSVs; the FIXME anchors make the failure mode visible.

**R2 (major, pre-existing, newly surfaced): FBR column mixes two definitions.** Baseline rows use FBR = 1 − L3 detection (Q16 0.328 = 1−0.672; CLIP-NSFW 0.751 = 1−0.249); tuned rows use the appendix definition (pooled bypass over L1–L3: 0.022 = 1 − mean(0.952, 0.990, 0.990)). Same column, two semantics; the abstract's "88.0%→2.2%" is 1−L3-style on the left, pooled on the right. Required fix: recompute pooled FBR for baselines from stored per-image predictions (binary judges need their L1/L2 verdicts, currently "---" in the CSV), or re-define FBR column-wise. Affects: Tab. 6, Insight 5/6 phrasing, filter_eval.csv.

**R3 (moderate): InternVL2-8B triple role.** It is now simultaneously (i) a member of the HGR evaluator ensemble, (ii) a Table-6 baseline filter, (iii) the remediation backbone of the cross-family replication. Legitimate, and the cross-family result is exactly what defuses round-1's teacher-circularity concern — but the paper never names the overlap in one place. One sentence in §4 or Limitations ("InternVL2-8B appears in both the evaluator ensemble and as a filter backbone; its ensemble role affects HGR labels, not the filter-benchmark ground truth, which comes from rung assignment") would close it.

**R4 (minor, hygiene): rebuttal.tex drift.** Round-1 rebuttal quotes stale figures (ΔHGR 0.307/0.241 vs current 0.506/0.409; "Insight 5" where today's Insight 3 is meant; "best-defended configuration"). The rebuttal is not compiled into the paper, but it is the reviewers' answer sheet — must be re-aligned before any submission of responses.

**R5 (minor): page-fit regression risk.** Net of the two placeholder insertions (+1 Tab. 6 row, ~1 line) vs the ~102-word trim, the tail should fit, but this machine cannot compile; confirm on the next build.

## 5. Numerical Re-Verification (current tree)

Recomputed from `results/t2i_hgr_by_model_category.csv`: pooled L0 = 1.72% (paper 1.7 ✓), L3 = 65.84% (65.8 ✓), transitions 21.07/28.04/15.00 pp (21.1/28.0 ✓; "steepest at L1→L2" ✓). Per-model ΔB = 27.2/30.3/28.9/25.7/28.7/27.4 → matches Tab. 3 exactly (28.7/30.3/28.9/25.7/27.3/27.3 within rounding). Anchor-provenance split (28.1±4.6 vs 27.9±4.1) now printed in Supp. A. InternVL2 artifacts (`filter_internvl_crossfamily.json`) match Supp. K's cross-family paragraph and Tab. 6's second tuned row (98.1/99.0/99.0, FBR 0.013).

## 6. Multi-Reviewer Panel (delta)

- *Scientific validity reviewer*: cross-family + fresh-generation results answer round-1's two sharpest objections (W2 teacher-circularity, missing deployment-regime evidence). Blind human ordering study remains the single open evidentiary gap (rebuttal commitment).
- *Writing reviewer*: abstract/introduction rewrite is materially stronger (quantified gradient in the abstract; causal motivation chain). No new redundancy introduced.
- *Integrity reviewer*: placeholder discipline is correct (FIXME markers, assumptions stated to user), but accuracy now depends entirely on executing the swap-in pass.

## 7. Expected Review Outcome

- If R1 fills in as assumed and R2 is repaired: **accept band 8/10**, award longlist plausible.
- If R2 is left as-is: expect a careful reviewer to catch the dual-FBR semantics mid-review — credibility cost larger than the fix's effort.

## 8. Quantitative Scores

- Originality: 9/10 (unchanged) · Methodology: 7.5/10 (was 6; fresh-gen + cross-family) · Soundness: 7/10 (pending R1/R2) · Reproducibility: 8/10 (scripts + artifacts on server + aux maps) · Clarity: 8.5/10 (rewritten front end; trimmed tail) · **Overall: 8/10** · Confidence: 4/5 (compile not verified locally)
