# CCFA Scientific Review — SafeGrad (Round 4: Writing Pass)

## 1. Metadata

- Review date: 2026-08-29 (late)
- Reviewer mode: **writing-only pass** (grammar, flow, reference integrity, parallelism, telegraphic compression)
- Manuscript version: commits up to `de06d71` + today's writing fixes

## 2. Method

Full re-read of all main-text sections, sentence by sentence, hunting: dangling/referent-less pronouns ("Both", "this"), forward-references that re-enter their own paragraph, ungrammatical compression artifacts left by the space-trimming rounds, em-dash overloading, slash-notation ambiguity, and bullet-list parallelism.

## 3. Findings & Dispositions (all fixed in this pass)

| # | Location | Issue | Fix |
|---|---|---|---|
| W1 | Related work | "Both still assign…" — antecedent orphaned by the intervening UnsafeBench sentence | "Generation-side safety benchmarks all still assign…" |
| W2 | Discussion ¶1 | "The handoff amplifies this" — no antecedent for "this" after paragraph merge | "The cross-modal handoff makes this worse: …" |
| W3 | §4.2 quality list | Claude relabeling was an unlabeled orphan inside a labeled bullet list | new "Independent relabeling." bullet |
| W4 | Abstract | 38-word ladder sentence with tacked-on ", and all ladders are built…without…" | split into two sentences |
| W5 | §4.1 attack list | "…and P4D, against X, plus PGJ, the most recent…" — two competing trailing phrases | single em-dash list, config after the dash |
| W6 | Insight 3 (line 51) | "With ρ = … the relative HGR reduction…" — "With X the Y" is ungrammatical | "Writing ρ = … for the relative HGR reduction…" |
| W7 | Insight 3 (line 53) | "13.1%/21.9%" slash pair — which is Legal vs Social? | explicit labels |
| W8 | Insight 3 (line 57) | "transfers beyond the U-Net backbone: … so the claim is not U-Net-specific" — says it twice | tail dropped |
| W9 | §5.2 | "SBS reads only alongside absolute HGR" — verb misuse | "should be read only alongside" |
| W10 | §6.4 fresh-gen | telegram style: "default settings, checkers disabled, ladder seeds" | restored clauses ("under each model's default settings…") |
| W11 | §6.4 backbone-sentence | three em-dashes overloading one sentence | split into three sentences |
| W12 | data_construction | stray double blank line after user's minor fix | removed |

## 4. Verified Clean

- Abstract: arc diagnosis→closure, rung labels bound at first use, all numbers trace to tables/CSVs.
- Introduction: temporal logic intact (hypothesis before artifact, findings after), insight arc ordering correct.
- Insights 1–7 italics: single-claim form preserved through all edits; no italic contradicts its own body.
- All number-bearing sentences re-verified against CSV outputs (no write-side drift).

## 5. Residual Writing Risks (not fixed, by design)

- The SLD provisional numbers (FIXME-marked) — pending aggregation; not a writing matter.
- Style is deliberately assertive ("measuring the wrong target", "everywhere we look"); consistent voice, but a cautious PC might flag one headline ("The gap is trainable-away") as slogan-like. Acceptable for the datasets track.

## 6. Score (writing dimension)

Clarity 8.5 → **9/10**. No grammar-level defects remain in main text.
