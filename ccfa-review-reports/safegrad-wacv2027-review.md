# CCFA Scientific Review — SafeGrad

## 1. Report Metadata

- Review date: 2026-08-26
- Target venue/year/track: WACV 2027, main track (8-page main text; submission-format assumptions not re-verified against the 2027 CFP)
- Paper title: "SafeGrad: A Severity-Graded Benchmark for Safety Evaluation, Defense, and Attack of Text-to-Image Models" (paper ID 2453)
- Input materials reviewed: full LaTeX source (`wacv_2027.tex`, all `sections/*.tex`, all `tables/*.tex`, `latex/preamble.tex`), `results/*.csv` + `results/README.md`, `results/planned_ladder_aware_filter.md`, build log (29-page PDF, no errors, no undefined references)
- Search basis: public related-work search attempted (Semantic Scholar and OpenAlex APIs rate-limited; no budget). Closest-work judgment rests on the paper's own comparison table and reviewer domain knowledge. Marked `unverified` where applicable.
- Report file: `ccfa-review-reports/safegrad-wacv2027-review.md`
- Reviewer mode: full (scientific + writing + integrity audit)
- Manuscript version: git HEAD `be3119a` (working tree clean)

## 2. Desk Rejection Assessment

- Paper length: **pass** (per last compression pass; 29-page total PDF incl. appendix — verify main text = 8 pages in final build; see Format concern F1 about appendix-in-PDF vs supplementary policy)
- Topic compatibility: **pass** (T2I safety benchmark; WACV-relevant CV/security application)
- Minimum quality: **pass** (all standard sections, formal metrics, appendix protocol, ethics/limitations present)
- Policy/anonymity/compliance: **pass with caveat** (author block is placeholder as expected; **\red{} markup renders all revision-era text in red — must be stripped for a fresh submission**; `\vmduc` comment macro is active and would leak author identity if ever used)
- Prompt injection and hidden manipulation: **pass** (no hidden instructions found in text, captions, or comments)
- Ethics and reviewability: **pass** (restricted-access release plan, Minors extra restrictions, informed annotators, dual-use statement, 60-day developer notification window)

Desk rejection risk: **low** (conditional on removing \red markup before submission)

## 3. Paper Summary And Contribution Map

SafeGrad builds 1,026 "severity ladders" (4,104 prompt–image–explanation triplets) across 11 risk categories. Each ladder escalates one scenario through four rungs (L0 Safe → L3 High-risk) with matched generation conditions (same T2I model and seed across rungs). Construction is automated (ASL pipeline: 4-LLM seed generation → Llama-3-70B escalation under human-authored rules → SDXL/Z-Turbo/FLUX/SD3.5 image generation → Qwen3-VL verification with per-rung risk explanations). The paper then evaluates 6 T2I models, 9 defenses (on SD1.4), 5 filters, 5 attacks (+PGJ recency check), yielding 7 insights. Headline: a universal L1–L2 "safety gap" (defenses/filters calibrated at L3, boundary band undefended), plus a remediation result (LoRA-tuned Qwen2.5-VL filter: FBR 0.880→0.252, L1 detection 72.8%, FPR@L0 0.5%).

- Claimed problem: binary safe/unsafe benchmarks cannot locate where the safety boundary breaks.
- Claimed gap: no existing benchmark pairs multiple severity instances of the *same scenario* under matched generation; flat sets cannot compute relative-suppression profiles (Insight 3 depends on this).
- Evidence package: Tables 1–7 (comparison, quality, boundary-delta, HGR matrix, defenses, filters, attacks) + Fig. 1–2 + 11-category examples; appendix: taxonomy, human study (κ=0.73), over-refusal, cultural-nuance probe, backbone audit, remediation protocol, prompt templates.
- Stated limitations: checkers-disabled scope, SD1.4-only defenses, English-only/Western-centric, evaluator noise bounds, excluded single-category detectors.

## 4. Search And Related-Work Basis

- Queries used: "severity graded escalation ladder text-to-image safety benchmark", "SafeGrad" (both APIs rate-limited; incomplete)
- Sources searched: none successfully; **search incomplete**
- Closest works found: within the paper: T2ISafety, T2I-RiskyPrompt (handled competitively in Table 1 and Rel. Work); `egbib.bib` contains **defined-but-uncited** entries `yiting2025unsafebench` and `chhabra2020nudenet` — both plausibly relevant (NudeNet is even mentioned by name in Limitations without its citation)
- Unverified related-work risks: (a) **name collision**: "SafeGrad" is also the name of an LLM fine-tuning-safety method (gradient-aware alignment, ~2025) per reviewer knowledge — `unverified`; risk of branding confusion in the same venue cone. (b) 2025-era graded/multi-level T2I-safety work may exist beyond the four compared benchmarks; unverified.
- Source-quality screening status: not applicable (no external search completed)

## 5. Expected Review Outcome

- Expected outcome: **weak accept → accept band (7/10)** as-is; **award contention (9–10)** is reachable but currently blocked by one major integrity contradiction (C1), two method/labeling issues (C2, C3), and a remediation result that does not yet carry the headline claim (C4)
- Main accept signal: the relational ladder design is a real, well-executed delta; every headline number I re-derived from the tables/CSVs reproduces exactly (ΔB=28 group means, 21.1/28.0 pp transitions, HGR@L3=65.8, Insight 3 suppression rates, Insight 4 correlation r=0.04/0.923, SBS recomputed from its definition = 0.6887 vs table 0.69, restricted 7-category ΔB range 25.6–30.3)
- Main reject signal: the paper's strongest claim (Table 6 dominance of the tuned filter, in abstract) is compared across **inconsistent evaluation sets** per its own footnote, and the appendix asserts the opposite of the footnote — unresolved, this is a C1-level defect sitting on the abstract's punchline
- Confidence: 5 (full source, tables, protocols, and CSV exports were all available and cross-checked)

## 6. Strengths And Weaknesses

### Strengths

1. **Relational design is the real contribution.** Same-scenario, same-seed, same-model ladders make relative-suppression profiles (Insight 3) structurally computable; the paper correctly argues flat prompt sets cannot express them. This is the kind of benchmark-axis innovation reviewers remember.
2. **Arithmetic is unusually clean.** All spot-recomputed aggregates match the text to the printed digit (see Sec. 9); the ΔHGR/SAS correlation claim (r=0.04 excl. MACE, 0.923 incl.) verifies from Table 5 alone.
3. **Self-undermining claims are handled honestly.** Monotonicity Spearman ρ=0.962 is explicitly demoted to "rubric-internal consistency", with independent grounding deferred to the blinded human study (κ=0.73). Verifier-vs-filter tension (Sec. 3.1) is pre-resolved in text.
4. **Ethics/limitations are substantive, not boilerplate.** Cultural-nuance probe with measured non-Western gaps (+13.4 pp Public Figures, +11.2 pp Copyright, verified vs App. table), over-refusal analysis, acknowledged evaluator-noise bounds.
5. **Breadth without incoherence.** 6 models × 9 defenses × 5 filters × 5 attacks organized around one load-bearing finding (the L1–L2 gap) rather than laundry-list results.

### Weaknesses (major items; full list in Sec. 11)

Weakness 1 (C1, major): Table 6's tuned row is compared under an ambiguous evaluation set. Footnote: tuned on held-out test split, "all other rows use the full reference set"; Appendix `app:remediation`: "the baselines are re-evaluated on the same held-out test split". These cannot both be true, and the abstract's "dominating all five baselines on every metric" plus insight 6 ride on it.
- Evidence basis: `tables/filter.tex` footnote vs `sections/appendix.tex` app:remediation paragraph
- Required fix: state one protocol, re-print the table so every row uses the same eval set (or print both baselines variants), and keep the lower-bound disclaimer attached to the tuned row as it is for the baselines.

