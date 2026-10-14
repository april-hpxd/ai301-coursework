# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives in an eval package:** the plan-context block (scope, named files, Out-of-scope, known limits / deferrals); the candidate PR's unified diff and commit list; the description's fidelity claims.

**Where it lives in live mode:** `plan.md` (Cause / Scope In-Out / Approach / `## Deviations`); `git diff main...HEAD` from the Path Review working copy; title and body in `pr_draft.md`.

**What acceptable evidence looks like:** every changed path is a file the plan named (or a deviation note added). A one-line theme deletion that matches a two-step plan is enough. An extra `truncate()` rewrite, a new config flag the plan scoped out, or a description that says "docs now document X" when the diff has no docs file is silent drift. An honest deferral already written in the plan and restated in the description (for example "%VAR% expansion not covered") is not drift.

**Missing / contradictory / ambiguous:** if the plan has no file list, infer In-scope only from the named components and treat extra files as drift. If the description and the diff disagree, believe the diff and fail the description-fidelity check. Do not fetch the live GitHub issue to "correct" a frozen bundle.

## Test evidence (harness category: not-tested)

**Where it lives in an eval package:** the candidate PR `### Test evidence` section, read against the plan-context test plan and the repro evidence quoted there.

**Where it lives in live mode:** `test_evidence.md` next to `plan.md`; the plan's Test plan section; Unit 2 reproduction steps re-run against `main` (before) and the implementation branch (after).

**What acceptable evidence looks like:** a named command plus output for the issue's actual trigger, before and after (or after plus a before the package already recorded). Example: configparser raising `DuplicateOptionError` on main and `parsed OK` on the branch. "cargo test passes", "tested locally, colors work now", or "verified working for a full day" with no repro transcript is not decisive. Running only the control path (the single-file case that never broke; a GET when the bug is a POST) is not the test plan.

**Missing / contradictory / ambiguous:** no test-evidence section → `unclear` on the decisive-evidence check. Plan names two failure modes and evidence covers one with no note → fail. Description claims `make test-unit` / `yarn test` but the evidence never shows it → fail the named-commands check. An honest "integration tests: 0 collected / no tests ran" is acceptable when that is what the command produced.

## Diff quality (harness category: unreviewable)

**Where it lives in an eval package:** the `### Diff` unified diff and `### Commits` list.

**Where it lives in live mode:** `git diff main...HEAD` and `git log main..HEAD --oneline`.

**What acceptable evidence looks like:** the planned edit is the bulk of the diff. Tests and a changelog the plan named may ride along. Debris tells: leftover `DEBUG`/`eprintln!`/`print(` that is not the product behavior, commented-out first attempts, dead `_unused_*` helpers, TODO on an import, identical lines re-printed as churn, unrelated files.

**Missing / contradictory / ambiguous:** a right fix plus debris still fails this family. Do not fail a small explanatory comment on the fix. If the diff is empty, the package is not ready (plan-fidelity fail and unreviewable).

## Standards and comms (harness category: standards-wall)

**Where it lives in an eval package:** the repo-facts block (PR template asks, contributing guide, AI-use policy) versus the candidate description and whether required changelog/whatsnew files appear in the diff.

**Where it lives in live mode:** `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and the filled sections in `pr_draft.md`. Path Review house rules also require the template sections to have real content.

**What acceptable evidence looks like:** required sections filled (Summary, Issue with `Closes #N`, Changes, Testing, Notes for Reviewers). If the template requires `closes #xxxx` and a whatsnew entry, both must be present — a solid ASAN-proven fix that omits them still fails this family. If the captured policy says all AI usage must be disclosed, the description must disclose it; eval packages are treated as AI-assisted work. If the policy is silent, missing AI disclosure does not fail this check.

**Missing / contradictory / ambiguous:** empty headings, unticked boxes that the evidence shows were not run (do not tick them), or a close-line with no number, fail. Tick only what the evidence supports. Whether the description's *claims about the code* match the diff belongs under plan fidelity, not here.
