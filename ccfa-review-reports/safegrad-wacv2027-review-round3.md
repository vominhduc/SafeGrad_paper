# CCFA Scientific Review — SafeGrad (Round 3)

## 1. Metadata

- Review date: 2026-08-29 (evening)
- Manuscript version: git HEAD `62141c6`
- Prior rounds: round 1 (`be3119a`, weak-accept 7/10), round 2 (`907a3c3`, 8/10)
- Focus: verify closure of round-2 findings; audit the new edits since `907a3c3` (abstract/intro rewrite, InternVL2 swap-in, layout trims, Insight-3/5/6 enrichment)

## 2. Round-2 Finding Disposition

| ID | Status |
|---|---|
| R1 (placeholders ahead of data) | **Mostly closed** — InternVL2-8B row now measured (job 4466672: FPR@L0 0.2%, det 0.9/2.4/12.1%, FBR 0.879); abstract "all six filters near-flat / outperforms all six baselines" are now true statements about measured rows. **One FIXME remains**: Insight 3's CogView4-SLD `XX.X%` placeholders (jobs 4466225/4466690, generation ~99% complete). |
| R2 (FBR column mixes 1−L3 vs pooled L1–L3) | **Open** — decision still required. |
| R3 (InternVL2 triple role disclosure) | **Open** — one-sentence disclosure still not in the text. §6.4 now *shows* InternVL2 as baseline and remediation backbone, and §4.2 names it in the ensemble; the overlap remains implicit. |
| R5 (page-fit) | User-reported builds pass 8 pages after trims; layout churn continues while SLD numbers land — recheck then. |

## 3. Audit of Edits Since Round 2

**Abstract rewrite (+47 words):** quantified gradient (1.7→65.8%, +28.0 pp at L1→L2) verified against recomputed values; rung labels bound at first use; matched-anchor framing correct. The narrative arc (diagnosis → closure) reads as intended. No new unsupported claim.

**Introduction rewrite (−11 net):** temporal logic fixed (gap stated as verifiable hypothesis, not pre-established finding); rungs defined before use; insight arc ordering (6 before 7) correct after renumbering.

**Insight 5 with the new row:** "strongest absolute riser (Q16, 55.3→67.2)" — accurate under the table (LlavaGuard's larger +0.184 sensitivity reaches only 21.0% absolute; Q16 dominates in absolute detection). InternVL2 detail sentence matches Table 6 and `filter_eval.csv`.

**Insight 3 enrichment:** PixArt per-rung numbers match `dit_defense_transfer.json` (20.2→18.3 / 63.5→58.7 / 80.8→76.0, n=104/rung) — note the JSON's `relative_reduction` values (9.5/6.0) are percentages of *relative* reduction and the prose reproduces them correctly.

**Insight 6 readout clause:** 100/100 claim verified against the InternVL2 run's `readout_consistency.json` (0 mismatches); correctly attributed to the cross-family sentence, not the Qwen row.

**Layout hygiene:** ~180 words removed across the three overflow rounds with no claim loss; all removed text was restatement of tables/captions.

## 4. Minor Items

- M1: Table 6's InternVL2 baseline row now has per-level values while the two VLM baselines show "---" (binary-only). Fine, but the caption should note half a sentence (e.g., "InternVL2-8B verdicts are rung-scored per level") so the "---" contrast doesn't read as missing data.
- M2: Insight 3 sentence now runs long (two DiT evidence clauses + rationale). If the CogView4 numbers land positive, consider splitting so the FIXME placeholder's removal doesn't reflow unexpectedly.
- M3: With the InternVL2 row measured, the rebuttal's Q1/A1 argument ("blind zero-shot undershoots every deployed filter") gained a seventh data point; consider citing 12.1% L3 for InternVL2-8B in the rebuttal text.

## 5. Scores

- Methodology 8/10 (+0.5: third-family baseline measured) · Soundness 7.5/10 (pending SLD fill + R2 decision) · Clarity 8.5/10 · Reproducibility 8.5/10 · **Overall: 8/10 (accept band), award longlist conditional on closing R2/R3.**

## 6. Immediate Action List

1. Fill SLD placeholders when vote CSVs land; remove last FIXME; rebuild.
2. Decide R2 (recompute pooled baseline FBRs from stored predictions recommended).
3. Add the one-sentence InternVL2 triple-role disclosure (R3).
4. M1 caption half-sentence.
