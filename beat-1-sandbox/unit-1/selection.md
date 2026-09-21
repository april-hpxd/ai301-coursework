# Unit 1 — Issue Selection

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64

**Verdict output**

Live-mode output for the three open candidates ranked #64 first because it was “Test-only fix, single file, clear reproduction with explicit pytest command, behavior verified by test pass/fail.”

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64",
    "checks": [
      {"name": "Maintained and usable repository", "grade": "pass", "evidence": "Not archived; latest commit Sept 16, 2026 by Aburke225 within 180 days"},
      {"name": "AI-assisted contribution permitted", "grade": "pass", "evidence": "CONTRIBUTING.md is silent on AI policy; silence passes"},
      {"name": "Bounded, actionable contribution", "grade": "pass", "evidence": "Fix test fixture in test_query_with_partial_overlap; reproduction command provided; expected failure clear"},
      {"name": "Available to start", "grade": "pass", "evidence": "No assignee, no linked PRs, no claim comments"},
      {"name": "Maintainer response signal", "grade": "pass", "evidence": "Issue authored by Aburke225, a core contributor with recent commits"},
      {"name": "Explicit newcomer signal", "grade": "pass", "evidence": "Labeled good first issue and tier-1"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "Maintained and usable repository", "grade": "pass", "evidence": "Not archived; latest commit Sept 16, 2026 by Aburke225 within 180 days"},
      {"name": "AI-assisted contribution permitted", "grade": "pass", "evidence": "CONTRIBUTING.md is silent on AI policy; silence passes"},
      {"name": "Bounded, actionable contribution", "grade": "pass", "evidence": "Fix verify_password to return False instead of raising UnknownHashError on malformed hashes; named file and unmark xfail test H-05"},
      {"name": "Available to start", "grade": "pass", "evidence": "No assignee, no linked PRs, no claim comments"},
      {"name": "Maintainer response signal", "grade": "pass", "evidence": "Issue authored by Aburke225, a core contributor with recent commits"},
      {"name": "Explicit newcomer signal", "grade": "pass", "evidence": "Labeled good first issue and tier-1"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
    "checks": [
      {"name": "Maintained and usable repository", "grade": "pass", "evidence": "Not archived; latest commit Sept 16, 2026 by Aburke225 within 180 days"},
      {"name": "AI-assisted contribution permitted", "grade": "pass", "evidence": "CONTRIBUTING.md is silent on AI policy; silence passes"},
      {"name": "Bounded, actionable contribution", "grade": "pass", "evidence": "Align README and .env.example on LLM API key configuration; named files with clear acceptance criterion"},
      {"name": "Available to start", "grade": "pass", "evidence": "No assignee, no linked PRs, no claim comments"},
      {"name": "Maintainer response signal", "grade": "pass", "evidence": "Issue authored by Aburke225, a core contributor with recent commits"},
      {"name": "Explicit newcomer signal", "grade": "pass", "evidence": "Labeled good first issue and tier-1"}
    ],
    "verdict": "accept"
  }
]
```

## Eval iterations

**Run history**

Two complete-run attempts produced no score and did not write `eval-run.txt`: the first terminal-hosted attempt ended before the harness completed, and the second stopped at the real Windows error `UnicodeEncodeError: 'charmap' codec can't encode characters`. I reran the unchanged harness in Python UTF-8 mode. The final complete run says: `categories: claimed 4/4  clear-accept 7/8  dead-repo 3/3  policy 1/1  scope 3/4` and `agreement: 18/20 scored items  (bar: 18/20: PASS)`.

**Issue analysis**

`issue-19` was the only false negative: the rubric decided `reject`, while the gold label was `accept`. The frozen bundle says, “There are two potential causes which should be fixed,” then lists three additional suggestions including “use multi-processing” and “Applying the rewrite should also happen in a separate thread.” Under the current bounded-contribution check, that is an explicitly unsettled multi-part design rather than one concrete change with a defined target, so the required check failed.

**Check rationale**

The relevant current rubric text is: “The issue asks for one concrete change or bug fix and supplies at least one observable target ... It must not be explicitly an umbrella/tracking issue, a support question, a still-unsettled design discussion, or a request that a maintainer says requires core-internal changes.” This check prevents a newcomer from accepting a ticket whose visible wording contains several competing implementation directions but no settled, independently verifiable first change.

**Trade-offs**

That strict scope rule rejected gold-accept `issue-19`, while it correctly rejected three of four scope-category items. I kept it because lowering the threshold to accept a ticket with multiple potential causes and three architectural suggestions would weaken the stated “one concrete change” boundary and make the rubric depend on an adjective instead of observable issue text.

## Selection rationale

**Selection rationale**

1. #64 fits the available time because it is a one-file test-fixture correction with the supplied command `pytest tests/unit/test_relevance_scorer.py -q`; it provides a compact first contribution workflow.
2. The live verdict correctly identified a maintained, non-archived repository, no AI prohibition, no assignee/linked PR/claim, and a concrete named test target. The rubric does not capture personal preference; with no supplied fit profile, I weighed the executable, single-file verification path above the broader code and documentation alternatives.
3. The anticipated claiming difficulty is low: live verification found issue #64 open, unassigned, and with zero comments. The course’s Path Review house rule also says classmates’ claim comments would not block it.
