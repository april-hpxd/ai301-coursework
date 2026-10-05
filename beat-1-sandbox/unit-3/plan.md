# Unit 3 — Plan and Implement

Path: beat-1-sandbox/unit-3/plan.md

Record of your plan for fixing the issue you reproduced in Unit 2, and of the implementation that follows. This file is graded at the path above.

Complete every labelled field below. Each is graded on its own; content placed under the wrong label is not graded.

---

## Your identity upstream

**GitHub username**

april-hpxd

---

## Posted upstream

**Plan comment**

(https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64#issuecomment-5988276638)

---

## Plan details

### Cause

The test fixture 	est_query_with_partial_overlap contains the chunk text "Django is a Python web framework for rapid development", which includes all four query terms: Python, Django, web, and framework. The RelevanceScorer._tokenize method splits on whitespace, producing {python, django, web, framework} (4 tokens), and the overlap calculation is 4/4 = 1.0, which matches the scorer's actual behavior. The issue is not in the scorer's logic but in the fixture's assumption: it should produce a partial overlap, not full overlap.

### Scope

**In:** Modify tests/unit/test_relevance_scorer.py, line 51 (the chunk text in the test_query_with_partial_overlap fixture), by removing one of the four query terms from the chunk to create genuine partial overlap. Also remove the xfail marker to enable the test.

**Out:** The RelevanceScorer._tokenize method, the overlap calculation logic, any other test fixtures, or the scorer's integration with the rest of the codebase.

### Approach

1. Open tests/unit/test_relevance_scorer.py.
2. Locate the test_query_with_partial_overlap test method (lines 47-58).
3. Remove the @pytest.mark.xfail decorator (lines 43-46).
4. On line 51, change the chunk text from "Django is a Python web framework for rapid development" to "Django is a web framework for rapid development" (removing the term Python).
5. This leaves three of the four query terms (Django, web, framework) in the chunk, creating partial overlap.
6. The scorer will calculate overlap = 3 and relevance = 3/4 = 0.75, which satisfies the assertion 0.3 < score < 0.9.

### Test plan

Run the fixed test:

`
pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -v
`

Expected outcome: the test passes. The fixture's overlap score of 0.75 falls within the asserted range (0.3, 0.9).

Additional verification: run the full test suite to confirm no other tests are affected:

`
pytest tests/unit/test_relevance_scorer.py -v
`

All tests should pass (19 total), including the test_query_with_full_overlap test which verifies that a fixture with all query terms present correctly scores 1.0.

### Unknowns and risks

- The fixture modification is localized to one test and does not affect the scorer's logic, so no other code paths should be impacted.
- The term Python was chosen for removal arbitrarily; any of the four terms could be removed.

---

## Deviations from plan

None. The implementation matched the plan exactly. The fixture change was made in one line, the xfail marker was removed, and all tests pass.

---

## Test run results

### Initial test run (failing)

Before the fix, the test was marked xfail. Running the test without the marker shows the failure:

`
assert 1.0 < 0.9
AssertionError: The fixture had full overlap (1.0), not partial overlap.
`

### Final test run (passing)

After applying the fix:

Run the specific failing test:
`
pytest tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap -v
`

Result:
`
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap PASSED [100%]
1 passed in 0.21s
`

Full test suite:
`
pytest tests/unit/test_relevance_scorer.py -v
`

Result:
`
19 passed in 0.26s
`

All tests pass, confirming the fix is correct and does not break any other tests.

**Branch:** fix/64-partial-overlap-fixture
**Commit:** d7f1fb9 - "fix: remove 'Python' from partial_overlap fixture to create true partial overlap"
