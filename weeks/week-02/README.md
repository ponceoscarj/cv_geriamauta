## Week 2 — Conditions, Boolean logic and functions

**Research question:** How do we consistently identify decreasing scores while keeping missing responses distinct from measured values?

**Learning objectives**
- Use `if`, `elif`, `else`, Boolean expressions, and `return`.
- Understand function inputs and outputs.
- Distinguish a valid low score from an unavailable assessment.
- Read a codebook before coding cleaning rules.

**Watch**
- [CS50P Lecture 1 — Conditionals](https://cs50.harvard.edu/python/weeks/1/).
- Revisit the functions portion of [CS50P Lecture 0](https://cs50.harvard.edu/python/shorts/functions/).

**Read**
- The **clock drawing / clock score** variable definition and missing-value conventions in the [NHATS documentation](https://nhats.org/). Document the exact round and source; don't guess which negative values signify inapplicable, refused or unavailable.
- [Python documentation — Control flow](https://docs.python.org/3/tutorial/controlflow.html).

**Math essentials**
- Inequalities: \(a<b\), \(a > b\).
- Boolean logic (`and`, `or`, `not`).
- Piecewise functions and unknown/missing values.

**NHATS task**
1. Make a short codebook note identifying the actual clock-score range and the relevant missing codes for the chosen NHATS round.
2. Implement `is_worsening(previous, current)`; return `None` if either value is missing, rather than falsely assuming no decline.
3. Create at least six synthetic examples including stable, improving, worsening and missing pairs.
4. Describe why a score difference is **not** equivalent to a dementia diagnosis.

**Starter interface**
```python
def is_worsening(previous, current):
    """Return True, False or None when a score is unavailable."""
    # Your implementation here
    raise NotImplementedError
```

**Submit**
- `src/week02_conditions.py`
- `notes/week02_clock_codebook.md`
- Six documented examples and expected outputs.

**Self-check:** Can you explain why `None`, `0` and `False` should not be treated as interchangeable?

---
