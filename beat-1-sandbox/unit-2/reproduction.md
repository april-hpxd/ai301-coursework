# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

april-hpxd

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64#issuecomment-5853268370

> Claiming this. test_query_with_partial_overlap asserts score < 0.9 for a query/chunk pair that
> shares all four query terms, so I'm going to first confirm the failure locally with pytest
> tests/unit/test_relevance_scorer.py -q, then look at whether the scorer's 1.0 output is correct
> for full coverage, which would put the fix in the fixture rather than the scorer.
> I'll report back what I find before touching anything.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64#issuecomment-5853507683

Posted text (verified against the live comment; graded `accept` by the installed `repro-check`
skill in full-package mode before posting):

> Reproduced. Environment: Windows 11, Python 3.13.14 (repo asks for 3.11+; this test has no
> version-sensitive logic, so I did not pin down to 3.11), pytest 9.1.1, structlog 26.1.0 in a
> plain venv — `tests/unit/` is marked `unit: Unit tests (fast, no external dependencies)` in
> `pyproject.toml`, so I didn't bring up the Docker/Postgres stack for this one.
>
> Steps:
>
> ```
> $ git clone https://github.com/codepath/pathreview-ai301-fa26-s3.git
> $ cd pathreview-ai301-fa26-s3
> $ python -m venv .venv-repro && .venv-repro/Scripts/pip install pytest structlog
> $ .venv-repro/Scripts/python -m pytest tests/unit/test_relevance_scorer.py -q
> ```
>
> Running the issue's exact command against the repo as shipped gives:
>
> ```
> ..x................                                                      [100%]
> 18 passed, 1 xfailed in 0.51s
> ```
>
> That's because `test_query_with_partial_overlap` carries `@pytest.mark.xfail(strict=True,
> reason="issue #64: ...")`, per this repo's seeded-bug convention (`docs/CONTRIBUTING.md`: "Many
> issues in the tracker are deliberately-planted bugs ... marked so the suite can stay green"). To
> see the actual behavior underneath the marker, I removed just that decorator locally (not
> committed) and reran the single test:
>
> ```
> $ .venv-repro/Scripts/python -m pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -v
> ...
> >       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
> E       assert 1.0 < 0.9
> tests/unit/test_relevance_scorer.py:54: AssertionError
> Captured stdout call: relevance_scored avg_score=1.0 chunks_count=1 query_len=4
> 1 failed in 0.36s
> ```
>
> Expected (per the issue): the fixture's query/chunk pair should have partial keyword overlap,
> landing the score strictly between 0.3 and 0.9.
>
> Actual: `RelevanceScorer.score` (`rag/evaluator/relevance_scorer.py`) tokenizes like this:
>
> ```python
> @staticmethod
> def _tokenize(text: str) -> list[str]:
>     return text.lower().split()
> ```
>
> and scores with `overlap = len(query_tokens & chunk_tokens); relevance = overlap /
> len(query_tokens)`. The fixture's chunk is:
>
> ```python
> {"text": "Django is a Python web framework for rapid development"}
> ```
>
> so `query_tokens = {python, django, web, framework}` (4 tokens) and all 4 are present in the
> chunk's tokens, giving `overlap/len(query_tokens) = 4/4 = 1.0` — exactly matching the issue's
> claim that the scorer is correct and the fixture is the actual bug. The chunk needs to drop at
> least one of the four query terms to produce genuine partial overlap.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Three runs. (1) First complete 20-package run: 3 packages (`pkg-04`, `pkg-07`, `pkg-11`) errored
with a genuine Windows environment bug — `UnicodeEncodeError: 'charmap' codec can't encode
character '\U0001f64f'` when the harness piped an emoji-bearing bundle to `claude`'s stdin under
the default `cp1252` console encoding — and two packages I could grade disagreed with gold:
`pkg-09` and `pkg-10`, both `clear-accept` "honest cannot-reproduce" packages my rubric's
artifact check rejected. That run scored `agreement: 15/17 scored items` (partial; the 3 errors
don't count as scored). (2) I fixed both problems: reran the harness under
`PYTHONUTF8=1`/`python -X utf8` to fix the encoding, and rewrote the "Artifact shows the issue's
own behavior" check (see Check rationale) to accept an evidenced cannot-reproduce as a pass, not
only a positive match. Since that loosens a required check, I re-ran `--only` with the 3 errored
packages plus the 2 disagreements plus one canary from the single-package `disclosure` category
(`pkg-20`, already agreeing, to confirm the loosened check didn't leak into it): `agreement:
6/6 scored items`, all matching gold, `pkg-20` still `reject` as before. (3) Final confirming
full run: `agreement: 20/20 scored items (bar: 18/20: PASS)`, with `categories: clear-accept 8/8
disclosure 1/1 no-evidence 4/4 unfollowable-comms 3/3 wrong-target 4/4` — every category has a
match. This is the run saved to `eval-run.txt`.

**Package analysis**

`pkg-09` (`sharkdp/fd#2033`, category `clear-accept`, gold `accept`). My rubric graded it
`accept` too, on the final version of the artifact check. The package is an honest
cannot-reproduce: the student ran the issue's exact scenario 2 (an `--exec-batch` ordering
guarantee under an argument-size limit), showed the actual command and a 5-run marker-order log
that never contradicted the ordering guarantee, then explicitly stated they could not trigger the
report's failure and named what likely differed (uniform file-name lengths meant both commands
hit the same flush boundary, rather than one hitting the limit before the other). My rubric read
this as a pass because "Artifact shows the issue's own behavior, or an honest miss" explicitly
allows condition (b): a shown attempt at the issue's actual trigger plus a named, specific
difference from the issue's stated conditions. The "Honest claim, evidenced conclusion" check
also passed it: the conclusion ("I could not reproduce...") is no stronger than the shown
5-run log supports.

