# Week 3 — Loops, Sequences and Summation

## Learning objectives

By the end of Week 3, you should be able to:

1. Explain what a **sequence** is and why order matters.
2. Access elements of a Python list using an **index**.
3. Explain the difference between mathematical indexing and Python's zero-based indexing.
4. Write basic `for` and `while` loops.
5. Use `range()` and `len()`.
6. Iterate through a list of repeated patient measurements.
7. Use a loop to accumulate a total and a count.
8. Explain summation notation, \(\Sigma\), in plain language.
9. Calculate an arithmetic mean manually and in Python.
10. Calculate successive changes between longitudinal observations.
11. Handle `None` correctly when iterating through a sequence.
12. Explain why repeated observations from the same participant belong together in longitudinal data.

---

## Do you need to download NHATS this week?

**No. Do not download or import NHATS participant-level data for the Week 3 assignment.**

Week 3 uses:

- public NHATS documentation;
- the NHATS Cohort Profile;
- NHATS educational videos;
- completely synthetic patient data.

The purpose this week is to understand loops, sequences, indexing, summation, means, and longitudinal structure without adding file-import and data-cleaning complexity.

Real NHATS data will be introduced in **Week 5**.

Students who will use NHATS later should make sure they can access the NHATS system before Week 5. NHATS public-use files require registration.

