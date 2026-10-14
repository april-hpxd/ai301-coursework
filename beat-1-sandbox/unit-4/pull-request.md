# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

Not opened in this session; this branch was validated locally and left ready for review, but no GitHub PR was published without explicit approval.

**Branch**

`fix/64-partial-overlap-fixture`

## Eval iterations

**Run history**

1. `python run_eval.py --skill C:\Users\rusla\.claude\skills\pr-precheck --save-run eval-run.txt` -> agreement `18/20` scored items (`PASS`). This was the only full run and it matches the agreement line in `beat-1-sandbox/unit-4/eval-run.txt`.

**Package analysis**

I picked `pkg-05`.

- Our rubric: `reject`
- Gold label: `accept`
- Why: the package is a strong `clear-accept` candidate on file scope and plan match, but the rubric rejects it on the `Test evidence is decisive` check because the evidence did not include an actual before/after transcript for the named trigger. The package looked good at a glance but the evidence did not prove the fix from the issue's reproduction path.

**Check rationale**

Quote from the final uploaded rubric at `tools/pr-precheck/rubric.md`:

```text
| Test evidence is decisive | The test-evidence section (live: `test_evidence.md`) read against the plan's test plan and the original repro steps in the plan-context. See evidence-guide.md, Test evidence. | For each failure mode or repro step the plan named as in-scope verification, the evidence shows an observable before/after (or after plus a named before from the package) of that actual trigger — command and output, not "tests pass" or "verified on my machine". Evidence that only exercises a control/unchanged path fails. If the plan named several in-scope modes and the evidence is silent on one with no disclosed deferral, fail. A plan-noted deferral whose remaining in-scope repro is shown can pass. | required |
```

I kept this rule as written because the whole point of the course assignment is to prevent package-level judgments from being driven by a polished description alone. The rubric has to require an observed command-and-output reproduction, not just a plausible or narratively convincing claim.

**Trade-offs**

I kept the strict `Test evidence is decisive` rule even though it rejected two otherwise strong `clear-accept` packages (`pkg-05` and `pkg-19`). That was the cost of preventing narrative-only evidence from passing automatically. I explicitly re-checked those packages against the rubric and accepted the trade-off because the evaluation objective is to reward observable proof, not polish.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
