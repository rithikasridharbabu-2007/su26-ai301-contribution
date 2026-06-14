# su26-ai301-contribution
# Open Source Contribution Log

## Phase I

**Issue:** https://github.com/catboost/catboost/issues/872

**Problem Summary:**
When a user passes an incorrect parameter type (e.g. a float instead of an 
int) to CatBoost's training function, the error message only says 
"Can't parse parameter" without indicating what type was expected. This makes 
debugging difficult, especially for users running hyperparameter optimization 
pipelines. The fix involves improving the error message in json_helper.h to 
clearly state the expected type.

**Why I Chose This Issue:**
I chose this issue because it is well-scoped and clearly defined — the 
problem and the fix are both easy to understand. It touches C++ which I know, 
and the maintainer has explicitly invited contributions. Improving error 
messages is a meaningful quality-of-life fix that directly helps users debug 
their code faster.

**Status:** Phase I Complete


## Phase II

### Reproduction Process

**Environment Setup:** Reproduced at runtime using the pip-installed package
(`pip3 install catboost`, v1.2.10) on macOS with Python 3.14 — no source
build needed to trigger the bug, since it's raised during parameter parsing
at fit time. (Note: the fix itself is in a C++ header, so Phase III will
require building CatBoost from source.) Only setup hiccup was a dropped
`git clone` on the full repo; resolved with a shallow clone (`--depth 1`).

**Branch Link:** https://github.com/rithikasridharbabu-2007/catboost/tree/fix-issue-872

**Steps to Reproduce:**
1. `pip3 install catboost`
2. Start Python and run:
```python
   from catboost import CatBoostClassifier
   model = CatBoostClassifier(thread_count="four")
   model.fit([[1, 2], [3, 4]], [0, 1])
```
3. **Expected:** An error naming the parameter, the bad value, AND the
   expected type (e.g. "expected int").
4. **Actual:** The error names the parameter and value but not the expected
   type:
   `_catboost.CatBoostError: catboost/private/libs/options/json_helper.h:41: Can't parse parameter "thread_count" with value: "four"`

### Solution Approach

**Implementation Plan (UMPIRE)**

- **Understand:** When a parameter value can't be parsed, the error from
  `json_helper.h` reports the parameter name and value but not the expected
  type, making debugging harder — especially in hyperparameter tuning.
- **Match:** The parsing helper in `json_helper.h` is templated on the
  target type `T`, so the expected type is already available at the throw
  site and can be added to the message.
- **Plan:** Locate the throw at `json_helper.h:41` where parsing fails, and
  extend the error string to include the expected type. Add/adjust a unit
  test covering the type-mismatch case.
- **Implement:** (Phase III) Branch link above.
- **Review:** Self-review against the project's CONTRIBUTING.md and
  commit/PR conventions before opening the PR.
- **Evaluate:** Re-run the reproduction above — the message should now state
  the expected type. Confirm existing parameter-parsing tests still pass.