- [NHATS Data Access](https://www.nhats.org/data-access)
- [NHATS Files by Round](https://www.nhats.org/data-access/nhats)

---

# 1. Main idea for the week

The central question is:

> **How do we represent and summarize repeated measurements from the same patient over time?**

For example:

```text
Patient 1: 5 → 4 → 4 → missing → 3
Patient 2: 3 → 3 → 2 → 2       → 1
Patient 3: 5 → 5 → 5 → 4       → 4
```

This week connects:

```text
sequence
→ index
→ loop
→ missing value
→ change over time
→ summation
→ arithmetic mean
```

These concepts will later reappear in longitudinal data analysis, matrices, tensors, loss functions, gradients, and neural networks.

---

# 2. Required resources

## Programming

### Harvard CS50P — Week 2: Loops

**Primary programming resource**

- [CS50P Week 2 — Loops](https://cs50.harvard.edu/python/weeks/2/)

Focus on:

- `while`
- `for`
- lists
- `range()`
- `len()`
- `None`

The same page also contains shorter videos. The most useful for this week are:

- **For Loops**
- **Lists**
- **While Loops**

You do **not** need to master dictionaries, comprehensions, or advanced list methods this week.

---

## Mathematics

### Sequences

Watch:

- [Khan Academy — Sequences intro](https://www.khanacademy.org/math/algebra-home/alg-sequences) (Focus a lot on notation!)

Focus on:

- a sequence as an ordered list;
- individual terms;
- indexing/position;
- why order matters.

You do not need to study arithmetic- or geometric-sequence formulas in depth.

### Summation notation

Watch:

- [Khan Academy — Understanding summation notation](https://www.khanacademy.org/math/algebra2/sequences-and-series/alg2-sigma-notation/v/sigma-notation-sum)

Focus on understanding:

$$ \sum_{i=1}^{n} x_i $$

You should be able to explain what the following mean:

- $\Sigma$
- $i=1$
- $n$
- $x_i$

### Arithmetic mean

Watch:

- [Khan Academy — Statistics intro: Mean, median, and mode](https://www.khanacademy.org/v/statistics-intro-mean-median-and-mode) (if you know these concepts, skip the video)

For Week 3, focus on the **mean**.[Here is **important** to understand the notation]

You should understand that:

$$
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i
$$

means:

> Add the observations and divide by the number of observations.

---

## NHATS

### NHATS video

Watch:

- [NHATS Video Tutorials](https://www.nhats.org/videos)

On that page, watch:

**NHATS Design Basics — 5:43**

Focus on:

- the population NHATS represents;
- sampling;
- oversampling;
- longitudinal follow-up;
- sample replenishment.

### NHATS reading

Read:

- [Cohort Profile: The National Health and Aging Trends Study (NHATS)](https://pmc.ncbi.nlm.nih.gov/articles/PMC6934030/)

Do not try to memorize the entire article.

Focus on answering (if you doubt your answers, bring the questions on Saturday):

1. What population does NHATS study?
2. Where did the original sampling frame come from?
3. Which groups were sampled at higher rates?
4. Why were they oversampled?
5. How often are participants followed?
6. What is sample replenishment?
7. Why is replenishment necessary?

---

# 3. From individual variables to a sequence

In previous weeks, repeated measurements might have been stored as separate variables:

```python
score_2021 = 5
score_2022 = 4
score_2023 = 3
```

Python can instead store them as one ordered list:

```python
scores = [5, 4, 3]
```

A list is an ordered sequence.

The order matters.

```text
5 → 4 → 3
```

describes a different trajectory from:

```text
3 → 4 → 5
```

For longitudinal clinical data, position often represents time.

---

# 4. Indexing

Consider:

```python
scores = [5, 4, 3, 4, 2]
```

Python assigns each element an index:

```text
Python index:    0   1   2   3   4
Score:           5   4   3   4   2
```

Try:

```python
print(scores[0])
print(scores[1])
print(scores[4])
```

Predict the output before running the code.

## Mathematical indexing versus Python indexing

Mathematics often writes a sequence as:

$$
s_1,s_2,s_3,s_4,s_5
$$

Python represents those positions as:

```python
scores[0]
scores[1]
scores[2]
scores[3]
scores[4]
```

Python therefore uses **zero-based indexing**.

This distinction will become important later with NumPy arrays, matrices, images, and tensors.

---

# 5. Why loops exist

Without a loop:

```python
print(scores[0])
print(scores[1])
print(scores[2])
print(scores[3])
print(scores[4])
```

This becomes impractical when there are hundreds or thousands of observations.

A loop repeats an operation:

```python
for score in scores:
    print(score)
```

Read this as:

> For every score in the sequence `scores`, perform the indented operation. Remember that you can use anything in `score`. Try changing `score` with your name so you can understand that it does not matter what you use for `score`; however, it is very important to understand that `scores` cannot be changed.

---

## Practice 1 — Iterate through values

```python
scores = [5, 4, 3, 4, 2]

for i in scores: 
    print(i)
```

Predict the output first.

---

## Practice 2 — `range()`

```python
for i in range(5):
    print(i)
```

Predict the output first.

`range(5)` produces:

```text
0
1
2
3
4
```

---

## Practice 3 — Loop over indices

```python
scores = [5, 4, 3, 4, 2]

for i in range(len(scores)):
    print(i, scores[i])
```

Here:

```python
len(scores)
```

returns the number of elements.

The variable `i` represents the current index.

---

# 6. Successive changes

Suppose:

```python
scores = [5, 4, 4, 3, 2]
```

The change between consecutive observations is:

$$
\Delta s_t=s_{t+1}-s_t
$$

By hand:

```text
4 - 5 = -1
4 - 4 =  0
3 - 4 = -1
2 - 3 = -1
```

In Python:

```python
scores = [5, 4, 4, 3, 2]

for i in range(len(scores) - 1):
    previous = scores[i]
    current = scores[i + 1]

    change = current - previous
    print(change)
```

Think about:

> Why do we use `len(scores) - 1`?

For five observations, there are only four consecutive pairs.

---

# 7. Missing observations

Longitudinal datasets often contain missing visits.

Use:

```python
None
```

to represent an unavailable value in this week's synthetic data.

Example:

```python
scores = [5, 4, None, 3, 2]
```

This represents:

```text
Round 1: 5
Round 2: 4
Round 3: missing
Round 4: 3
Round 5: 2
```

A missing value is **not** the same thing as zero.

`0` may be a valid score.

`None` means no usable value is available.

---

## Reuse Week 2 Boolean logic

```python
scores = [5, 4, None, 3, 2]

for i in range(len(scores) - 1):
    previous = scores[i]
    current = scores[i + 1]

    if previous is None or current is None:
        print("Change unavailable")
    else:
        change = current - previous
        print(change)
```

Conceptually:

```text
5 → 4       change = -1
4 → None    unavailable
None → 3    unavailable
3 → 2       change = -1
```

---

# 8. Summation

Consider:

```text
5, 4, 3
```

The total is:

$$
5+4+3=12
$$

Mathematics provides a compact representation:

$$
\sum_{i=1}^{3}s_i
$$

This means:

> Add each value $s_i$, starting at $i=1$ and ending at $i=3$.

If:

$$
s_1=5,\quad s_2=4,\quad s_3=3
$$

then:

$$
\sum_{i=1}^{3}s_i=5+4+3=12
$$

## Meaning of the symbols

- $\Sigma$: add values together.
- $i$: index.
- $i=1$: begin with the first value.
- $3$: stop after the third value.
- $s_i$: the value at position $i$.

---

# 9. Implement summation using a loop

The mathematical idea:

$$
\sum_{i=1}^{n}s_i
$$

can be implemented with a running total:

```python
scores = [5, 4, 3]

total = 0

for score in scores:
    total = total + score

print(total)
```

Follow the accumulator manually:

```text
Start:        total = 0
After 5:      total = 5
After 4:      total = 9
After 3:      total = 12
```

`total` is an **accumulator**.

---

# 10. Arithmetic mean

The arithmetic mean is:

$$
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i
$$

For:

```text
5, 4, 3
```

the mean is:

$$
\frac{5+4+3}{3}=4
$$

Implement it manually:

```python
scores = [5, 4, 3]

total = 0
count = 0

for score in scores:
    total = total + score
    count = count + 1

mean = total / count

print(mean)
```

For the first implementation, do not use `sum()` or a statistics package.

The goal is to understand what the computer is doing.

---

# 11. Mean with missing observations

Consider:

```python
scores = [5, 4, None, 3, None]
```

Only valid observations should contribute:

```python
total = 0
count = 0

for score in scores:
    if score is not None:
        total = total + score
        count = count + 1

mean = total / count

print(mean)
```

The valid scores are:

```text
5, 4, 3
```

Therefore:

$$
\bar{x}=\frac{5+4+3}{3}=4
$$

**Do not count missing observations as zero.**

---

# 12. From one participant to many participants

A longitudinal study contains many participants.

For example:

```python
patients = [
    [5, 4, 4, 3, 3],
    [3, 3, None, 2, 1],
    [5, 5, 4, 4, None]
]
```

Conceptually:

```text
             Round (or surveys/censos/visitas)
             1   2   3   4   5

Patient 1    5   4   4   3   3
Patient 2    3   3   .   2   1
Patient 3    5   5   4   4   .
```

You can see that patients is a python variable that has 3 lists inside 1 list. That means that the outer list is only 1 that has 3 inner lists each of which belongs to a participant. Inside each of the inner list you have 5 measurements or values.

The outer list contains participants.

Each inner list contains the repeated measurements for one participant.

---

## Nested loops

```python
for patient_scores in patients:
    for score in patient_scores:
        print(score)
```

The outer loop moves between participants.

The inner loop moves through the observations for the current participant.

This is similar to how a matrix is interpreted so try to understand these concepts carefully. Do not worry about matrices yet. 

For now, think:

> many participants, each containing an ordered sequence of observations.

---

# 13. Week 3 assignment

## Goal

Create a synthetic longitudinal dataset with:

```text
100 participants × 5 annual observations
```

The assignment practises:

- lists;
- indexing;
- loops;
- nested loops;
- missing observations;
- successive differences;
- summation;
- arithmetic means.

Do not use real participant data.

---

## Part A — One participant

Create:

```python
scores = [5, 4, None, 3, 2]
```

Using loops:

1. Print every observation.
2. Print the Python index of every observation.
3. Count the number of valid observations.
4. Count the number of missing observations.
5. Calculate the sum of the valid observations.
6. Calculate the mean of the valid observations.
7. Calculate every successive change for which both observations are available.

For the first implementation, do not use:

```python
sum()
```

Use an accumulator.

Your successive-change logic should represent:

```text
5 → 4       change = -1
4 → None    unavailable
None → 3    unavailable
3 → 2       change = -1
```

---

## Part B — Five participants

Create five fictional participants with five rounds each.

Example:

```python
patients = [
    [5, 4, 4, None, 3],
    [3, 3, 2, 2, 1],
    [5, 5, 4, 4, None],
    [2, None, 2, 1, 1],
    [4, 4, 3, 3, 3]
]
```

Using loops and nested loops:

1. Print each participant's trajectory.
2. Count valid observations for each participant.
3. Count missing observations for each participant.
4. Calculate valid successive changes for each participant.
5. Calculate the mean score for each round across participants.

Do not use NumPy or pandas.

---


## Part C — Simulate 100 participants

Now expand the same structure to:

```text
100 participants × 5 rounds
```

Use Python's built-in `random` module.

Start with:

```python
import random

random.seed(42)
```

The fixed seed makes the simulation reproducible. If you run the same code again with the same seed, Python will generate the same sequence of random values. You can use any number.

### How the synthetic data will be generated

For this part, you are **not expected to invent the simulation logic from scratch**.

The purpose of Week 3 is to practise:

- loops;
- nested loops;
- sequences;
- missing values;
- counting;
- summation;
- arithmetic means;
- longitudinal changes.

Use the following starter code to generate the synthetic cohort:

```python
import random

random.seed(42)

patients = []

for participant in range(100):
    scores = []

    for round_number in range(5):

        if random.random() < 0.15:
            score = None
        else:
            score = random.randint(0, 5)

        scores.append(score)

    patients.append(scores)
```

### What does this code do?

The line:

```python
random.random()
```

generates a random decimal number between `0` and `1`.

For example:

```text
0.04
0.72
0.31
0.91
```

The condition:

```python
random.random() < 0.15
```

has approximately a **15% chance of being `True`**.

When it is `True`, the observation is recorded as missing:

```python
score = None
```

Otherwise:

```python
score = random.randint(0, 5)
```

generates a random integer between `0` and `5`, including both `0` and `5`.

For example:

```text
0
3
5
2
4
```

The method:

```python
scores.append(score)
```

adds the newly generated score to the current participant's sequence.

After five rounds, this:

```python
patients.append(scores)
```

adds the participant's complete sequence to the cohort.

The final variable:

```python
patients
```

therefore contains 100 participants, each with five observations.

Conceptually, part of the dataset might look like:

```python
[
    [5, 4, None, 2, 3],
    [1, 3, 4, 4, None],
    [5, 2, 2, 0, 1]
]
```

The exact values generated by the code will depend on the sequence produced by the random-number generator.

Because we use:

```python
random.seed(42)
```

the results will be reproducible: running the same code again with the same seed will generate the same simulated dataset.

### Why use `random.random()` for missing observations?

The expression:

```python
random.random() < 0.15
```

can be interpreted as:

> Give each observation approximately a 15% chance of being missing.

For example:

```text
random number = 0.08  → less than 0.15 → missing
random number = 0.67  → greater than 0.15 → observed
random number = 0.14  → less than 0.15 → missing
random number = 0.52  → greater than 0.15 → observed
```

This is only a simple simulation.

In real longitudinal studies, missing observations are often **not random**. Older or more impaired participants, for example, may have different probabilities of missing assessments.

You will study missing-data mechanisms in more detail later in the course.

### Important: `None` is not zero

`None` represents a **missing observation**.

It does **not** represent a clock score of zero.

A score of:

```python
0
```

is a valid simulated clock score.

A value of:

```python
None
```

means that no usable score is available for that visit.

For example:

```python
scores = [4, 3, None, 2, 0]
```

means:

```text
Round 1: score = 4
Round 2: score = 3
Round 3: missing
Round 4: score = 2
Round 5: score = 0
```

The final `0` is a real observation.

The `None` is missing.

### Understanding the nested loops

Look carefully at:

```python
for participant in range(100):
```

This loop runs **100 times**, once for each simulated participant.

Inside it:

```python
scores = []
```

creates a new empty list for that participant.

Then:

```python
for round_number in range(5):
```

runs five times, representing five longitudinal rounds.

Each time through the inner loop, Python generates either:

```python
None
```

or a clock score between:

```text
0 and 5
```

The score is then added to the participant's sequence:

```python
scores.append(score)
```

After all five rounds are complete:

```python
patients.append(scores)
```

adds that participant to the overall cohort.

The structure is therefore:

```text
patients
│
├── participant 1
│   ├── round 1
│   ├── round 2
│   ├── round 3
│   ├── round 4
│   └── round 5
│
├── participant 2
│   ├── round 1
│   ├── round 2
│   ├── round 3
│   ├── round 4
│   └── round 5
│
└── ...
```

### Check the simulated cohort

After generating the cohort, inspect the first few participants:

```python
for i in range(5):
    print(patients[i])
```

You should see five lists.

Each list should contain five elements.

You can also check:

```python
print(len(patients))
```

The result should be:

```text
100
```

Then check:

```python
print(len(patients[0]))
```

The result should be:

```text
5
```

Before running these lines, predict the results.

### Simulation rules

For this exercise:

- simulate `100` participants;
- give each participant `5` rounds;
- use synthetic clock scores from `0` to `5`;
- represent missing observations with `None`;
- give each observation approximately a `15%` probability of being missing;
- use `random.seed(42)` so the simulation is reproducible;
- do not use real NHATS participant data;
- do not use NumPy;
- do not use pandas.

This simulation is intentionally simple.

It does **not** attempt to reproduce the true NHATS data-generating process.

Its purpose is to give you a longitudinal dataset on which to practise loops, indexing, missing values, summation, and arithmetic means.

### Optional improvement

The basic simulation generates each observed score independently:

```python
score = random.randint(0, 5)
```

This means a participant could have a trajectory such as:

```text
5 → 0 → 5 → 1 → 4
```

That is possible in the simulation but may not resemble a realistic longitudinal cognitive trajectory.

As an **optional extension**, try generating later scores based partly on the previous score.

For example, a score could usually change by:

```text
-1
0
+1
```

while remaining between:

```text
0 and 5
```

Do not attempt this extension until the basic assignment works correctly.

The basic simulation is sufficient for Week 3.

---


## Part D — Summarize the simulated cohort

Using loops, calculate for each round:

```text
Round 1: valid = ___, missing = ___, mean = ___
Round 2: valid = ___, missing = ___, mean = ___
Round 3: valid = ___, missing = ___, mean = ___
Round 4: valid = ___, missing = ___, mean = ___
Round 5: valid = ___, missing = ___, mean = ___
```

Also calculate:

1. Total number of observations.
2. Total number of valid observations.
3. Total number of missing observations.
4. Number of participants with all five observations.
5. Number of participants with at least one missing observation.

For the **first five participants**, print:

- the complete trajectory;
- all valid successive changes.

---

# 14. Written NHATS assignment

Create:

```text
notes/week03_nhats_cohort.md
```

Write approximately half a page answering:

1. What population does NHATS study?
2. How was the original cohort sampled?
3. Which groups were oversampled?
4. Why were they oversampled?
5. How often are participants followed?
6. What does sample replenishment mean?
7. Why does NHATS periodically replenish the cohort?

Use the:

- [NHATS Design Basics video](https://www.nhats.org/videos)
- [NHATS Cohort Profile](https://pmc.ncbi.nlm.nih.gov/articles/PMC6934030/)

as your sources.

Then add a short section titled:

## How our simulation differs from real NHATS

Identify at least **three limitations** of your simulation.

Consider questions such as:

- Is real missingness random?
- What happens when participants die?
- Can participants permanently leave a longitudinal study?
- Are real assessment intervals always exactly one year?
- What happens when a study recruits a replenishment sample?
- Do cognitive scores really follow a simple random rule?
- Does NHATS use simple random sampling?

You do not need to solve these issues yet.

The goal is to recognize them.

---

# 15. Required files

Your repository should contain:

```text
nhats-deep-learning/
├── src/
│   ├── week01_first_program.py
│   ├── week02_conditions.py
│   └── week03_loops_sequences.py
├── notes/
│   ├── week01_nhats_files.md
│   ├── week02_clock_codebook.md
│   └── week03_nhats_cohort.md
├── README.md
├── environment.yml
└── .gitignore
```

---

# 16. Run the Week 3 script

From the terminal:

```bash
conda activate nhats-course
cd path/to/nhats-deep-learning
python src/week03_loops_sequences.py
```

Confirm that the program runs from beginning to end.

---

# 17. Commit your work

From the repository:

```bash
git status
git add src/week03_loops_sequences.py notes/week03_nhats_cohort.md
git commit -m "Complete Week 3 loops and longitudinal simulation"
git push
```

Open GitHub and confirm that the files and latest commit are present.

---

# 18. What not to use this week

Do not use:

- pandas;
- NumPy;
- Jupyter notebooks;
- automated unit tests;
- `pytest`;
- exception handling;
- real NHATS participant-level data.

These topics are deliberately reserved for later weeks.

The goal is to understand the operations before libraries automate them.

---

# 19. Suggested study order

| Activity | Approximate time |
|---|---:|
| Review Week 2 (`if`, `None`, functions) | 0.5 h |
| CS50P loops lecture / selected shorts | 2–2.5 h |
| Sequences and indexing | 0.75 h |
| Summation and arithmetic mean | 1–1.5 h |
| NHATS Design Basics + focused Cohort Profile reading | 1–1.5 h |
| Programming assignment | 4 h |
| Written NHATS reflection | 1 h |
| **Total** | **~11–11.5 h** |

---

# 20. Self-check

Answer these without looking at your code.

### 1

Given:

```python
scores = [5, 4, None, 3]
```

what are:

```python
scores[0]
scores[2]
```

### 2

Why is the first Python list element indexed by `0`, while mathematical notation often calls the first observation \(s_1\)?

### 3

Explain in plain language:

```python
for score in scores:
```

### 4

What does:

```python
range(5)
```

produce?

### 5

What is the difference between:

```python
for score in scores:
```

and:

```python
for i in range(len(scores)):
```

### 6

Explain:

\[
\sum_{i=1}^{n}x_i
\]

without simply saying "sigma notation."

### 7

Write a loop that calculates the total of:

```python
scores = [4, 5, 3]
```

without using `sum()`.

### 8

How do `total` and `count` allow you to calculate a mean?

### 9

Why should `None` not be treated as `0`?

### 10

A participant has five annual measurements. How many consecutive changes can exist at most? Why?

### 11

Why are five measurements from one participant different from one measurement from five different participants?

### 12

What does a fixed random seed do?

---

# 21. Pass criterion

You are ready for Week 4 when you can:

- create and access a Python list;
- use indices correctly;
- write a `for` loop without copying one from a tutorial;
- explain `range()` and `len()`;
- calculate successive differences;
- handle `None` inside a loop;
- calculate a sum manually using an accumulator;
- calculate a mean from `total` and `count`;
- explain \(\Sigma\) notation in plain language;
- explain why repeated observations from one participant form a longitudinal sequence;
- explain the basic NHATS sampling and longitudinal structure;
- run and commit the Week 3 program independently.

---

# 22. Scope boundary

Week 3 is about:

```text
loops
sequences
indexing
summation
means
longitudinal repeated measurements
```

Week 4 will introduce:

```text
exceptions
debugging
automated tests
pytest
```

Week 5 will introduce:

```text
NumPy
file input/output
real NHATS data
```

Do not move those topics forward yet.

---

## Standing course rule

Make the first attempt at each task yourself.

AI may help explain errors, review reasoning, or clarify concepts after you have attempted the work.

Never paste real participant-level NHATS data into an external AI assistant.
