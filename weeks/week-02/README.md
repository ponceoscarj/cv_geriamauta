# Week 2 — Conditions, Boolean logic and functions

## Learning objectives

By the end of Week 2, you should be able to:

1. Use comparisons (`<`, `<=`, `>`, `>=`, `==`, `!=`) and `if`, `elif`, `else`.
2. Combine logical conditions with `and`, `or`, and `not`.
3. Explain function parameters, arguments, inputs, outputs, and `return` versus `print`.
4. Distinguish `None`, `0`, `False`, and raw NHATS missing-value codes.
5. Use a specified NHATS codebook to document scoring and missingness rules before writing data-cleaning code.
6. Run a Python script in a terminal, commit the work, and push it to the GitHub repository created in Week 1.

## Watch — required videos

1. **[Harvard CS50P — Week 1: Conditionals](https://cs50.harvard.edu/python/weeks/1/)**. Cover comparison operators, `if`/`elif`/`else`, `and`/`or`, and Boolean expressions. The lecture also introduces `match`, but you do not need to use it this week.
2. **[Harvard CS50P — Functions (short video)](https://cs50.harvard.edu/python/shorts/functions/)**. Review functions, parameters and arguments.
3. **[Harvard CS50P — Return Values (short video)](https://cs50.harvard.edu/python/shorts/return_values/)**. Concentrate on the difference between returning a value and printing it.
4. **[Harvard CS50P — Boolean Expressions (short video)](https://cs50.harvard.edu/python/shorts/boolean_expressions/)**. Reinforce `True`, `False`, conditions and logical reasoning.

## Consult — required research documentation

- **[NHATS official website and documentation](https://nhats.org/)**. Choose **one named NHATS round** and locate the official codebook/user guide relevant to its clock drawing or clock-score variables. Document the variable name, the exact round, the valid score range, the population to whom the assessment applies, and the meanings of relevant special/missing codes.

*This is source checking, not an additional Python reading assignment: the precise NHATS definitions must come from the corresponding codebook, not from a general programming video. Do not assume that a negative number always represents the same missingness reason or that variables have unchanged definitions across rounds.*

## Mathematics essentials

- Comparisons and inequalities: \(a < b\), \(a \geq b\), and equality.
- Boolean logic: AND, OR, NOT; when a condition evaluates to true or false.
- A piecewise decision rule, with an **unknown** outcome if a measurement is unavailable:

```text
is_worsening(previous, current):
    None   if either score is unavailable
    True   if current < previous
    False  if current >= previous
```

Here, `previous` is a previous valid clock score and `current` is the current valid clock score. For this exercise, assume a lower clock score represents worse performance **after confirming the scoring direction in your chosen codebook**.

## NHATS programming task (synthetic data only)

1. Write `notes/week02_clock_codebook.md` documenting the actual clock-score range, scoring direction, respondent/assessment eligibility, raw missing codes and codebook source for one NHATS round. **Do not guess codes.**
2. In `src/week02_conditions.py`, implement `is_worsening(previous, current)` so it returns `True` for decline, `False` for stable or improving scores, and `None` when either input is unavailable.
3. Use **valid synthetic scores or Python `None`** when calling the function. Do not pass NHATS raw missing-code numbers directly into the function; their conversion into missing values will be developed in Week 4.
4. Make at least **six documented synthetic calls** covering decline, improvement, no change, zero as a valid score, missing previous score, and missing current score. For each, write the expected output **before** running the script.
5. Write a short reflection: why does a clock-score decline alone not establish a dementia diagnosis?
6. Run the program **from the terminal** inside your Week 1 Conda environment, not solely through the VS Code Run button.

### Starter interface

```python
def is_worsening(previous, current):
    """Return True, False, or None if a score is unavailable."""
    # Implement the logic yourself.
    raise NotImplementedError
```

Your solution should use `is None` when checking for missing Python values. Explain why using `if not previous` would be wrong when a valid score can be zero.

### Run from the terminal

Open **Miniforge Prompt on Windows** or **Terminal on macOS**, navigate to your existing project repository, and run:

```bash
conda activate nhats-course
cd path/to/nhats-deep-learning
python src/week02_conditions.py
```

Replace `path/to/nhats-deep-learning` with your actual path. Make sure that the output agrees with all six expected results.

## Submit via your existing private GitHub repository

Commit and push the work to the **same `nhats-deep-learning` repository you created in Week 1**. Do not make a separate repository for Week 2.

Required files:

- `src/week02_conditions.py`
- `notes/week02_clock_codebook.md`
- Six synthetic examples with expected and observed outputs (in the script or a clearly identified section of the notes)
- A short note explaining why a score change alone is not a dementia diagnosis

From your repository directory:

```bash
git status
git add src/week02_conditions.py notes/week02_clock_codebook.md
git commit -m "Complete Week 2 conditions and functions"
git push
```

Open GitHub in your browser, verify that both files and the latest commit appear, and share the **private repository URL with the designated assessor**, granting access as instructed by your course organiser.

## Self-check

- Can I explain what `if`, `elif`, and `else` do?
- Can I distinguish parameters from arguments, and `return` from `print`?
- Can I explain why `None`, `0`, and `False` are not interchangeable?
- Can I predict the result of `and`, `or`, and `not` expressions?
- Can I identify the exact NHATS round and codebook supporting my score and missingness rules?
- Can I run my script from the terminal and push the final work to GitHub without copying every command from a tutorial?

**Scope boundary:** Loops are for Week 3; exceptions and automated unit tests are for Week 4. Do not add those topics now.
