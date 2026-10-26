# Week 1 — Windows/macOS setup, terminal, Conda, pip, Git, GitHub and your first Python program

## Learning objectives

By the end of this week you should be able to:

1. Open a command-line terminal: **Git Bash** on Windows or the built-in **Terminal** on macOS; install or verify Git.
2. Navigate folders and create files from a terminal.
3. Use VS Code to edit a `.py` file and run it.
4. Explain what Python, Conda, pip, Git and GitHub each do.
5. Create, activate, deactivate and inspect a Conda environment.
6. Install, inspect and uninstall a package using **pip**, practise Conda package installation, and understand when each is appropriate.
7. Use Python variables, numeric types, f-strings and basic arithmetic.
8. Initialise Git, create a commit and push a private repository to GitHub.
9. Describe the three NHATS file families (sample-person, other-person and tracker) using NHATS documentation.
10. Keep sensitive or restricted data out of the repository.

## Study order and video-first resources

**How to use resources:** Watch the linked video or video lesson first, pause to reproduce what it demonstrates, then consult the documentation if a step is unclear. Some longer courses are listed for **selected sections only**; do not try to finish them all this week.

### Step 1 — Open your terminal (Windows or macOS) and learn the shell

**Windows users:**

1. Download and install [Git for Windows](https://git-scm.com/install/windows). Choose the installer for your computer and follow the setup wizard using sensible defaults.
2. Open **Git Bash** from the Windows Start menu. Git for Windows includes Git Bash, so no separate terminal installation is needed.
3. Use **Git Bash** for the Step 1 shell exercises and the Git exercises in Step 6.

**macOS users:**

1. Open the built-in **Terminal** via **Applications → Utilities → Terminal**, or press `Command + Space`, search for `Terminal` and press Return.
2. Check Git with `git --version`. If macOS offers to install **Apple Command Line Tools**, accept the prompt. If no prompt appears and Git is unavailable, consult [Apple's command-line developer tools guidance](https://developer.apple.com/xcode/resources/) and install the command-line tools before Step 6.
3. Use **Terminal** for the Step 1 shell exercises and Git exercises in Step 6. macOS normally uses the **zsh** shell; MIT's introductory shell concepts still apply.

**Watch (video-first, both operating systems):**
- [MIT Missing Semester: The Shell](https://missing.csail.mit.edu/2020/course-shell/) — primary video lecture with notes and exercises. Focus on navigation, file paths, running commands, and what a shell does.
- [GitHub: A brief introduction to Git for beginners](https://www.youtube.com/watch?v=r8jQ9hVA2qs) — optional installation and Git introduction; Git commands are covered in Step 6.

**Practise in Git Bash (Windows) or Terminal (macOS):**

```bash
pwd                         # Print working directory
ls                          # List files
mkdir nhats-deep-learning   # Create your course project folder
cd nhats-deep-learning      # Enter it
touch hello.txt             # Create an empty file
ls                          # Verify the file exists
cd ..                       # Go up one directory
git --version               # Confirm Git is available
```

Learn `pwd`, `ls`, `cd`, `mkdir`, `touch`, `cat`, `clear`, `.` and `..`, and what a **file path** means. Understand relative versus absolute paths. **Windows:** Git Bash may show `C:\Users\...` as `/c/Users/...`. **macOS:** home folders are commonly under `/Users/...`.

**Checkpoint:** Open the terminal without help, create a folder and file, and navigate into and out of the folder without using File Explorer or Finder.

### Step 2 — Install VS Code and connect it to a terminal

**Install:** [Visual Studio Code](https://code.visualstudio.com/download). In Extensions, install Microsoft's **Python** extension.

**Watch:**
- [Visual Studio Code: Official beginner tutorial (2026)](https://www.youtube.com/watch?v=f8_uF_IDV50) — basic editor navigation and file creation. Watch the beginner/editor sections; AI features are not required.
- [VS Code introductory videos](https://code.visualstudio.com/docs/getstarted/introvideos) — use the basics/editing clips if further explanation is needed.

**Practice:** Open your `nhats-deep-learning` folder using **File → Open Folder**. Open **Terminal → New Terminal**. **Windows:** select Git Bash if available. **macOS:** the integrated terminal normally uses zsh. Create a blank `hello.py` file in the editor. You will execute Python after installing Miniforge. Do not rely only on VS Code’s **Run** button; in Step 5 you will execute the script from the shell.

**Checkpoint:** Explain the difference between VS Code (editor), your terminal/shell (Git Bash or macOS Terminal) and Python (interpreter).

### Step 3 — Install Miniforge and learn Conda environments

**Install:** [Miniforge](https://github.com/conda-forge/miniforge), choosing the correct installer for **Windows or macOS** and your computer's processor (for example, Apple Silicon versus Intel on macOS). Miniforge supplies Conda and the conda-forge channel without installing the full Anaconda distribution.

- **Windows:** After installation, open **Miniforge Prompt** from the Start menu. Use it for Conda and Python commands in Steps 3–5; Git Bash remains the shell for Git practice.
- **macOS:** Follow the Miniforge installation instructions, then open or reopen **Terminal**. If the installer has not configured your shell, follow Miniforge's documented shell-initialisation instructions; on a typical modern Mac this means initialising **zsh**. Confirm that `conda --version` works before continuing. You can use the same Terminal for both Conda/Python and Git.

**Watch (selected video lessons, both operating systems):**
- [Anaconda Learning: Get Started with Anaconda](https://learning.anaconda.com/courses/get-started-with-anaconda) — focus on packages, Conda overview and environment concepts. The installation shown may use Anaconda/Miniconda; **install Miniforge using the official link above**.
- [Anaconda Learning: Conda Basics](https://learning.anaconda.com/courses/conda-basics) — focus on *Getting Started with conda* and *Working with Environments*.

**Practise in Miniforge Prompt (Windows) or Terminal (macOS):**

```bash
conda --version
conda create -n nhats-course python=3.12 pip
conda activate nhats-course
python --version
conda env list
conda list
conda deactivate
conda activate nhats-course
```

**Understand:** An *environment* is an isolated set of packages and a Python interpreter. The `base` environment is not where the course's research dependencies should live. Creating the environment does not create a Git repository.

**Shell note:** `conda activate` may not work in an uninitialised shell. **Windows beginners should use Miniforge Prompt** for this week's Conda commands instead of troubleshooting activation in Git Bash. **macOS students should use Terminal after following the Miniforge shell setup guidance.**

**Checkpoint:** Activate and deactivate `nhats-course` without referring to a video.

### Step 4 — Learn pip and compare it with Conda

**Watch:**
- [Real Python: Installing packages with pip (overview)](https://realpython.com/videos/python-pip-install-overview/) — short introductory video.
- [Real Python: Installing packages with pip](https://realpython.com/lessons/python-pip-install-packages/) — video demonstration; some content may require an account.

**Core distinction:**

| Tool | Main job |
| --- | --- |
| Python | Runs Python programs |
| Conda | Creates environments; installs Python and packages, including some non-Python dependencies |
| pip | Installs and manages Python packages, usually from PyPI, into a Python environment |
| VS Code | Edits and helps debug code |
| Git | Records local changes to a project |
| GitHub | Hosts Git repositories remotely |

**Practise in Miniforge Prompt (Windows) or Terminal (macOS), with `nhats-course` activated:**

```bash
conda activate nhats-course
python -m pip --version
python -m pip install pyjokes
python -m pip list
python -m pip show pyjokes
python -c "import pyjokes; print(pyjokes.get_joke())"
python -m pip uninstall pyjokes
```

Then compare a package installed by Conda, preferably in a separate temporary practice environment:

```bash
conda create -n conda-practice python=3.12
conda activate conda-practice
conda install numpy
conda list
conda remove numpy
conda deactivate
conda env remove -n conda-practice
conda activate nhats-course
```

Use `python -m pip` rather than a bare `pip` command so that it is clearer which Python interpreter is being used. For this course, **use Conda to manage environments and Python versions, and prefer `python -m pip install` for Python packages**. Learn `conda install` as an alternative when package compatibility or non-Python dependencies justify it. Avoid switching repeatedly between package managers in an established environment; use a disposable practice environment to compare installation methods if needed. For specialised software such as GPU-enabled PyTorch, follow the official installation instructions.

**Checkpoint:** Explain why installing a package is different from installing Python, and why you should activate the environment first.

### Step 5 — Write and run your first Python program

**Watch:** [Harvard CS50P Lecture 0 — Functions, Variables](https://www.youtube.com/watch?v=JP7ITIXGpHk). Focus on `print`, variables, strings, integers, floats, f-strings and simple operations. Functions are introduced here but do not need to be mastered yet.

Create `src/week01_first_program.py` in VS Code:

```python
participant_id = "synthetic_001"
age = 78
score_2021 = 5
score_2022 = 4

change = score_2022 - score_2021

print(f"Participant: {participant_id}")
print(f"Age: {age}")
print(f"Clock score change: {change}")
```

**Run the script from the shell, not only by clicking VS Code’s Run button.** Use **Miniforge Prompt on Windows** or **Terminal on macOS**. First change into the project folder, then activate the environment and run the file:

```bash
# Move to the nhats-deep-learning folder you created in Step 1:
cd PATH_TO_YOUR/nhats-deep-learning

conda activate nhats-course
python src/week01_first_program.py
```

Replace `PATH_TO_YOUR/nhats-deep-learning` with the real location. For example, on **macOS** you might use `cd ~/nhats-deep-learning`; on **Windows Miniforge Prompt** you might use `cd %USERPROFILE%\nhats-deep-learning` (if it is inside your user folder). Your actual path depends on where you created the folder. Confirm your location with `pwd` (macOS or Git Bash) or `cd` (Windows Miniforge Prompt) before running the script.

**Task:** Create three fictional participants using simple variables (IDs, ages, years and clock scores). Print their details and at least one score change. Predict the result before running the script. **Do not use real participant records.** Loops and richer data structures are introduced in later weeks.

**Checkpoint:** Explain the difference between `"78"`, `78` and `78.0`, and how an f-string works.

### Step 6 — Learn Git, create a private GitHub repository and push your work

**Watch:**
- [GitHub: A brief introduction to Git for beginners](https://www.youtube.com/watch?v=r8jQ9hVA2qs) — revisit staging, commits and Git versus GitHub.
- [GitHub: How to create your first GitHub repository](https://www.youtube.com/watch?v=-RZ03WHqkaY) — official beginner video.
- [Kevin Stratvert: Git and GitHub tutorial for beginners](https://www.youtube.com/watch?v=tRZGeaHPoaw) — use **00:00–13:27** for local Git and **32:37–37:29** for GitHub and first push; skip branching/merging for now.

Configure Git in **Git Bash (Windows)** or **Terminal (macOS)**:

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Use the `nhats-deep-learning` folder created in Step 1 and place all Week 1 files inside it. In Git Bash (Windows) or Terminal (macOS), navigate to that folder and run:

```bash
git init
git branch -M main
git status
```

Before staging anything, create `.gitignore` with at least:

```gitignore
# Real data must never be tracked
/data/
*.sas7bdat
*.dta
*.parquet

# Local environments and Python-generated files
.venv/
venv/
env/
__pycache__/
*.pyc
.ipynb_checkpoints/

# Secrets and credentials
.env
.env.*
*.key
```

**Important:** Keep actual NHATS files **outside the repository**, not merely inside an ignored folder. `.gitignore` is a safeguard, not a substitute for approved data storage or compliance with NHATS data conditions. Never push credentials, participant-level information or restricted data.

Create `README.md`, `environment.yml` and the NHATS notes described below. Then:

```bash
git add .
git status
git commit -m "Complete Week 1 setup and first Python script"
git log --oneline
```

In GitHub, create an **empty private repository** named `nhats-deep-learning` (do not initialise it with a GitHub README, license or `.gitignore`, since your local project already has them). Copy the HTTPS repository address shown by GitHub and run:

```bash
git remote add origin YOUR_GITHUB_REPOSITORY_URL
git push -u origin main
```

GitHub may ask you to authenticate through a browser or credential manager. **Do not enter your GitHub account password in place of a personal access token if Git asks for a password.** After the push, open GitHub in your browser and confirm that the code files are visible but data files are absent.

**Checkpoint:** In your own words, explain `git add`, `git commit`, `git push`, local repository and remote repository.

### Step 7 — Learn the NHATS file structure, without using real participant data

**Watch — official NHATS videos:** [NHATS Video Tutorials](https://nhats.org/videos). Select:

1. **Introduction to the National Health and Aging Trends Study (NHATS)** (8:24).
2. **Using NHATS (Part 1: Files)** (4:53).
3. **Using the “Other Person” (OP) Files in NHATS** (6:47).
4. **Using NHATS (Part 3: Documentation)** (5:11), if useful.

These are official, short, topic-specific tutorials. You do not need the sampling-weight or merging tutorials yet.

**Write:** `notes/week01_nhats_files.md`, answering:

- What is the **sample-person** file about?
- What is the **other-person** file about?
- What is the **tracker** file about?
- Why might these files contain different numbers of records or represent different units of observation?
- What should be checked in the documentation before joining these files?

Do not guess variable names or assume definitions are identical across rounds. Do not download restricted images or import actual NHATS data into Python this week.

## Reproducibility files

Create an `environment.yml` file with:

```yaml
name: nhats-course
channels:
  - conda-forge
dependencies:
  - python=3.12
  - pip
```

It documents a minimal Conda environment. Later weeks will introduce `requirements.txt` for pip-managed project dependencies, alongside `environment.yml`. A command such as `python -m pip freeze > requirements.txt` captures installed versions (including indirect dependencies) and should be reviewed rather than accepted blindly. Because no permanent third-party package is required in Week 1, the temporary `pyjokes` and NumPy installation experiments do not need to appear here. Optionally test recreating the environment later; do not delete your working environment just to test this in Week 1.

Your project should look like:

```text
nhats-deep-learning/
├── src/
│   └── week01_first_program.py
├── notes/
│   └── week01_nhats_files.md
├── README.md
├── environment.yml
└── .gitignore
```

In `README.md`, include the project purpose and basic instructions:

```bash
conda env create -f environment.yml
conda activate nhats-course
python src/week01_first_program.py
```

## Submit

- [ ] `src/week01_first_program.py` runs **from the terminal** in the activated Conda environment (not only from VS Code’s Run button).
- [ ] `notes/week01_nhats_files.md` accurately describes the NHATS file families.
- [ ] `README.md` explains the project and how to run the code.
- [ ] `environment.yml` contains the minimal Conda specification.
- [ ] `.gitignore` excludes sensitive files and disposable artefacts.
- [ ] A **private GitHub repository** includes at least one meaningful commit and successful push; open the repository webpage to confirm that the files are present.
- [ ] Share the **repository URL** with your assessor if an assessor or submission system has been specified; for a private repository, grant the designated reviewer access only when requested. Do not make it public just to submit.
- [ ] No participant data, credentials or local environments have been uploaded.

## Self-check (answer without copying a video)

1. What is the difference between a terminal/shell (Git Bash or macOS Terminal) and Git?
2. What does `cd ..` do?
3. How are VS Code and Python different?
4. What is a Conda environment? Why not install every package into `base`?
5. What is pip, and what is the purpose of `python -m pip install ...`?
6. What is the difference between a package and a Python interpreter?
7. What is the difference between Git and GitHub?
8. How do staging, committing and pushing differ?
9. What does $\Delta s = s_{t+1}-s_t$ mean?
10. Why must NHATS data be kept separate from version-controlled source code?

**Pass criterion:** From a fresh terminal session (Miniforge Prompt on Windows or Terminal on macOS), you can navigate to the project, activate the environment, execute the Python script, inspect Git status in your Git-capable terminal and explain how to push a new commit. Week 1 is a preparation week, not one of the course's formal unaided gates.

---

**Standing course rule:** Make the first attempt at each task yourself. AI may explain errors or review your work afterwards. Real NHATS participant-level data must not be pasted into external AI tools.
