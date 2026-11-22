# Week 4 — Exceptions, Testing and Debugging

## Learning objectives

By the end of Week 4, you should be able to:

1. Explain the difference between:
   - a syntax error;
   - an exception;
   - code that runs but produces an incorrect result.
2. Recognize common Python exceptions such as `ValueError`, `TypeError`, and `NameError`.
3. Use `try` and `except` for simple expected errors.
4. Use `raise` when a function receives invalid input.
5. Use `assert` to check whether a function behaves as expected.
6. Write several automated tests for the same function.
7. Test normal cases, boundary cases, missing values, and invalid values.
8. Read an error message and identify where a problem occurred.
9. Debug a small program systematically.
10. Reuse:
    - variables;
    - conditions;
    - Boolean logic;
    - functions;
    - lists;
    - indexing;
    - loops;
    - nested loops;
    - `None`;
    - arithmetic means;
    - successive differences.
11. Build a small tested cleaning and analysis pipeline for synthetic longitudinal data.

---

# Do you need NHATS data this week?

**No.**

You do not need to download or import real NHATS participant-level data in Week 4.

Everything needed for the programming assignment is included in this lesson.

This week uses:

- synthetic clock scores;
- synthetic missing-value codes;
- Python functions;
- `assert`;
- exceptions;
- debugging.

Real NHATS files will be introduced in **Week 5**.

The synthetic codes used this week are educational examples only. They are **not intended to represent the actual coding scheme of any NHATS round**.

---

# 1. Main idea for the week

Until now, most of our questions have been:

> Can I make the program run?

This week we ask a more important question:

> **How do I know that the program is correct?**

A program can:

```text
run without crashing
```

and still:

```text
produce the wrong result
```

This is particularly important in research.

A mistake in a data-cleaning function may affect hundreds or thousands of observations without producing an obvious error.

This week introduces the workflow:

```text
write a function
↓
decide what it should do
↓
test it
↓
observe whether it passes
↓
debug it if necessary
↓
test it again
```

---

# 2. Required resources

## Exceptions

Watch:

- [Harvard CS50P — Week 3: Exceptions](https://cs50.harvard.edu/python/weeks/3/)

Focus on:

- syntax errors;
- runtime errors;
- `ValueError`;
- `NameError`;
- `try`;
- `except`;
- `else`;
- `raise`;
- debugging.

You do not need advanced exception handling.

---

## Unit testing

Watch:

- [Harvard CS50P — Week 5: Unit Tests](https://cs50.harvard.edu/python/weeks/5/)

Focus primarily on:

- `assert`;
- expected versus observed results;
- why automated testing is useful.

The lecture also discusses `pytest`.

We will first learn the testing principle using ordinary Python.

---

# 3. Three different types of problems

Consider three situations.

---

## Problem 1 — Syntax error

```python
if score < 3
    print("Low score")
```

Python cannot understand this program because the colon is missing.

This is a:

```text
SyntaxError
```

---

## Problem 2 — Runtime exception

```python
score = None

change = score - 1
```

The syntax is valid.

But Python cannot subtract an integer from `None`.

The program therefore fails while running.

---

## Problem 3 — The program runs but is wrong

Consider:

```python
scores = [5, 4, None, 3]

total = 0

for score in scores:
    if score is None:
        score = 0

    total = total + score
```

This program may run successfully.

But it incorrectly turns a missing observation into:

```text
0
```

If zero is a valid score, this changes the meaning of the data.

This is often the most dangerous type of problem because Python does not necessarily warn you.

---

# 4. Exceptions

An exception occurs when Python encounters a problem while running otherwise valid code.

For example:

```python
age = int("seventy")
```

Python cannot convert:

```text
seventy
```

into an integer.

It raises:

```text
ValueError
```

---

# 5. `try` and `except`

Consider:

```python
age = int(input("Age: "))
```

If the user enters:

```text
70
```

everything works.

If the user enters:

```text
seventy
```

Python raises a `ValueError`.

We can handle that specific situation:

```python
try:
    age = int(input("Age: "))
except ValueError:
    print("Age must be entered as a number.")
```

Conceptually:

```text
TRY:
    perform an operation

IF a particular error occurs:
    handle that error
```

---

# 6. Do not hide every error

Avoid using:

```python
try:
    ...
except:
    pass
```

as a normal programming strategy.

This means:

> If anything goes wrong, silently ignore it.

That can hide serious mistakes.

Prefer handling specific expected errors:

```python
except ValueError:
```

rather than catching everything.

---

# 7. Raising an exception deliberately

Sometimes an error is exactly what we want.

Suppose a function accepts only scores between `0` and `5`.

```python
def validate_score(score):
    if score < 0 or score > 5:
        raise ValueError("Score must be between 0 and 5.")

    return score
```

Then:

```python
validate_score(4)
```

returns:

```text
4
```

but:

```python
validate_score(9)
```

raises:

```text
ValueError
```

The function is not broken.

It is deliberately refusing invalid input.

---

# 8. Expected missingness versus invalid input

For this week's synthetic exercises, assume:

```text
0, 1, 2, 3, 4, 5
```

are valid clock scores.

We will also use two **synthetic raw missing-value codes**:

```text
-1 = not assessed
-2 = missing / unavailable
```

Again:

> These are synthetic teaching codes, not actual NHATS codes.

Our cleaning function should therefore behave like:

```text
0 to 5
→ keep the value

-1 or -2
→ convert to None

anything else
→ raise ValueError
```

This distinction is important:

```text
expected missing value
→ None

unexpected impossible value
→ error
```

---

# 9. Synthetic raw dataset

Use the following synthetic dataset this week:

```python
raw_scores = [
    [5, 4, -1, 3, 2],
    [3, 3, 2, 2, 1],
    [5, -2, 4, 4, 4],
    [0, 1, 1, -1, 2],
    [4, 4, 3, 3, -2],
]
```

Interpret it as:

```text
             Round
             1    2    3    4    5

Patient 1    5    4   -1    3    2
Patient 2    3    3    2    2    1
Patient 3    5   -2    4    4    4
Patient 4    0    1    1   -1    2
Patient 5    4    4    3    3   -2
```

The codes:

```text
-1
-2
```

should eventually become:

```python
None
```

The score:

```text
0
```

must remain:

```text
0
```

because it is a valid score.

---

# 10. First cleaning function

Create:

```text
src/week04_testing.py
```

Write:

```python
def clean_clock_score(value):
    if value == -1 or value == -2:
        return None

    if value < 0 or value > 5:
        raise ValueError("Unexpected clock score.")

    return value
```

Try:

```python
print(clean_clock_score(5))
print(clean_clock_score(0))
print(clean_clock_score(-1))
print(clean_clock_score(-2))
```

Predict each result before running the program.

---

# 11. What should happen with an impossible value?

Try:

```python
clean_clock_score(9)
```

The function should raise:

```text
ValueError
```

This is intentional.

The function protects the rest of the analysis from unexpected values.

---

# 12. Manual testing

You could test the function with:

```python
print(clean_clock_score(5))
```

Then look at the output and decide whether it is correct.

But imagine having:

```text
5 functions
×
many possible inputs
```

Checking every result manually becomes difficult.

Instead, we can tell Python what we expect.

---

# 13. `assert`

Write:

```python
assert clean_clock_score(5) == 5
```

This means:

> I expect `clean_clock_score(5)` to equal `5`.

If the statement is true, Python continues.

If it is false, Python raises:

```text
AssertionError
```

---

# 14. Several tests

Try:

```python
assert clean_clock_score(5) == 5
assert clean_clock_score(0) == 0
assert clean_clock_score(-1) is None
assert clean_clock_score(-2) is None
```

If all tests pass, Python produces no error.

You can then add:

```python
print("All cleaning tests passed.")
```

---

# 15. Deliberately break a test

Change:

```python
assert clean_clock_score(5) == 5
```

to:

```python
assert clean_clock_score(5) == 4
```

Run the program.

You should see:

```text
AssertionError
```

The test correctly detected that your expectation was wrong.

Restore the correct test afterward.

---

# 16. Tests should cover different types of cases

Good tests should not only check easy examples.

Think about four categories:

```text
normal case
boundary case
missing case
invalid case
```

For `clean_clock_score()`:

### Normal

```python
assert clean_clock_score(3) == 3
```

### Boundary

```python
assert clean_clock_score(0) == 0
assert clean_clock_score(5) == 5
```

### Missing

```python
assert clean_clock_score(-1) is None
assert clean_clock_score(-2) is None
```

### Invalid

An impossible score should raise an exception.

We will test that manually for now.

---

# 17. Testing expected exceptions without pytest

You can write:

```python
try:
    clean_clock_score(9)
    assert False
except ValueError:
    pass
```

Read this carefully.

We expect:

```python
clean_clock_score(9)
```

to raise a `ValueError`.

If it does **not**, Python reaches:

```python
assert False
```

and the test fails.

If a `ValueError` occurs, the `except` block handles it.

This is enough for our introductory testing exercises.

---

# 18. Reuse Week 2 — Conditions and functions

Now create:

```python
def is_deteriorating(previous, current):
    if previous is None or current is None:
        return None

    return current < previous
```

Assume that a lower score represents worse performance for this synthetic exercise.

Test:

```python
assert is_deteriorating(5, 4) is True
assert is_deteriorating(4, 5) is False
assert is_deteriorating(4, 4) is False
assert is_deteriorating(None, 4) is None
assert is_deteriorating(4, None) is None
```

Notice that this directly reuses:

- functions;
- `if`;
- Boolean logic;
- `None`.

---

# 19. Reuse Week 3 — Successive change

Write:

```python
def score_change(previous, current):
    if previous is None or current is None:
        return None

    return current - previous
```

The mathematical rule is:

$$
\Delta s_t = s_{t+1} - s_t
$$

For:

```text
previous = 5
current  = 4
```

the change is:

```text
4 - 5 = -1
```

Test:

```python
assert score_change(5, 4) == -1
assert score_change(3, 4) == 1
assert score_change(4, 4) == 0
assert score_change(None, 4) is None
assert score_change(4, None) is None
assert score_change(None, None) is None
```

---

# 20. Reuse Week 3 — Counting valid observations

Write:

```python
def count_valid(scores):
    count = 0

    for score in scores:
        if score is not None:
            count = count + 1

    return count
```

Test:

```python
assert count_valid([5, 4, None, 3]) == 3
assert count_valid([None, None]) == 0
assert count_valid([0, 1, 2]) == 3
assert count_valid([]) == 0
```

Notice that:

```python
0
```

counts as a valid observation.

---

# 21. Reuse Week 3 — Arithmetic mean

Write:

```python
def mean_valid(scores):
    total = 0
    count = 0

    for score in scores:
        if score is not None:
            total = total + score
            count = count + 1

    if count == 0:
        return None

    return total / count
```

Test:

```python
assert mean_valid([5, 4, 3]) == 4
assert mean_valid([5, 4, None, 3]) == 4
assert mean_valid([0, 2, 4]) == 2
assert mean_valid([None, None]) is None
assert mean_valid([]) is None
```

The arithmetic mean is:

$$
\bar{x} = \frac{1}{n}\sum_{i=1}^{n}x_i
$$

For:

```text
5, 4, None, 3
```

the valid observations are:

```text
5, 4, 3
```

so:

$$
\frac{5 + 4 + 3}{3} = 4
$$

---

# 22. Debugging

A useful debugging workflow is:

```text
1. Read the error message.
2. Identify the line that failed.
3. Look at the input.
4. State what you expected.
5. State what actually happened.
6. Reduce the problem to the smallest example possible.
7. Change one thing.
8. Run the program again.
```

Avoid randomly editing code until the error disappears.

---

# 23. Debugging exercise

Here is a broken function:

```python
def mean_valid(scores):
    total = 0

    for score in scores:
        total = total + score

    return total / len(scores)
```

Try:

```python
mean_valid([5, 4, None, 3])
```

Before fixing it, answer:

1. What exception occurs?
2. Which value causes it?
3. Why?
4. Should `None` contribute to the total?
5. Should `None` contribute to the denominator?
6. What should the correct result be?

Then repair the function.

---

# 24. Organizing your tests

Create a separate file:

```text
tests/week04_assert_tests.py
```

Import your functions:

```python
from src.week04_testing import (
    clean_clock_score,
    score_change,
    mean_valid,
    is_deteriorating,
    count_valid,
)
```

Then place your assertions underneath.

For example:

```python
assert clean_clock_score(0) == 0
assert clean_clock_score(5) == 5
assert clean_clock_score(-1) is None

assert score_change(5, 4) == -1
assert score_change(3, 4) == 1

assert mean_valid([5, 4, None, 3]) == 4

assert count_valid([5, None, 3]) == 2

assert is_deteriorating(5, 4) is True

print("All Week 4 tests passed.")
```

Run the tests with:

```bash
python tests/week04_assert_tests.py
```

If everything is correct, the final output should include:

```text
All Week 4 tests passed.
```

---

# 25. Optional preview — pytest

You do **not** need pytest to complete Week 4.

However, you should know that larger Python projects commonly use a test runner such as:

```text
pytest
```

Instead of manually running a file containing many assertions, pytest automatically discovers and runs tests.

For now, understanding:

```python
assert expected == observed
```

is more important than learning a testing framework.

We can adopt pytest later when the project becomes larger.

---

# Week 4 assignment

The homework is intentionally progressive.

Each part builds on skills from earlier weeks.

Complete the sections in order.

---

# Assignment Part A — Clean individual raw values

Create:

```text
src/week04_testing.py
```

Implement:

```python
def clean_clock_score(value):
    ...
```

For this synthetic exercise:

```text
0–5  → valid scores
-1   → missing
-2   → missing
```

Any other numerical value should raise:

```text
ValueError
```

Your function should therefore behave like:

```text
5    → 5
0    → 0
-1   → None
-2   → None
9    → ValueError
```

### Tests

Write assertions for:

```text
minimum valid score
maximum valid score
ordinary valid score
first missing code
second missing code
invalid value below range
invalid value above range
```

---

# Assignment Part B — Clean one patient trajectory

Use:

```python
raw_patient = [5, 4, -1, 3, 2]
```

Create a new list called:

```python
clean_patient
```

Use a loop and your:

```python
clean_clock_score()
```

function to transform:

```python
[5, 4, -1, 3, 2]
```

into:

```python
[5, 4, None, 3, 2]
```

Do not manually type the cleaned list.

Generate it with a loop.

Test:

```python
assert clean_patient == [5, 4, None, 3, 2]
```

This reuses:

- lists;
- loops;
- functions;
- missing values.

---

# Assignment Part C — Analyze one cleaned patient

Using:

```python
clean_patient
```

calculate:

1. number of valid observations;
2. mean of valid observations;
3. every available consecutive score change;
4. whether each available consecutive pair represents deterioration.

Use the functions:

```python
count_valid()
mean_valid()
score_change()
is_deteriorating()
```

Do not duplicate the same calculation logic outside the functions.

---

# Assignment Part D — Clean a longitudinal cohort

Use:

```python
raw_scores = [
    [5, 4, -1, 3, 2],
    [3, 3, 2, 2, 1],
    [5, -2, 4, 4, 4],
    [0, 1, 1, -1, 2],
    [4, 4, 3, 3, -2],
]
```

Create:

```python
clean_scores
```

using nested loops.

The result should be:

```python
[
    [5, 4, None, 3, 2],
    [3, 3, 2, 2, 1],
    [5, None, 4, 4, 4],
    [0, 1, 1, None, 2],
    [4, 4, 3, 3, None],
]
```

Do not manually construct the final cleaned dataset.

Use:

```text
outer loop
→ participant

inner loop
→ score

clean_clock_score()
→ cleaned value
```

Then test:

```python
assert clean_scores == [
    [5, 4, None, 3, 2],
    [3, 3, 2, 2, 1],
    [5, None, 4, 4, 4],
    [0, 1, 1, None, 2],
    [4, 4, 3, 3, None],
]
```

---

# Assignment Part E — Analyze every participant

For every participant in:

```python
clean_scores
```

print:

```text
Participant number
Scores
Number of valid observations
Mean score
```

For example:

```text
Participant 1
Scores: [5, 4, None, 3, 2]
Valid observations: 4
Mean: 3.5
```

Then calculate every available consecutive change.

Example:

```text
Round 1 → 2: -1
Round 2 → 3: unavailable
Round 3 → 4: unavailable
Round 4 → 5: -1
```

Reuse your existing functions.

---

# Assignment Part F — Deliberately introduce a bug

After your tests pass, deliberately break:

```python
score_change()
```

Change:

```python
return current - previous
```

to:

```python
return previous - current
```

Run:

```bash
python tests/week04_assert_tests.py
```

At least one assertion should fail.

Answer:

1. Which assertion failed?
2. What value did you expect?
3. What value did the broken function return?
4. Why did the test detect the problem?

Then restore the correct function and confirm that all tests pass again.

---

# Assignment Part G — Advanced integration task

This is the only advanced task this week.

Do not start it until Parts A–F work correctly.

## Build a small longitudinal analysis report

Using:

```python
clean_scores
```

produce a summary for the entire cohort.

Your program should calculate:

1. total number of possible observations;
2. total number of valid observations;
3. total number of missing observations;
4. percentage of observations that are missing;
5. number of participants with all five observations;
6. number of participants with at least one missing observation;
7. mean score for each round;
8. number of observed deteriorations between each pair of consecutive rounds.

Your output might look like:

```text
COHORT SUMMARY

Participants: 5
Rounds: 5
Possible observations: 25

Valid observations: ___
Missing observations: ___
Missing percentage: ___%

Mean score by round:
Round 1: ___
Round 2: ___
Round 3: ___
Round 4: ___
Round 5: ___

Observed deteriorations:
Round 1 → 2: ___
Round 2 → 3: ___
Round 3 → 4: ___
Round 4 → 5: ___
```

### Rules

You must reuse the functions you already created.

Do not rewrite existing logic unnecessarily.

Use:

```text
functions
conditions
Boolean logic
lists
indices
loops
nested loops
None
counters
accumulators
```

from previous weeks.

---

## Advanced-task tests

Before trusting the report, write at least three additional assertions.

For example, check that:

```text
valid observations + missing observations
=
total possible observations
```

You could also test that:

```text
complete participants
+
participants with missing observations
=
total participants
```

Think of one additional logical property that should always be true.

Write an assertion for it.

---

# Minimum testing requirement

Your test file should contain at least:

```text
20 assertions
```

They should cover different situations rather than repeating the same test with slightly different values.

Include:

```text
normal values
boundary values
missing values
empty sequences
invalid values
```

---

# Required files

Your repository should now contain:

```text
nhats-deep-learning/
├── src/
│   ├── week01_first_program.py
│   ├── week02_conditions.py
│   ├── week03_loops_sequences.py
│   └── week04_testing.py
│
├── tests/
│   └── week04_assert_tests.py
│
├── notes/
│   ├── week01_nhats_files.md
│   ├── week02_clock_codebook.md
│   └── week03_nhats_cohort.md
│
├── README.md
├── environment.yml
└── .gitignore
```

---

# Run your main program

From the repository root:

```bash
conda activate nhats-course
python src/week04_testing.py
```

---

# Run your tests

Then run:

```bash
python tests/week04_assert_tests.py
```

The final line should be:

```text
All Week 4 tests passed.
```

---

# Commit your work

Check:

```bash
git status
```

Then:

```bash
git add src/week04_testing.py tests/week04_assert_tests.py
git commit -m "Complete Week 4 testing and debugging"
git push
```

Confirm the files appear in your GitHub repository.

---

# Suggested study order

| Activity | Approximate time |
|---|---:|
| CS50P exceptions | 1.5–2 h |
| Exception examples | 0.5 h |
| CS50P unit testing | 1–1.5 h |
| `assert` exercises | 1 h |
| Cleaning functions | 1–1.5 h |
| Basic tests | 1–1.5 h |
| Cohort cleaning and analysis | 2 h |
| Debugging exercise | 0.5 h |
| Advanced integration task | 1.5–2 h |
| Git and review | 0.5 h |
| **Total** | **~11–12 h** |

---

# What not to use this week

Do not use:

```text
NumPy
pandas
real NHATS files
Jupyter notebooks
pytest fixtures
mocking
test coverage tools
GitHub Actions
```

The goal is to understand testing and debugging using ordinary Python first.

---

# Self-check

Answer these without looking at your notes.

### 1

What is the difference between a syntax error and a runtime exception?

### 2

Can code run successfully and still be incorrect?

Give an example.

### 3

What does:

```python
try:
```

mean?

### 4

What does:

```python
except ValueError:
```

mean?

### 5

Why can this be dangerous?

```python
except:
    pass
```

### 6

What does:

```python
raise ValueError(...)
```

do?

### 7

What does:

```python
assert score_change(5, 4) == -1
```

mean?

### 8

What happens when an assertion is false?

### 9

Why should a missing code become:

```python
None
```

rather than:

```text
0
```

?

### 10

Why must a valid zero remain:

```text
0
```

?

### 11

What is the difference between:

```text
normal case
boundary case
missing case
invalid case
```

when testing a function?

### 12

Why do five observations produce at most four consecutive changes?

### 13

Why should you test a function using a small synthetic dataset before applying it to real research data?

### 14

Why is:

```python
current - previous
```

different from:

```python
previous - current
```

?

### 15

How can an assertion help detect a mistake that does not produce a Python exception?

---

# Pass criterion

You are ready for Week 5 when you can independently perform:

```text
raw value
↓
cleaning function
↓
cleaned value
↓
analysis function
↓
expected result
↓
assert
↓
pass or fail
↓
debug if needed
```

You should be able to:

- write a function;
- define what it should do;
- test normal behavior;
- test boundaries;
- test missing values;
- reject invalid values;
- read an error message;
- identify a failing assertion;
- find and repair a bug;
- combine several small functions into a larger analysis.

---

# Why Week 4 comes before real NHATS data

Next week, you will begin working with real NHATS files.

Before applying code to hundreds or thousands of observations, we want to know that the basic operations behave correctly.

The workflow will eventually become:

```text
real NHATS raw value
↓
cleaning
↓
validated score
↓
summary
↓
analysis
```

Week 4 teaches you how to test the individual pieces before trusting the entire pipeline.

---

## Standing course rule

Make the first attempt at each task yourself.

AI may help you:

- understand an error message;
- understand why an assertion failed;
- review your reasoning;
- explain a concept after you have attempted it.

Do not ask AI to write the entire graded assignment before attempting it yourself.

Never paste real participant-level NHATS data into an external AI assistant.