**Check rationale**

Quoting `rubric.md` exactly: "Artifact shows the issue's own behavior, or an honest miss ... Pass
condition: Either: (a) the artifact demonstrates the same behavior the issue describes, produced
by the same trigger the issue names (not a substituted input, a different code path, or a
different error/exit code); or (b) the report is an evidenced cannot-reproduce — it shows the
attempt's own artifact/transcript from running the issue's actual trigger, and names the specific
way its setup differs from the issue's stated conditions." I revised it to this two-branch form
after the first full run: my original wording only had branch (a), so it rejected `pkg-09` and
`pkg-10` — both gold `accept`, both "honest cannot-reproduce" clear-accepts — for not showing the
issue's positive behavior, which is exactly the outcome an honest cannot-reproduce is supposed to
be allowed to report. I kept the check required rather than dropping it to preferred, because a
package that shows no attempt at all, or substitutes an adjacent trigger while claiming a match,
should still fail; branch (b) only opens the door for a real, evidenced miss, not for absence of
evidence.

**Trade-offs**

Loosening the artifact check to accept branch (b) risked flipping the `wrong-target` and
`no-evidence` categories, since those categories also contain rejects whose reports narrate a
result without matching artifacts (e.g. `pkg-08`, which substitutes an unbound-variable
expression and narrates certainty over the wrong artifact). I re-ran the single-package
`disclosure` category's package (`pkg-20`) as a canary because it was the category most exposed
to any check I touched producing a floor failure on its own, and it still rejected on the
distinct disclosure check after the change, so the loosening was confirmed local to the intended
cases before I spent the full run. The trade-off I accepted: branch (b) is worded around a "named,
specific difference" from the issue's conditions, which is a real but softer bar than branch (a)'s
exact-match requirement — a report could in principle name a plausible-sounding but wrong
difference and still pass. I judged that risk acceptable because the check is paired with
"Honest claim, evidenced conclusion," which independently requires the stated conclusion to be no
stronger than the shown transcript, so a fabricated-sounding difference with no artifact under it
still fails on that second check.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
