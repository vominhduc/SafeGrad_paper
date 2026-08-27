# Ladder-Aware Filter Training (Remediation Validation)

Status: **EXECUTED (2026-08-26/27).** This file was the original plan document;
results now exist and supersede the plan. Current artifacts: Table 6 tuned row
(7B, test split), second backbone (3B, test split + fresh generations),
threshold sweep, and fresh-generation evaluation exports — see this directory's
`filter_eval.csv`, `filter_second_backbone.json`, `fresh_gen_filter_eval*.json`,
and `filter_threshold_sweep.json`. The numbers in this plan file below are
**historical placeholders** and must not be quoted.

## Motivation
The paper's findings are currently diagnostic-only. This experiment converts
SafeGrad from a measurement artifact into an enabling one: if a filter trained
on ladder data detects L1-L2 content that zero-shot filters miss, the benchmark
closes the gap it diagnoses (candidate award-level differentiator).

Requires NO T2I regeneration: it uses only existing reference images and the
existing MLLM ensemble.

## Setup
- Data: SafeGrad reference images. Split ladders (not images) 80/10/10 into
  train/val/test so that all four rungs of a ladder stay together
  (prevents subject leakage across splits).
- Model: Qwen2.5-VL-7B (same family as the weakest filter baseline) with LoRA
  on the vision-language projection; input = image + the ladder's risk
  explanation for the target rung (severity-aware filter, mirroring the
  evaluator protocol in Sec. 5).
- Training pairs: (image, risk description) -> YES/NO severity verdict.
  Oversample L1-L2 so the split is level-balanced across rungs.
- Baselines (from Table 6, values already published): Q16, CLIP-NSFW,
  SD Filter, LlavaGuard, zero-shot Qwen2.5-VL.

## Evaluation
- Metrics identical to Tab. 6: per-level detection rate (L0-L3), FPR@L0,
  FBR, severity sensitivity (L3-L0).
- Evaluator for verdicts: same non-blind MLLM ensemble as Sec. 5 for the
  filter's own outputs is NOT needed — the filter returns YES/NO directly;
  ground truth = benchmark level labels; spot-check against human labels
  from the 500-pair study subset in the test split.
- Also report per-family breakdown (NSFW / Legal / Social).

## Success criteria
1. Detection at L1 and L2 rises materially above zero-shot Qwen2.5-VL
   (current FBR = 0.880), without inflating FPR@L0 beyond Q16's (0.553).
2. Severity sensitivity (L3 minus L0 detection) exceeds all five baselines.
3. Failure analysis: which categories resist ladder-aware training
   (expect Copyright/PID per Discussion).

## Risks / honesty notes
- Training on reference images (constructed to clearly manifest severity)
  favors the trained filter; report the same disclaimer used for Tab. 6
  (lower-bound statement) symmetrically.
- Do NOT test on model-generated images from Tables 4-5 (those logs are lost);
  the test split is benchmark-resident only.
- If results are negative, report them as-is; the diagnostic contribution
  stands independently.
