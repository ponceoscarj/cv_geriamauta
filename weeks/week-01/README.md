## Week 1 — Environment, terminal, Git and your first Python program

**Research question:** How can we represent an older adult's characteristics and clock score in a reproducible program?

**Learning objectives**
- Run Python from a terminal and execute a `.py` script.
- Distinguish variable names, values, strings, integers and floating-point numbers.
- Create a repository, ignore data and environment files, and make your first commit.
- Translate a simple patient-score difference into Python.

**Watch**
- [CS50P Lecture 0 — Functions, Variables](https://cs50.harvard.edu/python/0/).
- [MIT Missing Semester — The Shell](https://missing.csail.mit.edu/2020/course-shell/).
- [MIT Missing Semester — Version Control (Git)](https://missing.csail.mit.edu/2020/version-control/); focus on `init`, `status`, `add`, `commit`.

**Read**
- [CS50P Lecture 0 notes](https://cs50.harvard.edu/python/weeks/0/).
- [NHATS website](https://nhats.org/) and its overview/documentation pages. Identify what the **sample-person**, **other-person**, and **tracker** files describe; do not assume they share the same unit of observation.

**Math essentials**
- Variables, symbolic notation and substitution: `age_i`, `score_i,t`.
- Change between measurements: $\Delta s=s_{t+1}-s_t\$.
- Distinguish score difference from percentage difference.

**NHATS task (synthetic data)**
1. Create fictional records for three older adults: age, year and clock score.
2. Print each participant's details using an f-string.
3. Calculate the difference between two clock scores for one participant.
4. Create a `.gitignore` that excludes virtual environments, caches, credentials and real data.
5. Write a brief note defining the three NHATS file families (sample-person, other-person, tracker) using the current documentation.

**Starter exercise**
```python
participant_id = "synthetic_001"
score_2021 = 5
score_2022 = 4

change = score_2022 - score_2021
print(f"{participant_id}: change = {change}")
```
Change the input values and predict the output *before* rerunning the script.

**Submit**
- `src/week01_first_program.py`
- `notes/week01_nhats_files.md`
- `README.md` with instructions to run your script.

**Self-check:** Can you create a repo and run the script from the terminal without copying every command from the video?

---

