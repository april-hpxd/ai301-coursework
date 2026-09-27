# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | The repro report's environment line(s): OS/distro, the tool's own version, and any dependency or config version the issue's behavior depends on (per references/evidence-guide.md's Environment section). | The report states the OS, the tool version, and any dependency version the issue's trigger depends on (e.g. a library version named in the thread, or a build/profile flag the issue turns on). Missing one of these when the issue's behavior is known to hinge on it is a fail, not unclear. | required |
| Steps are followable by a stranger | The repro report's steps: starting state, exact commands/config/inputs, and how the trigger condition named in the issue is produced. | A reader with no other context could reproduce the starting state and run the exact steps without guessing a missing command, config file, or private resource. Steps that depend on the reporter's own private repo, unshared config, or an unstated setup step fail this check. | required |
| Artifact shows the issue's own behavior, or an honest miss | The candidate repro report's shown output/log/screenshot, read against the issue body's stated actual/expected behavior and its exact trigger condition. | Either: (a) the artifact demonstrates the same behavior the issue describes, produced by the same trigger the issue names (not a substituted input, a different code path, or a different error/exit code); or (b) the report is an evidenced cannot-reproduce — it shows the attempt's own artifact/transcript from running the issue's actual trigger, and names the specific way its setup differs from the issue's stated conditions. A control run that contrasts the triggering and non-triggering case is preferred evidence but not required by itself. Fails only when the shown artifact is passed off as matching the issue's behavior while actually showing an adjacent symptom (different error class, different exit code, a substituted trigger), or when a cannot-reproduce is asserted with no attempt shown at all. | required |
| Honest claim, evidenced conclusion | The report's stated conclusion (reproduced / could not reproduce / root cause) compared against the artifacts it actually shows. | The report's conclusion is no stronger than what its own artifacts support. An evidenced "could not reproduce," including a named hypothesis for why, passes. A confident diagnosis, a "guaranteed" claim, or a "verified" root cause with no shown transcript or artifact fails. | required |
| AI-disclosure compliance | The repo-facts contribution-policy line (eval) or CONTRIBUTING.md / AI-policy file (live), read against the claim and repro comment text. | If the stated policy requires disclosing AI assistance, the comment discloses it (naming that AI was used, in whatever form the policy asks). If the policy is silent or explicitly permissive, this check passes regardless of disclosure. Only an explicit disclosure requirement, left unmet, fails this check. | required |
| Claim states specific, bounded intent | The candidate claim comment's own text. | The claim names the specific behavior/issue being investigated and a concrete next step, in the commenter's own words. It fails if it is interchangeable boilerplate (generic enthusiasm, a flattery opener, an unconditional promise of a fix or a fixed delivery date, or an "assign me" demand with no stated investigation). | required |

## Verdict rule

Accept (ready to post) only if every required check passes. Any required `fail` or `unclear` produces `reject`. In claim-only mode, "Environment recorded", "Steps are followable by a stranger", and "Artifact shows the issue's own behavior" are reported `unclear` / `not yet applicable: claim-only draft` and are excluded from the verdict rule per SKILL.md; the verdict then rests on "Honest claim, evidenced conclusion" (applied to the claim's own assertions only), "AI-disclosure compliance", and "Claim states specific, bounded intent".
