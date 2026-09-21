# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintained and usable repository | In eval mode, read `archived:`, the five default-branch commit dates and authors, and the capture date in **Repo facts**. In live mode, use the repository archive banner and the last five default-branch commits described in the evidence guide. | The repository is not archived, and at least one of the last five default-branch commits is by a non-bot account dated within 180 days of the capture date (eval) or today (live). | required |
| AI-assisted contribution permitted | The **contribution policy** line in the eval bundle, or `CONTRIBUTING.md`, linked contributor documents, AI-policy files, and PR templates in the live repository. | The policy is silent about AI or permits AI-assisted contributions subject to conditions. Fail only when it explicitly says AI-generated or AI-assisted code/documentation/contributions are not accepted. | required |
| Bounded, actionable contribution | The issue body and comment thread in the bundle; in live mode, the issue body and thread. Identify the requested change and any files, behavior, reproduction, acceptance criteria, or maintainer diagnosis stated there. | The issue asks for one concrete change or bug fix and supplies at least one observable target (a named behavior, reproduction, affected file/component, acceptance criterion, or maintainer diagnosis). It must not be explicitly an umbrella/tracking issue, a support question, a still-unsettled design discussion, or a request that a maintainer says requires core-internal changes. A maintainer-authored issue carrying a `good first issue`/equivalent label also passes this check when its short description names the concrete behavior to change. | required |
| Available to start | In eval mode, read `this issue: assignees`, `linked PRs`, and the **Comments** section. In live mode, inspect the issue sidebar Development/Assignees boxes and its comment thread, following the Path Review house rule in `scope.md`. | There is no assignee, no open linked pull request, and no non-stale claim in the thread. A claim is non-stale when a contributor says they are working on or will submit a PR within the preceding 90 days; an older claim does not block the issue if a maintainer subsequently invites new takers. In Path Review live mode, ignore classmates' claim comments as the house rule directs. | required |
| Maintainer response signal | In eval mode, use the **maintainer first-response sample** and the issue thread. In live mode, inspect five recently updated issues and this issue for Owner, Member, or Collaborator responses as described in the evidence guide. | At least one sampled issue received a maintainer response within 30 days, or this issue has a maintainer response. An issue opened by a maintainer counts as a maintainer response signal. | preferred |
| Explicit newcomer signal | The issue labels and author association in the issue metadata, plus the issue body/thread. | The issue has a `good first issue`/equivalent newcomer label, or it was opened by an Owner, Member, or Collaborator and gives a concrete target for the change. | preferred |

## Verdict rule

Accept only if every required check passes. Any required `fail` or `unclear` produces `reject`. Preferred checks never change the verdict; report them and use them only to rank accepted live candidates.
