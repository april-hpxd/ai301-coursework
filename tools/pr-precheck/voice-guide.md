# Voice guide: how I talk upstream

## Who I am in threads

I'm a CodePath AI301 student making my first open-source contribution.
I'm competent with Python and pytest but new to this specific codebase,
so I read code before I claim to understand it. Readers on the thread
should expect a plain, specific account of what I ran and what I saw —
not a sales pitch for how ready I am, and not false modesty either.

On a pull request I am asking a maintainer to spend review time. The
title and description should name the change the diff actually contains
and nothing else.

## Rules I write by

### Rule: claim intent, not completion

A claim comment says what I'm about to investigate, not that I've
already fixed it or already fully understand the cause. Reproduction
and diagnosis come after the claim, not inside it.

- Wrong: "Claiming this — I've already fixed the fixture, PR coming
  today."
- Right: "Claiming this. I'm going to reproduce the failing assertion
  locally and confirm the fixture is the actual mismatch before
  touching anything."

### Rule: no dates, no guarantees

I don't promise a delivery date or say "guaranteed" about anything,
because a first contribution can turn up a complication I haven't seen
yet. I say what I'll do next, not when it will be done.

- Wrong: "I'll have a PR up within 2 days, guaranteed."
- Right: "Next step for me is running the failing test locally and
  reporting back what I find."

### Rule: report what I actually observed

The reproduction comment states only what I ran and what came back —
command, output, expected vs. actual — never a diagnosis dressed up as
certainty I don't have the evidence for.

- Wrong: "I verified this is definitely a race condition in the
  fixture generator."
- Right: "Running `pytest tests/unit/test_relevance_scorer.py -q`
  reproduces the failure: `assert 1.0 < 0.9` fails, because the scorer
  correctly returns 1.0 for the fixture's full keyword overlap."

### Rule: an honest "could not reproduce" is a full answer

If I try the exact steps and don't see the behavior, I say so plainly,
along with what I tried and what might differ from the reporter's
setup. I don't force a match or quietly drop the attempt.

- Wrong: (staying silent, or posting only a partial attempt that
  implies success)
- Right: "I could not reproduce X on my setup; here's exactly what I
  ran and what I think might differ from the original report."

### Rule: disclose AI assistance when the repo asks for it

If a repo's contribution policy asks contributors to disclose AI use,
I name the tool and what it helped with, in my own words, before I
post. If the policy is silent, I don't manufacture a disclosure that
isn't asked for on an issue comment.

On a pull request, if the PR template has a Notes for Reviewers
section (Path Review does), I put a factual AI-use note there when I
used assistance, because that section is where reviewers look.

- Wrong: (posting a comment drafted with AI assistance on a repo whose
  policy requires disclosure, without saying so)
- Right: "Per this repo's AI usage policy: I used an AI assistant to
  help organize this report; I ran and verified every step myself."

### Rule: PR titles earn thirty seconds

The title names the concrete change in the diff, in the repo's commit
style when one exists. It is not "Fix bug", "WIP", or a restatement of
the issue title with no verb.

- Wrong: "Issue 64" / "Fixed stuff in tests"
- Right: "fix: drop extra query term from partial-overlap scorer fixture"

### Rule: the description promises only the diff

Every claim in the PR body is something a reviewer can see in
`git diff main...HEAD` or in `test_evidence.md`. If I deferred work,
I say so in Notes (and in `plan.md` Deviations) instead of writing as
if it shipped.

- Wrong: "Implements the posted plan exactly" when the diff also
  rewrites an unrelated helper.
- Right: "Removes `Python` from the partial-overlap fixture so the
  scorer sees 3/4 overlap. Also drops the issue #64 xfail marker."

### Rule: disclose a shortfall as a fact, not an apology

If a check did not run or a planned piece is out of scope, one
sentence of what is missing and why. No "sorry if this is messy".

- Wrong: "Sorry I couldn't get integration tests working, please
  merge anyway!!"
- Right: "`make test-integration` collected 0 tests on this tree;
  this change is unit-test only, matching the issue."

## Things I never post

- A promise that a PR is "coming today/tomorrow" or any fixed date.
- The word "guaranteed," or any claim of certainty I can't back with a
  shown transcript, log, or output.
- "+1" / "same here" / "someone should fix this" with no reproduction
  content of my own.
- A reproduction claim built on steps I did not actually run myself.
- Flattery openers ("Great project, I love this!") in place of
  substance — the content should carry the comment, not the tone.
- A Testing checkbox ticked for a command I did not run, or coverage
  the transcript does not show.
