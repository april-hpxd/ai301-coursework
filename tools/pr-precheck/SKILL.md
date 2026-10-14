---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: **is this ready to submit?** A PR package is a candidate pull request — title, description, commits, branch diff, and test evidence — read against the plan it claims to implement and the issue that plan belongs to. Do not grade a different object, do not grade more than one package per run, and do not answer from gut feel. Execute the components in this directory.

## Inputs and modes

Run in exactly one of these two modes:

- **Live mode**: grade the student's own draft before it is opened. Inputs, and where each comes from:
  - The issue: the issue URL named in the invocation. Read the live issue thread for title, body, labels, and highlights.
  - The plan: `plan.md` in the working copy, including any `## Deviations` notes. A house-chain student reads the house plan (and the house repro pack quoted there) instead of their own posted plan; the same checks still grade the same things.
  - The branch diff: everything this branch changes relative to the repository default branch. Produce it with `git diff main...HEAD` (three dots) from the working copy. That command's output is the diff the checks read — not an unstaged working-tree diff, not `git diff HEAD`.
  - The draft PR title and description: `pr_draft.md` in the working copy (or the title and body the student names in the invocation).
  - Test evidence: `test_evidence.md` in the working copy (commands run, before/after output, repo-check results).
  - Repo standards: the repository PR template (typically `.github/PULL_REQUEST_TEMPLATE.md`) and `docs/CONTRIBUTING.md` (or the contributing guide the repo names). Gather these live from the working copy / GitHub; do not invent template asks.
- **Eval mode**: a package bundle is the whole world. Every fact comes from the bundle text (issue context, thread highlights, repo-facts block, plan-context block, candidate PR title/description/commits/diff/test-evidence). Fetch nothing. Read nothing else. Grade every check with the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything else. Apply the `Repo:` line and the house rules there. Refuse to grade a PR that does not target the scoped repository, and say so. If the `Repo:` line still carries an unfilled placeholder (`<ORG>/<PATH-REVIEW-REPO>` or similar), stop without grading and tell the student to fill the `Repo:` line in `scope.md` with their section's Path Review repo; never guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`. Hold the outgoing PR title and description against those rules. Report any broken rule in the readable summary, quoting the rule. The voice guide never changes the verdict on its own unless `rubric.md` has a check that reads it. In eval mode, ignore `voice-guide.md` entirely: voice is personal and carries no gold labels; universal communication-quality checks live in the rubric.

## Component reads

- `rubric.md` defines the checks (name, evidence, pass condition, weight) and the verdict rule.
- `references/evidence-guide.md` maps where each evidence family lives in a PR package (eval bundle vs live files) and what acceptable evidence looks like.
- Execute `procedure.md` as written for read order, evidence gathering, check execution, and verdict assembly.

If the procedure is silent on a step, report that gap in the summary; do not silently invent a step. If `rubric.md` has no checks filled in, or `procedure.md` has no steps filled in, refuse to grade and say so. Template HTML comments are not content.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject` (hold). There is no third verdict and no score. Reservations belong in check evidence lines, not in the verdict.

End the reply with the fenced JSON block below, valid and last, with nothing after it. A readable per-check summary may come before the block. The `item` field is the PR URL in live mode, or the bundle id in eval mode.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Grade no check without naming the fact or quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse complete PR can be ready and a beautiful confident one can be hiding drift. Read the diff against the plan, the test evidence against the plan's test plan, and the description against the diff — never the formatting of the write-up.
- **The rubric decides, not the run.** If a check passes by its stated condition but feels wrong, it still passes. The fix belongs in the rubric, not in this run.
- **The procedure decides how, not the run.** Follow `procedure.md` as written and report its gaps instead of inventing steps.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's verdict rule directs. Where the rule is silent, an unverifiable claim is a failing one. A PR you cannot verify from the package is a PR that is not ready to submit.