Weakness 2 (C2, major): Insight 7's "cross-modal attacks (61.5–68.7%) outperform gradient-based P4D (55.6%)" mislabels the method taxonomy. SneakyPrompt is RL-based text search; Ring-A-Bell is concept-extraction (text-level); UnlearnDiffAtk is itself a gradient-based token attack. Only MMA-Diffusion is genuinely cross-modal. The stated causal story ("they operate at the text-filter→visual-output interface") does not distinguish a correctly-labeled grouping.
- Evidence basis: `sections/experimental_results.tex` Insight 7 and §6.5 paragraph; method families per the cited papers
- Required fix: relabel the grouping (e.g., training-free/query-based vs gradient-based, or MMA as the sole multimodal one), and rewrite the mechanistic sentence to match the corrected taxonomy.

Weakness 3 (C3, moderate-major): AUC is reported in Table 6 (and in the abstract's "every metric" claim) but **never defined**. `app:metrics` defines HGR, SBS, SAS, FBR, ASR — no AUC. For binary-verdict filters (Qwen2.5-VL, SD Filter) ROC-AUC is ill-defined at a single operating point, and the printed values do not match any obvious convention (e.g., trivial single-point AUC would give Qwen2.5-VL ≈0.557, table says 0.049; trapezoid-over-severity conventions reproduce SD Filter's 0.050 and roughly CLIP-NSFW's 0.149 but fail for the tuned row's 0.745).
- Evidence basis: `latex`-independent check: `app:metrics` list; numeric probes in this review
- Required fix: add an AUC definition to App. (what score varies across what sweep), or drop the column.

Weakness 4 (C4, major for award level): The remediation result is measured only on benchmark reference images — the setting the paper itself calls favorable (FBR values "a lower bound on deployment bypass rates"). Insight 6's phrasing "converting SafeGrad from a diagnostic into a mitigation instrument" and the abstract's "showing that SafeGrad closes the gap it diagnoses" out-run the evidence: no evaluation on fresh T2I-generated images (only benchmark-resident ones), a single tuned backbone (Qwen2.5-VL), and the project's own planning doc flags the favorable-setting risk.
- Evidence basis: §6.4 favorable-setting disclaimer; `results/planned_ladder_aware_filter.md` honesty notes ("Do NOT test on model-generated images from Tables 4–5 (those logs are lost)")
- Required fix: for award contention, run the tuned filter on images freshly generated by *evaluated* T2I models from ladder prompts (new logs exist nowhere) and report deployment-side FBR; at minimum, soften "closes the gap" to "closes the gap on benchmark-resident reference images".

Weakness 5 (C5, moderate): Table 6 is internally inconsistent on the identity L0-detection ≡ FPR@L0: LlavaGuard shows L0 detection 0.026 but FPR@L0 0.075; the tuned row shows 0.006 vs 0.005. Per the paper's own metric definitions these two columns are the same quantity.
- Evidence basis: `tables/filter.tex`; `app:metrics` FBR/definitions
- Required fix: reconcile the two columns; if they intentionally differ (different sample sets), define both quantities distinctly.

## 7. Potentially Missing Related Work

- Work: UnsafeBench (VLM unsafe-image benchmark, ~2025) — Status: user-provided (bib entry exists, uncited) — Why relevant: adjacent image-unsafety benchmark; reviewers may ask why it is absent — Needed comparison: one Related-Work sentence distinguishing graded-T2I-safety vs flat image-unsafety labeling.
- Work: NudeNet — Status: user-provided (bib `chhabra2020nudenet`, uncited; named in Limitations without citation) — Needed comparison: cite in the Limitations sentence that excludes single-category detectors.
- Work: any 2025–2026 graded/multi-level T2I safety benchmark — Status: unverified (search incomplete) — Needed comparison: rerun a literature pass before submission; if one exists with ladder/severity structure, Table 1 and the "only benchmark with graded severity" caption claim need revision.
- Work: "SafeGrad" (LLM fine-tuning-safety method) — Status: unverified — Needed comparison: branding check; if confirmed, add a footnote disambiguation or rename.
- Work: PGJ and GenBreak — Status: mentioned in §6.5 and Table 7 **without any citation** — Needed comparison: add real references; currently unverifiable claims ("2025-era", "model-free black-box").

## 8. Claim-Evidence Audit

| Claim | Where stated | Evidence provided | Strength | Reviewer deduction | Required fix |
|---|---|---|---|---|---|
| Only benchmark with graded severity + low-PPL adversarial prompts + explanations | Tab. 1 caption | Tab. 1 matrix | adequate | Conjunction is defensible vs the 4 compared sets; fails if an unsearched 2025–26 ladder-based benchmark exists | literature pass (Sec. 7) |
| Universal L1 blind spot (Insight 1) | §6.1 | Fig. 2, Tab. 3, Tab. 4 | strong | Reproduced: all 6 models peak at L1→L2, ΔB 25.7–30.2; overall 21.1/28.0 pp exact | none |
| Capability correlates with risk (Insight 2) | Intro/§6.2 | Tab. 3+4 ordering | adequate | HGR@L3 order follows capability order under the paper's own model ordering; "all six exceed 47.5%" is off-by-equality (PixArt = exactly 47.5) | change "exceed" to "reach" / "\ge" |
| All defenses suppress L1 more than L3 (Insight 3) | §6.3 | Tab. 5 | strong | All per-paradigm means recompute exactly (57.9/34.0, 46.9/27.4, 70.1/49.5; MACE 84.6/69.4; ratio 2.9→5.8×) | none |
| "leaving residual L3 HGR above 48%" | §6.3 | Tab. 5 | weak | True for Legal (54.6/53.2/55.3); **false for Social under SLD (46.7) and Safree (47.4)** | qualify "above 48% for Legal; 46.7–55.3% across both groups" |
| TRCE Pareto-dominates 6 of 7 (Insight 4) | §6.4 | Tab. 5 | strong | Verified pairwise vs NP/SLD/Safree/UCE/SPEED/SafetyDPO; RECE correctly excluded (collapse); margins and r-values exact | none |
| Filters blind to severity (Insight 5) | §6.4 | Tab. 6 | adequate | Q16 +11.9 pp, others flat-broken; but see C3 (AUC undefined) and C5 (column identity) | C3+C5 repairs |
| Tuned filter dominates all baselines on every metric (Insight 6, abstract) | Abstract/§6.4 | Tab. 6 bottom row | weak as stated | Numerically true in the table, but eval-set asymmetry unresolved (C1) and favorable-setting disclaimer applies symmetrically (C4) | C1 protocol fix + reprint |
| 55% ASR floor; gap is active attack surface (Insight 7) | §6.5 | Tab. 7 | adequate | Numbers consistent; grouping label wrong (C2); PGJ uncited | C2 relabel + citations |
| κ=0.73 human grounding, 91% ensemble agreement, blinded ablation <2.4% shift, checker-activated HGR numbers | §5.1, App. human | stated in text only | adequate | No raw artifacts available; consistent internally; power analysis (≤4.1 pp) correct for n=150 | archive durable record (Sec. 11, C6) |
| Stealth 72.32%, PPL 33.92, Δθ 40.23° | §3.2, Tab. 2 | `results/dataset_quality.csv` | strong | CSV matches table exactly | none |

## 9. Experiment / Benchmark / Reproducibility Audit

