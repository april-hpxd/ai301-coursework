# Unit 4 — Test and Submit

## pull request

**Pull request**

(https://github.com/codepath/pathreview-ai301-fa26-s3/pull/232)

**Branch**

`fix/64-partial-overlap-fixture`

## Eval iterations

**Run history**

1. Full evaluation run:
   `python run_eval.py --skill "C:\Users\rusla\.claude\skills\pr-precheck" --save-run eval-run.txt`

   Result: **18/20 scored items agreed (PASS)**.

   Category results:
   - `clear-accept`: 5/7
   - `not-tested`: 4/4
   - `silent-drift`: 4/4
   - `standards-wall`: 2/2
   - `unreviewable`: 3/3

   This was the full evaluation run recorded in `beat-1-sandbox/unit-4/eval-run.txt`. No additional evaluation runs are claimed here.

**Package analysis**

I selected `pkg-05`.

- Our rubric: `reject`
- Gold label: `accept`
- Category: `clear-accept`

The package is a strong candidate for acceptance based on file scope and plan alignment. However, the rubric rejects it under the `Test evidence is decisive` check because the evidence did not include an actual before/after transcript for the named trigger. The package appeared convincing at first glance, but its evidence did not demonstrate the fix through the issue's reproduction path.

This mismatch highlights a trade-off in the rubric: requiring explicit reproduction evidence can reject a package that otherwise appears correct, even when the gold evaluation labels it as acceptable.

**Check rationale**

The following is the `Test evidence is decisive` check from `tools/pr-precheck/rubric.md`:

```text
| Test evidence is decisive | The test-evidence section (live: `test_evidence.md`) read against the plan's test plan and the original repro steps in the plan-context. See evidence-guide.md, Test evidence. | For each failure mode or repro step the plan named as in-scope verification, the evidence shows an observable before/after (or after plus a named before from the package) of that actual trigger — command and output, not "tests pass" or "verified on my machine". Evidence that only exercises a control/unchanged path fails. If the plan named several in-scope modes and the evidence is silent on one with no disclosed deferral, fail. A plan-noted deferral whose remaining in-scope repro is shown can pass. | required |
```

I kept this rule because the assignment emphasizes evidence-based package review rather than accepting a polished description at face value. The check requires observable command-and-output evidence tied to the actual reproduction path, rather than relying on general claims that tests passed.

**Trade-offs**

I kept the strict `Test evidence is decisive` rule even though it rejected two otherwise strong `clear-accept` packages, `pkg-05` and `pkg-19`. The rule prioritizes observable proof over persuasive descriptions and prevents narrative-only evidence from automatically passing.

I accepted this trade-off because the skill should evaluate whether the evidence demonstrates the claimed fix, not merely whether the package looks convincing. The `pkg-05` mismatch also shows that a stricter evidence requirement can disagree with a gold label, so the evaluation results should be considered alongside the rubric's intended behavior and the observed mismatches.