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
