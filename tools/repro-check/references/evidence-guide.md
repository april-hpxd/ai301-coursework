# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: the repro report's opening environment line(s) — in an
eval bundle, the sentence(s) right after "Environment:"; live, the same
line in the student's draft report. Cross-check any version-specific
trigger against the package's "Repo facts" (or, live, the repo's release
page) and against anything a maintainer or thread comment named as
version-relevant (a dependency, a build flag, a platform).

What good looks like: OS/distro plus architecture where relevant, the
tool's own version (matching or explicitly reconciled against the
issue's target version, e.g. "issue filed on 1.9.4/1.10.0; still present
on 1.11.7"), and any dependency/library version the thread already
pointed to as version-relevant. A report that omits a version dimension
the issue's own thread flagged as load-bearing (e.g. a build profile
that changes the failure mode) is insufficient even if OS and tool
version are present.

## Steps

Where it lives: the repro report's numbered/step-by-step section:
starting state (files, config, fixtures created), the exact
commands/inputs run, and, when present, a control run for contrast.

What makes steps followable: a stranger with only the report text could
recreate the starting state and rerun the exact commands without
consulting anything not shown (no "our internal config," no "the repo I
already had set up," no step that silently assumes prior state). A
report that reproduces "in a private monorepo" or skips a needed
driver/flag on a platform-specific issue is not followable, regardless
of how convincing its output looks.

## Behavior shown

Where it lives: the artifact block(s) in the repro report (console
output, log excerpt, diff/CSS output, screenshot description), read
side by side with the issue body's "actual behavior" and its exact
trigger (the specific input, selector shape, argument pattern, or
condition named in the issue, not a paraphrase of it).

What it means to show the issue's own behavior rather than an adjacent
one: the trigger used in the steps must be the same trigger the issue
names (same syntax form, same code path, same argument shape), and the
artifact's failure mode must match the issue's (same error class, same
exit behavior, same visual symptom) rather than a different failure
that happens to also be wrong. A graceful validation error standing in
for a reported crash, or a substituted expression that exercises a
different code path, is an adjacent-symptom fail even when everything
else about the report is well-written.

## Honesty

Where it lives: the report's own concluding sentence(s) ("Actual:",
"Expected:", or a closing diagnosis), read against whether the report
actually shows a transcript, log, or artifact backing that sentence.

How to tell honest from over-claimed: an honest report's confidence
never exceeds its shown evidence. "I could not reproduce X; here is
what I tried and what differed" is a full pass when the attempt and the
named difference are both shown. A report that states a root cause
("it's obviously the debounce race"), a certainty word ("guaranteed,"
"verified," "definitely"), or a fix timeline, without a transcript or
artifact under it, is over-claimed regardless of how plausible the
diagnosis sounds.

## Comms

Where it lives: the candidate claim comment's own wording, read against
(a) the issue it names and (b) the repo-facts contribution-policy line
(eval) or CONTRIBUTING.md/AI-policy file (live).

What specific-and-honest looks like next to boilerplate: a claim names
the concrete behavior being investigated and a concrete next step in
the commenter's own words. Generic enthusiasm ("+1, someone should fix
this"), flattery openers, unconditional fix promises, guaranteed
delivery dates, or "assign me" with no investigation content are
boilerplate and fail the claim-specificity check regardless of how
polite they read.

Disclosure is a separate, conditional read of the same comments: find
the repo's stated AI-usage policy first. Silence or an explicitly
permissive policy never requires anything of the comment. An explicit
disclosure requirement (naming the tool and the extent of assistance,
or similar) must be visibly met in the comment text — a policy that
only asks disclosure in the PR, not the issue comment, does not gate
the issue-comment check.
