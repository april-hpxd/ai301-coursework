# Procedure: how this tool grades a PR package

## Read order

1. In live mode only, read `scope.md` first and apply the scope seam from SKILL.md. In eval mode, skip this step.
2. Read the **plan-context** (live: `plan.md`) before opening the diff. Note: the In/Out scope, named files, promised deliverables, named deferrals/deviations, and the test plan (commands and failure modes). Also note the repro evidence the plan built on (the original trigger and expected-after).
3. Read the **unified diff** and commit list next. List every changed file and, for each hunk, whether it is the planned fix, a required test/changelog the plan named, or something else (rewrite, new option, debug, churn).
4. Read the **PR title and description**. Note fidelity claims ("exactly as planned", "docs now document", "no functional changes outside X"), close-issue lines, checklist items, and any disclosed shortfall.
5. Read the **test-evidence** section last (live: `test_evidence.md`). Note which plan steps have a shown command and output, which are only narrated, and which are missing.
6. Read **repo-facts** (live: PR template + contributing/AI policy) for required template asks and whether AI disclosure is required.

This order exists so the three side-by-side checks are possible: diff vs plan, evidence vs test plan, description vs diff. Reading the description first invites grading polish instead of the change.

## Evidence gathering

Record facts as short quotes or file lists; do not grade yet.

**Plan fidelity (silent-drift):**
- Plan In-files / Out-of-scope / deferrals.
- Diff file list and extra behavioral edits (new flags, config keys, unrelated rewrites).
- Description sentences that assert completeness or extra/absent work.

**Test evidence (not-tested):**
- Each test-plan command or failure mode the plan named.
- For each: the command shown, the before output (or named before), the after output.
- Any suite command the plan named (`yarn test`, `pytest …`, ASAN, etc.) and whether a transcript exists.
- Whether the shown trigger is the issue's path or a control/unchanged path.

**Diff quality (unreviewable):**
- Debug prints, commented-out blocks, dead functions, re-indent-only hunks, unrelated files.
- Commit subjects if they show `wip` / `misc cleanups` paired with debris in the diff.

**Standards and comms (standards-wall):**
- Template asks from repo-facts (closes #, checklist, whatsnew/changelog, Problem/Solution, etc.).
- Whether each ask is met in the description or, for file asks, in the diff.
- AI policy text, if any, and whether the description discloses AI use.

## Check execution

Execute checks in rubric-table order. Grade from the gathered notes; re-read a section only when a quote is needed for the evidence line. If evidence for a check is genuinely absent, grade `unclear` (not `pass`).

1. **Diff matches plan plus deviations.** Compare the diff file/behavior list to plan In/Out and deviation notes. `pass` if the diff is the plan (plus disclosed deferral). `fail` if the diff does more than the plan with no note, or less than the in-scope deliverables with no note.
2. **Description matches the diff.** If the description claims docs/changelog/extra files/"exactly the plan"/"no extra changes", verify against the diff. `fail` on a contradiction. `pass` if claims are no stronger than the diff.
3. **Test evidence is decisive.** For each in-scope plan failure mode, require a shown trigger and observable result. `fail` on narration-only, control-path-only, or a missing named mode with no deferral. `unclear` if there is no test-evidence section.
4. **Named verification commands were run.** `pass` if every plan-named verification command has an outcome in the evidence, or an honest cannot-run with a substitute shown. `fail` if the plan named a suite/repro command that never appears. Skip (grade `pass`) when the plan named no extra suite beyond the repro already covered in check 3 and the description does not claim other checks.
5. **Diff is reviewable.** `fail` if debris or unrelated hunks are present. `pass` if the change is the planned files plus their tests/changelog without leftover debug or churn.
6. **Template and policy honored.** `fail` if a stated required ask is empty/missing, or if AI disclosure is required and absent. `pass` if asks are filled with real content and AI rules are met or inapplicable.

## Verdict assembly

Apply the rubric verdict rule: `accept` only when every required check is `pass`; any required `fail` or `unclear` yields `reject`.

In the readable summary, quote the deciding check: the first required check in rubric order that is not `pass`. If all pass, quote the plan-fidelity check's key fact (diff files vs plan files). Put one-line evidence in each JSON check object. Then emit the fenced JSON block last, with nothing after it.