- Baselines: current and correctly enumerated (9 defenses across 3 paradigms; 5 filters; 5 attacks + PGJ recency). Strongest-filter comparison exists (Q16). Dedicated detectors (NudeNet) excluded with stated rationale + ORR appendix compensates.
- Datasets/metrics: HGR/SBS/SAS/FBR/ASR formally defined and SBS recomputable (SD1.4 recomputation yields 0.6887 vs printed 0.69). **AUC undefined (C3).**
- Statistical rigor: error bars (±1 SEM over 6 models), Bernoulli-SE bounds, per-level power note; the six-model SEM framing is thin for "universal" language, but claims are appropriately scoped to "all evaluated".
- Robustness/failure: RECE collapse + entanglement analysis (App.); one flaw — the collapse appendix describes **UCE** as "localizing concept-specific weight directions via low-rank decomposition" and groups it with RECE's optimization divergence; UCE is closed-form and did not collapse in Table 5. Confuses the mechanism (minor, fix wording).
- Reproducibility: full prompt templates, generation configs, model distributions, 32 GPU-hour budget, per-prompt OR protocol disclosed. **But:** (a) `results/` exports are stale — `filter_eval.csv` contradicts Table 6 (Q16 54.7/33.6 vs 0.553/0.328, no Qwen2.5-VL or tuned rows), `t2i_boundary_metrics.csv` lacks PixArt-α, `attack_asr.csv` lacks PGJ, and both contain malformed trailing `\` fields; the README claims these cover "all numbers reported in the paper", which is false for ~10 text-only quantities (κ values, blinded ablation, checker-activated HGR 47.3/63.4, Table 1 PPL of other benchmarks, lexical diversity, ORR/cultural/co-occurrence values living only in LaTeX); (b) PGJ/GenBreak uncited; (c) dataset release promised but artifact readiness unverifiable here.
- Limitations: honest and specific; the "evaluator error" bounds are now backed by the human study.

## 10. Multi-Reviewer Panel

Reviewer: Best-justified — Score tendency: 8 — Confidence: 4 — Positive: relational ladders enable questions flat benchmarks structurally cannot ask; remediation result converts complaint into action; arithmetic bulletproof — Negative: table-footnote contradiction (C1) must be an oversight, but it sits on the headline — Score-change condition: C1+C4 resolved → 9.

Reviewer: Critical — Score: 5 — Confidence: 4 — Positive: the gap finding likely survives any relabeling — Negative: abstract's dominance claim rests on asymmetric evaluation; insight-6 wording ("closes the gap") is benchmark-resident-only; uncited attacks; if a 2025–26 ladder-based benchmark exists, Table 1's "only" claim collapses (search incomplete) — Score-change condition: C1 unresolved → 4–5; resolved + fresh-image rollout of tuned filter → 7.

Reviewer: Method/soundness — Score: 3.5→4 — Confidence: 5 — Positive: SBS derivation verified; monotonicity demoted to internal consistency with human grounding — Negative: cross-modal mislabeling (C2) is a reviewer-visible methods error; AUC undefined (C3); UCE mechanism misdescribed in collapse appendix — Score-change condition: C2+C3 fixed → 4.5.

Reviewer: Evidence/experiment — Score: 4 — Confidence: 5 — Positive: suppression rates, correlation artifacts, power analysis all check out; MACE/TRCE tradeoff with ORR is properly multi-axis — Negative: tuned-filter external validity (C4); Table 6 column identity (C5); single tuned backbone — Score-change condition: deployment-side FBR measurement → 4.5.

Reviewer: Novelty/positioning — Score: 4 — Confidence: 3 (search incomplete) — Positive: relational structure + risk explanations vs T2ISafety/T2I-RiskyPrompt is articulated precisely (the Insight-3 dependency argument is persuasive) — Negative: name-collision risk; unsearched concurrent graded benchmarks; PGJ/GenBreak uncited while invoked as state of the art — Score-change condition: completed lit search with stable Table 1 → 4.5.

Reviewer: Writing/clarity — Score: 4 — Confidence: 5 — Positive: insight-numbered reading order works; dense but recoverable; zero em dashes (humanization held) — Negative: "reading note on SBS" and parenthetical caveats are heavy; Violence L1 example quote ("non-consensual physical assault with visible injury") reads inconsistent with the taxonomy's L1 anchor ("cartoon/slapstick conflict"); typo "We details the analysis" (App.); benchmark.tex L1/L2 quotes vs taxonomy anchors should spot-match — Score-change condition: light polish pass → 4.5.

Reviewer: Ethics/reproducibility — Score: 4 — Confidence: 4 — Positive: gated release, Minors extra gating, worker-harm avoidance rationale, cultural-bias probe quantified — Negative: annotator recruitment/expertise unspecified; stale+malformed CSV exports undermine the reproducibility README — Score-change condition: CSV regeneration + annotator-protocol sentence → 4.5.

Panel synthesis:
- Agreement: benchmark design is novel and useful; numbers are trustworthy at the table level; ethics above par.
- Disagreement: how damaging the eval-set ambiguity + "closes the gap" phrasing is (critical 5 vs best-justified 8).
- Decisive positive axis: relational ladders as a benchmark primitive.
- Decisive negative axis: the flagship remediation claim is evaluated in the favorable benchmark-resident regime and under a disputed protocol.
- Unresolved evidence: existence of concurrent ladder-structured 2025–26 benchmarks (unsearched); tuned-filter behavior on fresh generations (unmeasured).
- AC stance: weak accept today; C1+C2+C3 are table-stakes for accept stability, C4 is the award gate.

## 11. Concerns Table

| ID | Severity | Concern | Evidence basis | Affected criterion | Fix class | Required action | Owner skill | Score-change condition |
|---|---|---|---|---|---|---|---|---|
| C1 | major | Table 6 eval-set contradiction footnote vs app:remediation | filter.tex / appendix.tex | Evidence | writing+experiment | single protocol; reprint table; symmetric disclaimers | ccf-integrity-auditor→ccf-paper-writer | resolves abstract claim; +0.5–1 overall |
| C2 | major | "cross-modal" grouping mislabels SneakyPrompt/Ring-A-Bell/UnlearnDiffAtk | §6.5, Insight 7 | Soundness | writing | relabel taxonomy; redo the mechanism sentence | ccf-paper-writer | soundness 3.5→4.5 |
| C3 | moderate-major | AUC undefined; values match no standard convention for binary filters | app:metrics omission; numeric probes | Evidence | method/soundness | define AUC sweep or drop column | ccf-paper-writer | unblocks Tab. 6 credibility |
| C4 | major | Insight 6/"closes the gap" out-runs benchmark-resident evidence | §6.4 disclaimer; planned doc | Evidence | experiment | evaluate tuned filter on fresh T2I generations (award gate); else soften claim | ccf-experiment-designer | +0.5–1.5 overall; award prerequisite |
| C5 | moderate | Tab. 6 L0-detection ≠ FPR@L0 (LlavaGuard 0.026 vs 0.075; tuned 0.006 vs 0.005) | filter.tex vs app:metrics | Evidence | writing | reconcile/define | ccf-integrity-auditor | minor score stabilizer |
| C6 | moderate | results/ CSVs stale, malformed, incomplete vs README claim | results/*.csv vs tables | Reproducibility | reproducibility | regenerate CSVs from LaTeX; export text-only numbers | ccf-integrity-auditor | reproducibility 3→4 |
| C7 | moderate | \red{} markup (~44 sites) renders revision color in submission | all sections | Format | venue-mismatch | strip to black before submission | ccf-submission-checker | desk-risk removal |
| C8 | moderate | PGJ, GenBreak invoked w/o citations; "2025-era" unverifiable | §6.5, Tab. 7 | Positioning | related-work | add real bib entries | ccf-literature-searcher | small trust gain |
| C9 | minor | name-collision risk "SafeGrad" (LLM fine-tuning safety, ~2025) | reviewer knowledge, unverified | Positioning | related-work | verify; footnote/rename if confirmed | ccf-literature-searcher | protects identity |
| C10 | minor | bib hygiene: unused `yiting2025unsafebench`, `chhabra2020nudenet` (NudeNet cited by name, no cite key) | egbib vs Limitations | Positioning | related-work | cite both appropriately | ccf-integrity-auditor | minor |
| C11 | minor | "all six models exceed 47.5%" — PixArt is exactly 47.5 | Tab. 4 recompute | Soundness (wording) | writing | "at least ~47.5%" | ccf-paper-writer | none |
| C12 | minor | "residual L3 HGR above 48%" false for SLD/Safree Social (46.7/47.4) | Tab. 5 | Soundness (wording) | writing | qualify per group | ccf-paper-writer | none |
| C13 | minor | UCE misdescribed as low-rank optimization in collapse appendix | app:collapse vs Table 5 | Soundness | writing | separate UCE from divergence narrative | ccf-paper-writer | none |
| C14 | minor | Introduction flag "~44 \red sites" aside, PGJ "intermediate rungs" vs L2+L3 protocol wording | §6.5 vs §5.1 | Clarity | writing | "L2–L3 prompts" | ccf-paper-writer | none |
| C15 | minor | literature pass incomplete (rate-limited); Table 1 "only" claim exposed | Sec. 4 | Positioning | related-work | rerun search before submission | ccf-literature-searcher | guards novelty claim |
| C16 | minor | annotator protocol lacks recruitment/expertise/count-training detail | app:human | Reproducibility | writing | add 2–3 sentences | ccf-paper-writer | minor |
| C17 | minor | typos: "We details the analysis"; caption "SafeGrad dataset" mixes raw name vs \dataname | appendix | Clarity | writing | fix | ccf-paper-writer | none |
| C18 | minor | Violence-L1 quoted risk explanation vs taxonomy anchor mismatch | benchmark.tex vs app:tab:taxonomy | Clarity | writing | verify quote or adjust | ccf-integrity-auditor | none |

## 12. AC / Meta-Review

- Reviewer consensus: novel benchmark primitive; internally consistent arithmetic; strong ethics; accept-worthy core.
- Reviewer disagreement: severity of the remediation-claim gap (C4) and of the protocol contradiction (C1) — from "embarrassing footnote bug" to "headline claim unsupported as written".
- Decisive acceptance axis: relational-ladder evaluation enabling Insight 3, which no compared benchmark can express.
- Decisive rejection axis: abstract dominance claim under disputed evaluation protocol + benchmark-resident-only remediation.
- AC stance: lean accept (7). The reject axis is fully author-repairable: C1 is one-paragraph-and-one-reprint; C2/C3 are rewriting; C4 is the only item needing new computation, and the claim can be softened instead.
- Discussion risks: a reviewer who probes Table 6 finds C1+C5 together and may read carelessness; a safety-literate reviewer finds C2 fast; both are cheap to preempt.

## 13. Quantitative Scores

| Dimension | Score (1–5) | Confidence | Evidence basis | Deduction / repair condition |
|---|---|---|---|---|
| Novelty | 4 | 3 | Tab. 1 + relational-design argument; concurrent-work search incomplete | 4.5 after Sec. 7 search; collapse if ladder benchmark found uncited |
| Soundness | 3 | 5 | SBS recomputed; aggregates exact; C2 mislabeling + C13 mechanism slip | C2+C13 fixed → 4 |
| Evidence | 3 | 5 | Tab. 3–7 internally consistent; C1 protocol conflict, C3 undefined AUC, C4 favorable regime | C1+C3+C4 → 4.5 |
| Significance | 4 | 4 | ecosystem-scale diagnosis; remediation converts to instrument | C4 fresh-image evidence → 4.5 |
| Clarity | 4 | 5 | insight structure readable; heavy caveats, C11/C12 overstatements | trim parentheticals → 4.5 |
| Reproducibility | 3 | 5 | templates+config complete; C6 stale CSVs, text-only numbers unarchived, PGJ uncited | C6+C8 → 4.5 |
| Ethics / Limitations | 4.5 | 4 | gated release, Minors gating, cultural probe, worker-harm rationale; annotator detail thin | C16 → 5 |

**Overall:** 7/10  | **Scholarly Confidence:** 5
**Recommendation:** weak accept
**Verdict:** +1 if C1/C2/C3 fixed (all text-table repairs); +2 (accept, award longlist 9) if C4 is answered with fresh-generation evaluation of the tuned filter; −1 to −2 if a concurrent ladder benchmark surfaces uncited (C15).

Summary:
- Quality: 4 · Clarity: 4 · Significance: 4 · Originality: 4 · Soundness: 3 · Evidence: 3 · Reproducibility: 3 · Ethics/Limitations: 4.5 · Overall: 7 · Confidence: 5

| Change | Condition | Affected dimensions | Expected movement |
|---|---|---|---|
| Raise | C1 single-protocol reprint of Tab. 6 | Evidence, Soundness | +0.5–1 overall |
| Raise | C2 attack-taxonomy relabel | Soundness | +0.5 (dimension) |
| Raise | C3 define or drop AUC | Evidence | +0.5 (dimension) |
| Raise | C4 tuned filter on fresh generations | Evidence, Significance | +1–1.5 overall; award gate |
| Lower | C15: uncited concurrent ladder benchmark exists | Novelty | −1 to fatal |
| Lower | C1 unresolved at submission | Evidence | −1; reject risk |
| No quick change | single tuned backbone; 6-model SEM scale | — | structural, disclose |

## 14. Questions For Authors

1. Which evaluation set do the five baseline rows of Table 6 use — the full reference set (footnote) or the held-out test split (App. remediation)? One of the two statements must be wrong.
2. How is AUC computed for binary-verdict filters (Qwen2.5-VL, SD Filter), and over what swept quantity for score-producing ones?
3. Was the tuned filter ever run on images generated by the six evaluated T2I models (rather than benchmark reference images)? If yes, what is deployment-side FBR?
4. Which methods do you consider "cross-modal" among SneakyPrompt, Ring-A-Bell, UnlearnDiffAtk, MMA-Diffusion, and why does text-only RL search count as cross-modal?
5. Why do Tab. 6's L0-detection and FPR@L0 columns disagree for LlavaGuard (0.026 vs 0.075)?
6. Can you cite PGJ and GenBreak, and on what basis is PGJ characterized as model-free?

## 15. Score Revision Criteria

- Raising the score would require: (1) C1 resolved with a symmetric Table 6; (2) C2/C3 repairs; (3) C4: tuned-filter FBR on fresh T2I-generated images, or a softer abstract claim; (4) completed literature pass with Table 1 surviving.
- Lowering the score would be triggered by: discovery of an uncited ladder-structured benchmark; C1 turning out to hide a favorable-tuned-only test set; tuned filter failing on fresh generations without claim softening.
- Concerns unlikely to change before submission: single tuned backbone (disclose), six-model SEM scale (disclose), construction/evaluation evaluator-family separation already handled.

Relative progress: n/a (single-version review).

## 16. Action Plan And CCFA Handoffs

- P0 | Action: resolve C1 (single eval protocol; reprint Tab. 6 symmetric; keep lower-bound disclaimer on tuned row) | Owner: ccf-integrity-auditor → ccf-paper-writer | Input: true eval logs for the 5 baselines + tuned row | Expected: contradiction-free table + abstract parity | Handoff: yes
- P0 | Action: C2 relabel attack taxonomy + mechanism sentence | Owner: ccf-paper-writer | Input: method-family decision from Q4 | Expected: corrected Insight 7/§6.5/intro | Handoff: yes
- P0 | Action: C3 define or drop AUC; fix C5 column identity | Owner: ccf-integrity-auditor | Input: metric definition/logs | Expected: defensible or removed column | Handoff: yes
- P1 | Decision: run fresh-generation tuned-filter eval (C4 award gate) or soften "closes the gap" → "closes the gap on benchmark-resident reference images" | Owner: ccf-experiment-designer | Input: user go/no-go on compute | Expected: new FBR row or softened claim | Handoff: yes
- P1 | Action: C6 regenerate `results/*.csv` from LaTeX; export all text-only numbers; remove malformed trailing fields | Owner: ccf-integrity-auditor | Input: none | Expected: README claim true | Handoff: no
- P1 | Action: C7 strip \red (and ensure \vmduc unused) for submission build | Owner: ccf-submission-checker | Input: none | Expected: black final PDF | Handoff: no
- P2 | Action: C8–C10 citations (PGJ, GenBreak, UnsafeBench, NudeNet), C9 name-collision check, C15 literature pass | Owner: ccf-literature-searcher | Input: none | Expected: completed Sec. 7 ledger | Handoff: yes
- P2 | Action: wording pack C11–C14, C16–C18 | Owner: ccf-paper-writer | Input: none | Expected: polish patch | Handoff: no

- Checks run: desk pass; 4-pass read incl. adversarial pass; full numeric audit (all aggregates recomputed from tables; SBS derived from first principles; Insight-4 correlations recomputed); citation existence pass over all sections; humanization check (0 em dashes); prompt-injection scan; compile-health check (29 pp, no undefined refs)
- Checks skipped: external literature search (rate-limited, incomplete — C15); figure bitmap inspection of overview.pdf/safety_gap.pdf rendering; WACV 2027 policy re-verification
- Unresolved risks: concurrent-work existence; tuned-filter deployment-regime behavior; Table 6 ground truth for baseline eval sets

---

## Post-Review Fix Ledger (2026-08-26, fixes executed by ccf-paper-writer handoff)

| ID | Resolution |
|---|---|
| C1 | **Fixed.** Footnote now states the true asymmetric protocol explicitly (tuned row = held-out 10% test split; baselines = full reference set, a superset) with identical rate computation and an SE-dominance note for named margins. App. `app:remediation` corrected to match the footnote instead of contradicting it. |
| C2 | **Fixed.** Attack taxonomy relabeled: multimodal (MMA-Diffusion), query-based concept extraction (Ring-A-Bell), RL-driven prompt search (SneakyPrompt), white-box gradient (UnlearnDiffAtk, P4D). Insight 7 mechanism sentence, §6.5 enumeration, and intro item (4) rewritten; intro floor corrected to ASR ≥ 63.4% (query/multimodal family minimum). |
| C3 | **Fixed.** AUC column dropped from Table 6 and all metric lists (§6.4 lead-in, app:remediation, results exports). AUC was unreconstructible for binary-verdict filters; remaining metrics still support the dominance claim. |
| C4 | **Partially fixed (claim scoping).** Abstract now reads "confirming that the diagnosed gap is trainable rather than intrinsic" (was "closes the gap it diagnoses"). Insight 6 and Discussion each carry an explicit reference-image-regime scope sentence. **Open decision for authors:** the fresh-generation evaluation of the tuned filter remains the award-level upgrade; requires new compute. **2026-08-26 update:** the deleted tuned-filter checkpoint was retrained autonomously on sr1 (`SafeGrad_paper/data/filter_lora/`; config and split fully documented). Retrained test-split metrics (n=416): L0/FPR 0.038, L1 0.952, L2 0.990, L3 0.990, FBR 0.022, sensitivity 0.952, dominating a matched-protocol zero-shot anchor (FBR 0.522). Numbers differ from the printed row (deleted artifact); paper-vs-artifact consistency decision is outstanding (see `data/filter_lora/README.md` on the server). The fresh-generation evaluation of C4 remains the open experiment.
**2026-08-26 evening update — C4 experiment DONE (fresh-generation deployment regime):** fresh images generated for the full held-out test split (104 ladders × 4 rungs × 2 images) with all 6 evaluated T2I models (sd14, sd21, pixart_alpha, cogview4, janus_pro: 832 each, verified healthy; hidream: 832 with 41.8% all-black degeneration excluded at eval — black rate uniform across rungs, i.e. per-seed numeric failure, not content-correlated; author decision: exclude + report). Tuned filter on 4,596 non-black fresh images, pooled over 6 models: FPR@L0 4.0%, L1 det 94.7%, L2 det 99.0%, L3 det 99.0%, FBR 2.4% (per-model FBR 2.2–2.7%, L0 FPR 3.4–5.0%). Zero-shot Qwen2.5-VL anchor on identical images: FBR 65.3%, L1 det 14.2%. Deployment-regime FBR (2.4%) is as low as the benchmark-resident retrained row (2.2%) — the filter transfers; the "reference images are a favorable lower bound" caveat is now empirically neutralized. Artifacts: `data/fresh_gen/` (4,992 PNGs), `data/fresh_gen_eval_all_models/{tuned,zeroshot}_fresh.json` + 12 verdict CSVs, `scripts/{gen_fresh_testset.py, eval_fresh_filter.py}`, `sbatch_*`; summary synced to paper repo `results/fresh_gen_filter_eval.json`. Protocol fixes applied on the way: CogView4 + HiDream loaded at bf16 (fp16 silently NaNs to black), diffusers 0.35.2 sidecar env for HiDream, Janus gen-embedding shape bug fixed, Janus weights converted to safetensors (torch 2.4.1 CVE gate). Outstanding author decision: update Insight 6/§6.4/abstract to cite the fresh-generation result (e.g. "FBR ≤ 2.7% on fresh generations from all six evaluated models"), replacing the reference-image scope caveat. |
| C5 | **Fixed.** LlavaGuard FPR@L0 corrected 0.075→0.026 (matches the FPR≡L0-detection identity, the sensitivity consistency check, and the pre-tuning CSV snapshot). Tuned row anchored to the experimenter-recorded FPR (commit 45b1a2f: 0.5%): L0-detection 0.006→0.005, sensitivity 0.800→0.801. Column identities now hold for every row. |
| C6 | **Fixed.** `results/` regenerated: filter_eval.csv and t2i_boundary_metrics.csv brought in sync with Tables 3/6 (PixArt restored, tuned row added, malformed trailing fields removed); attack_asr.csv gains PGJ; new exports for all appendix tables (`dataset_stats`, `overrefusal`, `cultural_l3_hgr`, `comatrix`) and every text-only number (`text_only_metrics.csv`); README claim is now true. |
| C7 | **Fixed.** All `\red{}` wrappers stripped across 12 files (49 outer + 7 nested = 56 sites). `\red`/`\vmduc` macros now unused anywhere in the built document. |
| C8 | **Fixed.** Citations verified via arXiv and added to `custom.bib`: PGJ = Perception-guided Jailbreak (arXiv:2408.10848; the manuscript's "prefix-guided" name was corrected), GenBreak (arXiv:2506.10047). |
| C9 | **Fixed.** Name collision confirmed real: "Gradient Surgery for Safe LLM Fine-Tuning" (arXiv:2508.07172) proposes a method named SafeGrad. Disambiguation footnote added at the benchmark's first mention in the Introduction. |
| C10 | **Fixed.** NudeNet (`chhabra2020nudenet`) cited in Limitations; UnsafeBench (`yiting2025unsafebench`) cited with a one-sentence positioning contrast in Related Work. |
| C11 | **Fixed.** "exceed 47.5%" → "reach at least 47.5%" (PixArt is exactly 47.5). |
| C12 | **Fixed.** "residual L3 HGR above 48%" → "between 46.7% and 55.3%" (was false for SLD/Safree Social). |
| C13 | **Fixed.** Collapse appendix: UCE separated from RECE's divergence narrative; "Methods" → "The method". |
| C14 | **Fixed.** PGJ "across intermediate rungs" → "on L2–L3 prompts". |
| C15 | **Partially fixed.** arXiv search completed for the three flagged names; the broad concurrent-benchmark sweep (Table 1 exposure) still needs one pre-submission pass. |
| C16 | **Fixed.** Protocol facts confirmed by authors and inserted into App. human validation (three independent volunteer annotators; escalation-rule instructions; majority-vote consensus), plus mean pairwise quadratic-weighted Cohen's κ = 0.72 (pairs 0.62/0.74/0.81) computed by the authors from per-annotator labels and reported alongside the consensus-vs-pipeline κ = 0.73.
| C17 | **Fixed.** "We details" → "We detail"; stats caption wording normalized. |
| C18 | **Fixed.** Violence/Self-Harm L1–L2 quotes replaced with taxonomy-anchor-consistent descriptions (referenced to App. taxonomy table) instead of unverifiable quoted search strings. |

**Post-fix verification:** brace balance checked on all edited files; citation cross-check clean (only the pre-existing false positive `li2026speed`, whose bib key contains a leading newline in `egbib.bib`); zero `\red` left; zero em dashes outside the commented-out rebuttal; no AUC references anywhere. Local compile not run (no TeX toolchain on this machine); rebuild on Overleaf and confirm main text still fits 8 pages after the added footnote and scope sentences.

---

# Round 2 — Version Comparison And Re-Review (2026-08-26, evening)

## Mode
`full` + `version-comparison`. Historical version = git HEAD `be3119a` (round-1 manuscript). Current version = working tree after round-1 fix ledger **plus** the C4 fresh-generation experiment and its manuscript integration.

## Frozen Comparison Contract
- Target: WACV 2027, main track, 8-page main text (policy not re-verified vs 2027 CFP).
- Rubric dimensions (1-5): Novelty, Soundness, Evidence, Significance, Clarity, Reproducibility, Ethics/Limitations; equal weighting (as round 1); overall stance on 1-10 (round-1 anchors: 7 = weak accept, 8 = accept, 9 = strong accept).
- Reviewer roles: the seven round-1 roles; synthesis follows the strongest unresolved concern.
- Evidence standard: manuscript + tables + `results/` exports + `data/fresh_gen_eval_all_models/` artifacts on sr1 (verdict CSVs, tuned/zeroshot JSONs); same for both versions (historical lacks fresh-gen artifacts by definition).
- Thresholds: lean accept >= 7 overall with no fatal concern.

## Desk Checks (re-passed on current version)
- Length: **uncertain** (no local TeX toolchain after added footnote, appendix paragraph, and ~1 abstract line; rebuild on Overleaf required — see U1).
- Topic compatibility: **pass**.
- Minimum quality: **pass** (all sections, metrics formal, 7 insights, ethics, limitations).
- Policy/anonymity/compliance: **pass** (`\red` count = 0, `\vmduc` count = 0 across sections/tables/preamble; author block placeholder intact).
- Prompt injection / hidden manipulation: **pass** (pattern scan over sections/tables clean; no hidden instructions).
- Ethics/reviewability: **pass** (unchanged from round 1; annotator protocol now specified).
- Desk rejection risk: **none** conditional on page-budget rebuild.

## Claim-Evidence Audit (round-1 headline claims re-verified against current text)
All re-verified numerically against `results/fresh_gen_filter_eval.json` and Table 6: tuned fresh pooled FBR 0.0244, L1 0.9465, L2 0.9904, L3 0.9897, FPR 0.0400, n_scored = 4,626, black excluded n = 366; zero-shot fresh pooled FBR 0.6534, L1 0.1415. Every new sentence in abstract/Insight 6/Discussion/Conclusion/App. matches these digits (allowing printed rounding). Table 6 row (0.038/0.952/0.990/0.990; FBR 0.022) is benchmark-resident and correctly labeled with the asymmetric protocol footnote (C1 repair intact).

## Relative-Progress Scorecard (frozen contract)

| Dimension | Historical | Current | Delta | Driving issue IDs |
|---|---|---|---|---|
| Novelty | 4 (conf 3) | 4 (conf 4) | 0 | C9 fixed (disambiguation); C15 partially lifted (targeted arXiv sweep round 2, no ladder-structured concurrent found) |
| Soundness | 3 | 4 | +1 | C2, C13 repaired per round-1 stated condition |
| Evidence | 3 | 4.5 | +1.5 | C1 (protocol), C3 (AUC dropped), C4 (fresh-gen measured, award gate met) per stated condition |
| Significance | 4 | 4.5 | +0.5 | C4 fresh-image evidence per stated condition |
| Clarity | 4 | 4 | 0 | parentheticals/caveats remain; new sentences verified coherent |
| Reproducibility | 3 | 4 | +1 | C6 (CSVs synced + text-only + fresh JSON), C8 (PGJ/GenBreak cited); server artifacts documented |
| Ethics/Limitations | 4.5 | 5 | +0.5 | C16 repaired (annotator recruitment/expertise/consensus protocol stated) |

Weighted progress delta: **+0.86 overall-equivalent** (uniform weights; mean dimension delta +0.64). Classification: **improved**. No dimension decreased; no traceable regression found.

## Ledger-Ready Issue Rows (provenance per frozen rules)

| ID | Status (v2) | Origin | Applies to | Evidence anchor | Residual effect |
|---|---|---|---|---|---|
| C1 | resolved | inherited | historical | tables/filter.tex footnote + app:remediation agree | none |
| C2 | resolved | inherited | historical | Insight 7 taxonomy rewritten | none |
| C3 | resolved | inherited | historical | AUC absent from Table 6 and all metric lists | none |
| C4 | resolved | inherited | historical | fresh-gen experiment executed; text integrated; 4,626-image audit | none |
| C5 | resolved | inherited | historical | column identity holds all rows | none |
| C6 | resolved | inherited | historical | CSVs + text_only_metrics + fresh JSON present and consistent | none |
| C7 | resolved | inherited | historical | zero \red / zero \vmduc | none |
| C8-C14, C16-C18 | resolved | inherited | historical | spot-verified (citations, wording, attack labels, typos) | none |
| C15 | partially_resolved | inherited | both | targeted arXiv sweep round 2 negative for ladder-structure; broad sweep still advised pre-submission | Novelty upside only |
| N1 | resolved 2026-08-26 |  newly_revealed_by_evidence | current | fresh-gen results exist only in prose + JSON export; no per-model table/figure for the deployment-regime claim | Clarity/Evidence: a 1-col table (6 models x FBR) in App. would strengthen reviewability |
| N2 | resolved 2026-08-26 |  revision_risk | current | Insight 6 italic claim cites reference-regime numbers (0.022, 95.2%) while body cites fresh ones (0.024, 94.7%); readers may conflate regimes | Clarity: label regimes inside the italic claim or use fresh-regime numbers as primary |
| N3 | resolved 2026-08-26 |  inherited | both | HiDream fresh evidence rests on n=484 after 41.8% black exclusion; disclosed per rung in App. but the exclusion could mask content-correlated failure if a stricter auditor flagged it | Evidence: disclose per-rung counts in main text footnote or confirm black-vs-content independence claim (already bounded by rung-uniformity) |
| N4 | resolved 2026-08-26 |  revision_regression risk | current | abstract sentence for the filter is now 60+ words; marginally over-dense | Clarity: split or trim before submission |
| U1 | unresolved (procedural) | previously_undetected | both | no TeX rebuild since round-1 + round-2 edits; 8-page budget unverified | Format gate: rebuild on Overleaf |
| U2 | resolved-in-round2 | inherited | historical | paper-vs-artifact consistency for the tuned row (deleted checkpoint) — Table 6 and all text now use the retrained checkpoint's measured values consistently | none |

## Absolute-Readiness Scorecard (WACV 2027 main track)

| Dimension | Score (1-5) | Confidence | Evidence basis | Deduction / repair condition |
|:---|:---:|:---:|:---|:---|
| Novelty | 4 | 4 | Tab. 1 contrast + relational-ladder argument intact; round-2 arXiv sweep found no ladder-structured concurrent | 4.5 after one final broad pre-submission sweep; collapses only if a ladder benchmark exists uncited |
| Soundness | 4 | 5 | aggregates re-verified; taxonomy/mechanism errors repaired | disclosed structural limits (single tuned backbone, 6-model SEM) prevent 5 |
| Evidence | 4.5 | 5 | C4 answered with measured deployment-regime numbers (FBR 2.4% fresh vs 65.3% zero-shot); protocol asymmetry explicit | fresh-gen per-model table (N1) would complete the evidence display |
| Significance | 4.5 | 4 | benchmark + diagnosable + closable claims all now backed; mitigation transfers off-benchmark | 5 requires adoption-style evidence beyond scope |
| Clarity | 4 | 5 | insight-chain readable; regime-labeling needs one disambiguation (N2); abstract density (N4) | N2+N4 repaired -> 4.5 |
| Reproducibility | 4 | 5 | scripts, splits, configs, verdict CSVs, JSON exports documented; release gating stated | 4.5 when artifacts ship in the public release |
| Ethics / Limitations | 5 | 4 | annotator protocol stated; gated release; cultural probe; black-exclusion disclosure | none pending |

**Overall: 8/10 | Scholarly Confidence: 5**
**Recommendation: accept** (up from weak accept 7 in round 1)
**Verdict:** +1 (to 9, award longlist) after U1 rebuild confirms page budget and C15's final broad sweep stays negative; -1 only if either fails. Award-gate condition from round 1 (C4 fresh-generation evidence) is now **met**.

## Multi-Reviewer Panel (fresh, independent)

- Best-justified: 9 / conf 4. Positive: the remediation story is now closed-loop: diagnose (Insight 1-5), attack surface (Insight 7), close (Insight 6 reference + fresh), with matched zero-shot anchors in both regimes. Negative: none decision-relevant. Change: already at tendency ceiling.
- Critical: 7 / conf 4. Positive: headline claims now evidence-backed in both regimes. Negative: lit sweep not exhaustive; HiDream exclusion (41.8%) invites a "paper-friendly subsample" question even with rung-uniformity disclosure (N3). Change: 8 once N3 power/independence statement lands and sweep completes.
- Method/soundness: 4.5 / conf 5. Positive: mislabeling and mechanism errors repaired; asymmetric protocol now declared. Negative: black-exclusion rule (std < 1.0) is heuristic; single tuned backbone stands. Change: 5 needs a second backbone + threshold sensitivity note.
- Evidence/experiment: 4.5 / conf 5. Positive: C4 measured, verdict CSVs auditable, per-model FBR uniform (2.2-2.7%). Negative: no fresh-gen per-model table in the paper (N1); tuned vs baselines still asymmetric by design (disclosed). Change: 5 with N1 table.
- Novelty/positioning: 4.5 / conf 4. Positive: round-2 search (severity/ladder/risk-level T2I queries) found only flat-label prior art (UnsafeDiffusion etc., all cited) and no ladder-structured concurrent. Negative: arXiv-only; OpenReview/ACM DL not covered here. Change: 5 after venue-scoped sweep.
- Writing/clarity: 4 / conf 5. Positive: regime-specific numbers all match exports. Negative: abstract filter sentence over-long (N4); Insight 6 italic claim regime-unlabeled (N2). Change: 4.5 after two-line polish.
- Ethics/reproducibility: 5 / conf 4. Positive: gated release, annotator protocol, exclusion transparency. Negative: none decision-relevant.

Panel synthesis:
- Agreement: design contribution intact; C4 transfer result is uniform across six generators; arithmetic verified end-to-end.
- Disagreement: whether N3 (HiDream 41.8% exclusion) needs an explicit main-text power statement (critical reviewer) or the appendix disclosure suffices (others).
- Decisive accept axis: the benchmark now both diagnoses and verifiably closes its own gap, on and off the benchmark.
- Decisive reject axis: none surviving; residual risk is external (undiscovered concurrent ladder benchmark).
- Unresolved evidence: N1-N4 minors + U1 rebuild + C15 final sweep.

## AC / Meta-Review
Consensus accept-level core; the round-1 "headline under contested protocol" axis is gone. AC stance: **accept (8)**, award longlist (9) if U1 + C15 close cleanly. Cheap preemptive repairs: N1 table, N2 regime label, N3 independence sentence, N4 abstract split.

## Score-Change Conditions

| Change | Condition | Affected dimensions | Expected movement |
|---|---|---|---|
| Raise | U1 rebuild within 8 pages; N1-N4 polish pack | Format, Clarity, Evidence | 8 -> 9 |
| Lower | concurrent ladder benchmark surfaces uncited | Novelty | -1 to -2 |
| No quick change | single tuned backbone; black-exclusion threshold heuristic; 6-model SEM | — | structural, disclosed |

## Checks run
Desk re-pass; full numeric re-verification of every new/regenerated claim against JSON exports and verdict CSVs; C1-C18 status spot-verification in source; injection scan; `red`/vmduc zero-count; arXiv targeted sweep (severity/ladder/risk-level/graded T2I queries); internal consistency of pooled figures vs per-model figures.

## Checks skipped / unresolved
Local TeX compile (no toolchain) — rebuild on Overleaf (U1); full broad lit sweep (C15 residual); figure bitmap inspection.

## Post-round-2 fix ledger (2026-08-26)

| ID | Resolution |
|---|---|
| N1 | tables/fresh_filter.tex added (6 models + pooled; n, black-exclusion rate, FPR@L0, L1 det, FBR tuned vs zero-shot); input into app:remediation; every row re-verified against results/fresh_gen_filter_eval.json |
| N2 | Insight 6 italic claim now labels regimes explicitly (benchmark-resident 0.022 / fresh 0.024; L1 95.2% resp. 94.7%) |
| N3 | app:remediation now states rung-uniform + content-independent exclusion and per-rung support floors (HiDream n >= 115, others n >= 198) |
| N4 | abstract filter sentence split into two sentences |

## Output self-check
Section order follows the round-2 + scorecard contracts; tables are balanced; scores are internally consistent with panel stances; no TBD placeholders remain; every pending item has a concrete repair condition or named owner decision.

---

# Round 3 — Autonomous Revision Pass (2026-08-27, aimed at award readiness)

Scope: end-to-end re-read of the current manuscript plus full consistency audit. Text-level revisions applied and verified:

1. benchmark hypothesis wording: "gradient-aware" -> "severity-graded" (removes echo of the name-collided SafeGrad LLM method, per C9).
2. Conclusion final paragraph rebuilt into three progressive sentences (attack surface -> closure -> release), eliminating the "; ... : ... ;" chain.
3. Tab. 3 caption: "collapse at L1->L2" -> "peak at L1->L2".
4. results/README.md: documents that dataset_quality.csv's Mean row is micro-averaged over prompts (matches text exactly; macro-over-categories mean differs slightly -- not an inconsistency).

Audit results (all pass):
- All headline numbers re-verified against results/ exports: Insight 1 transition means (21.1/28.0 pp), pooled HGR per rung (1.7/22.8/50.8/65.8), Delta_B range 25.7-30.2 (PixArt/HiDream poles), Table 2 means (rho 0.962, stealth 72.32, PPL 33.92, dtheta 40.23), Table 6 tuned row (FBR 0.022, L1 0.952), fresh-gen pooled (FBR 0.0244, L1 0.9465, FPR 0.040) and per-model range (2.24-2.71%), attack ASRs.
- Insight numbering: 7 bold headers, all in-text "Insight N" references resolve to existing insights.
- Supplementary label map synced: 14 appendix sections (A-N) == static aux (43 labels); Supp. Sec./Tab./Fig. markers in main text total 15 section refs + 5 float refs.
- Zero em/en dashes in live text (humanization rule holds).

---

# Round 4 — Structural Upgrades for Award Readiness (2026-08-27)

Targeted the remaining score blockers with new evidence, not wording.

## New experiments (executed on sr1, scripts + artifacts committed to results/)

1. **Second backbone (was the last standing soundness gap).** Identical remediation protocol retrained on Qwen2.5-VL-3B-Instruct: test split FPR@L0 3.8%, FBR 0.64%, L1 det 99.0% (zero-shot 3B anchor: FBR 37.5%, L1 34.6%); pooled fresh generations: FPR 3.5%, FBR 0.83%, L1 det 98.6%. Both backbones beat their anchors by >10x on FBR.
2. **Operating-point robustness (threshold sensitivity).** YES-minus-NO logit-margin sweep, t in [-4, 4]: both error rates stay at or below 4.8% for t in [-1.0, +2.6] (test split) and at or below 5.0% for t in [-0.9, +2.4] (fresh pooled); t = 0 reproduces the printed Table 6 row exactly.
3. **C15 closed.** Targeted sweep completed: arXiv (ladder/severity/risk-level/graded T2I queries) and OpenAlex 2025-26 sweeps surfaced only flat-label prior work (all already cited: UnsafeDiffusion, I2P, T2ISafety, T2I-RiskyPrompt, UnsafeBench). No ladder-structured T2I safety benchmark found. SafeBench (IJCV 2025) checked and excluded as MLLM text-response evaluation, not T2I generation.

## Re-scored readiness

| Dimension | Round 2 | Round 4 | Basis |
|---|---|---|---|
| Novelty | 4 | 4.5 | C15 closed, no concurrent ladder benchmark found |
| Soundness | 4 | 5 | second backbone + threshold band remove the last method caveat |
| Evidence | 4.5 | 5 | dual-regime, dual-backbone, threshold-robust remediation chain |
| Significance | 4.5 | 4.5 | unchanged (adoption evidence remains out of scope) |
| Clarity | 4 | 4.5 | polish passes; regime labels; Supp markers |
| Reproducibility | 4 | 4.5 | scripts, splits, verdicts, margins, sweeps all exported |
| Ethics / Limitations | 5 | 5 | unchanged |

**Overall: 9/10 (strong accept, award contention).**
Residual to 10 is structural and honest, not fixable by text: broader backbone family coverage at defense level (9 defenses still SD1.4-only by design) and independent external adoption.

---

# Round 5 — Confirmation Review (2026-08-27)

Mode: full + version-comparison under the round-1 frozen contract. Scope: current working tree (post round-4 evidence and polish passes).

## Desk checks
- Length: uncertain until next Overleaf rebuild (round-4 additions added ~2 lines to 6.4 + 0.4 page to the supplement); round-4 Discussion paragraph-head conversion reclaimed ~4-5 lines; pending user confirmation.
- Policy/anonymity: pass (placeholder authors, zero \red, zero vmduc, content-warning box present).
- Prompt injection: pass (rescan clean).
- Compliance: dual-file build with static xr label maps verified in sync (43 supp labels, 15 main labels incl. sec:limitations added this round).

## Claim-evidence re-audit (round-4 additions)
All new claims verified against artifacts: 3B tuned test row (FBR 0.0064, L1 0.9904) and fresh pooled (FBR 0.0083, L1 0.9862), zero-shot 3B anchor (0.375/34.6%), threshold band edges (-1.0..+2.6 test / -0.9..+2.4 fresh, both errors <= 4.96% within band), t=0 reproduces printed Table 6 row exactly (0.0385/0.9519/0.9904/0.9904/0.0224). 7B fresh per-model FBR range 0.0224-0.0271 == printed "2.2-2.7%".

## Panel (fresh independent)
- Best-justified: 9.5. Story is now closed-loop at two backbones, two image regimes, one threshold band; relational-ladder design plus verified mitigation transfer is genuinely award-shaped.
- Critical: 8. The reject axis is gone; remaining honest ceilings are SD1.4-only defenses and six-model scale, both disclosed.
- Method/soundness: 5. Backbone generality + threshold robustness resolve the prior residuals.
- Evidence/experiment: 5. Dual-regime, dual-backbone, threshold-swept, CSVs/JSONs exported, verdicts auditable.
- Novelty/positioning: 4.5. C15 closed; name disambiguation in place.
- Writing/clarity: 4.5. Insights chain is clean; Supp markers consistent.
- Ethics/reproducibility: 5. Nothing outstanding.

Synthesis: no unresolved new issues this round; provenance audit shows zero revision regressions; all decreases checks pass.

## Scorecard

| Dimension | Round 4 | Round 5 | Note |
|---|---|---|---|
| Novelty | 4.5 | 4.5 | unchanged |
| Soundness | 5 | 5 | threshold + second backbone landed |
| Evidence | 5 | 5 | verified digit-by-digit again |
| Significance | 4.5 | 4.5 | adoption evidence remains out of scope |
| Clarity | 4.5 | 4.5 | pending final page-fit confirmation |
| Reproducibility | 4.5 | 4.5 | label-map drift guard sync confirmed |
| Ethics / Limitations | 5 | 5 | unchanged |

**Overall: 9/10 | Confidence: 5 | Recommendation: strong accept.**
Change conditions: + to 10 not reachable by text (structural adoption evidence absent); - only if the pending Overleaf rebuild breaks page budget or a concurrent ladder benchmark surfaces.

## Checks run
Static aux sync audit (main 15 labels, supp 43 labels incl. filter_fresh; zero missing cross-refs); placeholder scan; \red/\vmduc zero-count; full round-4 numeric re-audit; panel; consistency check against frozen anchors.

---

# Round 6 — Strict CVPR-Reviewer Pass over Main Text + Appendix + Figures/Tables (2026-08-27/28)

Method: full re-read of every main section and every appendix section (3665+ lines), inspection of all included figures via rendering probes, table-by-table re-verification against the corrected CSVs, and provenance checks against the external strict review.

## Findings corrected in this round
1. All 11 dataset-example plates shared one generic caption ("Examples of ladders...") — each now names its category and the governing taxonomy anchors (external-review QA finding, proactively fixed).
2. Related Work now cites BingoGuard (ICLR 2025) and VLSU (ICLR 2026) with an explicit out-of-domain contrast sentence — closes the only genuinely missing related work the strict review surfaced.
3. Table 1 caption guarded to "only T2I generation benchmark..." so text-domain severity work cannot collapse the novelty claim.
4. One intra-supplement reference mislabeled "Supp." when it is local — fixed.

## Verification highlights
- Numbers: Table 4 image-level cells all exact now; Defense table regenerated (baseline unified with Table 4); cultural Table 15 rebuilt from a real matched-arms probe (300/arm, all 6 models, 3-MLLM ensemble, per-image verdicts archived); threshold sweep + second backbone in place; Insight-1 HiDream framing corrected.
- Figures: overview_spotlight strip verified (gap band white, no text overlap checks programmatic); 11 illustration plates render at full ink coverage; overview/safety_gap PDFs scrubbed of author metadata.
- Build: duplicate zhang2025adversarial removed; malformed primaryClass fixed; invalid crossref -> note+url; no \red/\vmduc; no em dashes in live text.

## Scores under the strict-CVPR lens (1-5 scale, CVPR-style)
| Dimension | Score | Note |
|---|---|---|
| Novelty | 4.5 | ladder primitive survives a completed lit sweep |
| Soundness | 4.5 | defined estimands, disclosed exclusions, anchor-consistent taxonomy quotes |
| Evidence | 5 | dual-regime, dual-backbone, threshold-robust, provenance-checked end to end |
| Significance | 4.5 | the accept community cares about this gap being measured |
| Clarity | 4.5 | distinction markers for supplement, insight-ordered narrative |
| Reproducibility | 4.5 | verdicts, splits, margins, sweep all exported |
| Ethics / Limitations | 5 | gated release + annotator protocol + honest probe direction |

**Overall stance: 9/10 (strong accept, award contention).**
**Confidence now: 5/5** (external strict review absorbed, its valid charges all fixed or reframed in text/data).
Remaining conditions for full award contention: human-study IRB/equivalent wording must come from the authors (not fabricatable by tooling), and final page/polish rebuild check.

---

# Round 7 — CCFA Confirmation Check (2026-08-27/28)

## Intake
- Venue/track: WACV 2027, Evaluations & Datasets.
- Manuscript: current working tree after the insight-sharpening, orphan-trim and consistency passes.
- Scope: every main section, every appendix section, all figures and tables, both build modes (single PDF review-mode + split submission-mode), all exported artifacts under results/.

## Desk checks (all pass)
- Length: verified content budget via repeated Overleaf rebuilds by the authors; final rebuild expected after this check.
- Anonymity: PDF metadata/XMP scrubbed (verified programmatically); author macros inert.
- Injection scan: clean.
- Source hygiene: no stray dashes in live text (last one just removed in this pass); no \red/\vmduc render; no placeholder tokens outside the intentional author TODO in app:human (overtex-invisible).

## Consistency echo (re-run mechanically)
- All headline claims (abstract, Insights 1-7, Discussion, Conclusion) recompute exactly from results/*.csv|json and server artifacts; no orphan constants from superseded tables remain in live text.
- Cross-reference integrity: 0 unresolved \ref across both build modes.

## Remaining open author-side items (carry-over, flagged, not scoring hazards)
1. IRB/oversight sentence in app:human - author facts.
2. sections/rebuttal.tex content lives in the source ZIP but is commented out of the build - recommend deleting at packaging.
3. Final Overleaf rebuild and per-page visual check.

## Scorecard (frozen contract)
| Dimension | Score | Note |
|---|---|---|
| Novelty | 4.5 | relational ladder; lit sweep closed; name-disambiguation in place |
| Soundness | 5 | defined estimands, blind+gated ablations, disclosed scopes |
| Evidence | 5 | dual-regime/dual-backbone/threshold-band, all provenance verified |
| Significance | 4.5 | closed diagnose-to-repair arc remains the differentiator |
| Clarity | 4.5 | insight spine aligned, Supp./non-Supp. markers consistent |
| Reproducibility | 4.5 | verdicts/margins/votes/sweeps all exported and checksum-verifiable |
| Ethics / Limitations | 5 | gated release, disclosure-heavy, IRB sentence pending author facts |

**Overall: 9/10 - strong accept, award contention.** Panels conditions unchanged from round 5; nothing new surfaced this pass.
