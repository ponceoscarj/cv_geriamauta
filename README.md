## Structure

| Phase | Weeks | Focus | Main NHATS output |
| --- | --- | --- | --- |
| A1 | 1–6 | Python and workflow | Tested cleaning functions; first NHATS DataFrame |
| A2 | 7–12 | Data analysis, survey design, missingness, outcomes, survival | Longitudinal cohort table; data dictionary; outcome protocol v0.1 |
| A3 | 13–18 | Linear algebra, probability, statistics, classical ML | Tabular benchmark (Model A) |
| A4 | 19–24 | Calculus, optimisation, backprop, PyTorch | PyTorch tabular MLP; Stanford readiness test |
| B | 25–30 | CS231n core: linear classifiers to CNNs and transfer learning | Clock-image CNN; pretrained embeddings |
| C | 31–40 | RNN/LSTM, transformers/ViT, self-supervised learning/JEPA, VAE, diffusion, world models | Each method tried on NHATS; study protocol |
| D | 41–48 | Final research project | Temporal JEPA predictor, evaluation, manuscript draft |

Final objective:

> clock image I(i,t) → image encoder → z(i,t) → temporal predictor (with elapsed time) → predicted z(i,t+1) → clinical outcome

You will test whether this adds prediction of future cognitive decline beyond the conventional clock score and standard pretrained embeddings. A null result is an acceptable, publishable outcome.

## Resources (use these throughout)

| Resource | Purpose |
| --- | --- |
| [Harvard CS50P](https://cs50.harvard.edu/python/) | Python from zero |
| [MIT Missing Semester](https://missing.csail.mit.edu/) | Command line, Git, environments |
| [Python for Data Analysis (McKinney, free online)](https://wesmckinney.com/book/) | pandas depth |
| [Khan Academy](https://www.khanacademy.org/math) | Algebra, calculus, probability drills |
| [3Blue1Brown](https://www.3blue1brown.com/) | Linear algebra, calculus, neural-network intuition |
| [Mathematics for Machine Learning (free PDF)](https://mml-book.com/) | Linear algebra, calculus and probability exercises |
| [The Matrix Calculus You Need for Deep Learning](https://arxiv.org/abs/1802.01528) | Gradients with respect to vectors and matrices |
| [StatQuest](https://statquest.org/video_index.html) | Statistics and ML concepts |
| [OpenIntro Statistics](https://www.openintro.org/book/os/) | Probability and inference |
| [ISLP (Introduction to Statistical Learning, Python)](https://www.statlearning.com/) | ML textbook and labs |
| [Karpathy, Neural Networks: Zero to Hero](https://karpathy.ai/zero-to-hero.html) | Backprop from scratch (micrograd), later transformers |
| [Stanford CS231n 2025 videos](https://www.youtube.com/playlist?list=PLoROMvodv4rOmsNzYBMe0gJY2XS8AQg16) and [2026 schedule/slides](https://cs231n.stanford.edu/schedule.html) | Main computer-vision course |
| [PyTorch tutorials](https://docs.pytorch.org/tutorials/) | Implementation |
| [NHATS](https://nhats.org/) | Data, user guide, technical papers, codebooks |

Lecture numbers below follow the 2026 CS231n syllabus, which I checked. The public 2025 videos may be numbered differently, so match them by title.

**Open datasets used when NHATS cannot supply what a week needs:**

| Dataset | Use |
| --- | --- |
| [NHANES](https://wwwn.cdc.gov/nchs/nhanes/) and its [linked mortality files](https://www.cdc.gov/nchs/data-linkage/mortality.htm) | Survey weights, strata and clusters; mortality outcomes |
| [Health and Retirement Study](https://hrs.isr.umich.edu/) (free registration) | Longitudinal cognition and ageing, a second cohort for robustness checks |
| [MIMIC-IV demo](https://physionet.org/content/mimic-iv-demo/) and [PhysioNet/CinC Challenge 2012](https://physionet.org/content/challenge-2012/) | Irregularly sampled clinical time series |
| [MedMNIST](https://medmnist.com/) | Small open clinical images for development when compute is limited |
| scikit-learn, [scikit-survival](https://scikit-survival.readthedocs.io/) and [lifelines](https://lifelines.readthedocs.io/) built-in datasets | Learning APIs quickly |

## Standing rules

**Data handling.** Keep NHATS files in an approved folder outside the Git repository (add it to `.gitignore`) and work in an environment permitted by the NHATS data terms. Run NHATS image work on approved local or institutional machines. Use the open datasets for anything on shared cloud notebooks. Never paste participant-level data into an external AI assistant.

**NHATS conventions.** Keep a data dictionary (variable, round, definition, missing codes). Negative codes mean different things (inapplicable, refused, don't know, missing). Never assume a similarly named variable means the same thing in every round.

**Leakage.** Split by participant, never by image or observation. Use only information available at or before the prediction time. Never tune on the test set.

**AI-assistant policy.** The gate exercises (Weeks 6, 12, 18, 24, 30, 40) are done unaided. Elsewhere, use an assistant to explain error messages, review your code and explain concepts after you have attempted the work yourself. Write the first version of anything you will be tested on.

**Compute.** Weeks 1–24 run on a laptop. From Week 25 you need a GPU. Use an approved institutional or local GPU for NHATS images. Use MedMNIST on free cloud GPUs for hyperparameter exploration.

## Weekly pattern

| Activity | Hours |
| --- | --- |
| Lectures and videos | 3 |
| Mathematics exercises by hand | 2 |
| Reading (notes, papers, codebooks) | 1.5 |
| Programming task | 4 |
| Written reflection (research question, method, result, limitation) | 1 |
| **Total** | **11.5** |

Every notebook has the same structure: research question, mathematics, implementation, tests, findings, limitations. From Week 12 onward, each notebook rebuilds from raw files with one command, uses fixed random seeds, and is committed to Git.

**Gates.** Gate weeks are repeat-until-pass. Budget up to six slip weeks across the course.

---

# Part A1 — Python and workflow (Weeks 1–6)

NHATS-shaped records and the NHATS documentation are used until Week 5, when you load the real files.

### Week 1 — Setup, command line, Git, first program

- **Learn:** Missing Semester (shell, version control); CS50P Lecture 0 (functions, variables); Khan Academy arithmetic, variables, solving linear equations such as x + 3 = 7.
- **NHATS task:** Create your NHATS account and download the public-use files for one round. Store them outside the repo. Read the user guide's file list and write down what the sample-person file, other-person file and tracker file contain. In Python, store three hand-entered patients (age, clock score, year) as variables and compute score changes.
- **Deliverable:** A Git repo with README, `.gitignore`, a virtual environment, and a script that runs from the command line.

### Week 2 — Conditions, Boolean logic, functions

- **Learn:** CS50P Lecture 1; inequalities; `and`/`or`/`not`; functions as inputs and outputs, f(x).
- **NHATS task:** In the NHATS codebook, find how the clock score is defined (range, scoring rules), which codes mean "not done", and who is not asked to draw a clock. Write this up in a Markdown file. Then write `is_worsening(previous, current)` that handles missing values correctly.
- **Deliverable:** A tested-by-hand function plus a one-page codebook note.

### Week 3 — Loops, sequences, summation

- **Learn:** CS50P Lecture 2; sequences, indexing, Σ notation, arithmetic mean.
- **NHATS task:** Read the [NHATS Cohort Profile](https://pmc.ncbi.nlm.nih.gov/articles/PMC6934030/): sampling frame, oversampled groups, rounds, replenishment. Simulate 100 patients × 5 annual clock scores with a fixed seed, shaped like NHATS (including missing visits). Compute within-person changes and per-round means with loops.
- **Deliverable:** A simulation script and a half-page summary of the NHATS design.

### Week 4 — Exceptions, unit tests, debugging

- **Learn:** CS50P Lecture 3 (exceptions) and Lecture 5 (unit tests); pytest; Δs(t) = s(t+1) − s(t).
- **NHATS task:** Write and test `clean_clock_score(value)`, which turns NHATS special codes into missing values, using the codes you documented in Week 2. Add tested functions for score change, mean, deterioration flag and number of valid observations.
- **Deliverable:** A pytest suite that passes.

### Week 5 — NumPy, file I/O, first real NHATS data

- **Learn:** CS50P Lecture 4 (libraries) and Lecture 6 (file I/O); 3Blue1Brown Essence of Linear Algebra Chapters 1–2 (vectors); NumPy absolute-beginners guide; pandas `read_sas` / `read_stata` / `read_csv` depending on the release format.
- **NHATS task:** Load one round's sample-person file. Print its shape, list columns, extract the clock-score column, count valid scores, and compare a loop against a vectorised NumPy calculation of the same summary.
- **Deliverable:** A notebook that loads, cleans and summarises one NHATS round.

### Week 6 — pandas I, plotting, classes · **Gate A1**

- **Learn:** McKinney Chapters 5–7 and 9 (pandas basics, loading, cleaning, plots); CS50P Lecture 8 (classes) and Lecture 9 (type hints, comprehensions, generators). Classes matter because PyTorch models are classes.
- **NHATS task:** Build a participant-level DataFrame for one round: age, sex, education, clock score, recall score, orientation items. Produce descriptive tables and a histogram of clock scores. Save to CSV/Parquet and reload.
- **Gate (unaided):** Write a function that returns successive differences of an array of annual scores, with tests. Explain Σ, an index and a function in your own words.

---

# Part A2 — Data analysis, survey design, outcomes (Weeks 7–12)

### Week 7 — Merge, reshape, long vs wide

- **Learn:** McKinney Chapters 8 and 10 (joins, reshape, groupby); sets, unique IDs, join types.
- **NHATS task:** Merge Rounds 1–3 by participant ID. Reshape wide ↔ long. Build the participant × round table. Count participants with at least 2 and at least 3 valid clock scores. Start the data dictionary (variable, round, definition, missing codes).
- **Deliverable:** The participant × round table and a data dictionary draft.

### Week 8 — Longitudinal structure and time

- **Learn:** Slopes, rate of change per unit time, coordinate graphs (Khan Academy linear equations and graphs); 3Blue1Brown Chapter 3 (linear transformations) as preview.
- **NHATS task:** Use the tracker file to build a participant-round status table (interviewed, deceased, other). Compute elapsed time between assessments. Draw spaghetti plots of clock-score trajectories and compute person-level slopes.
- **Deliverable:** Trajectory plots, a status-by-round table, and a note on how irregular the intervals are.

### Week 9 — Missingness

- **Learn:** MCAR / MAR / MNAR; Sterne et al., "Multiple imputation for missing data" (BMJ 2009); proportions and conditional frequencies.
- **NHATS task:** Tabulate clock availability by respondent type (self vs proxy), age, and dementia classification. Show that missing clocks are not random. Fit a first logistic regression of "clock missing" on covariates.
- **Deliverable:** A table and a paragraph on why complete-case analysis would bias results, since the people who cannot draw a clock tend to be the most impaired.

### Week 10 — Survey design: weights, strata, clusters, attrition

- **Learn:** NHATS user guide and technical papers on sample design, analytic weights and replenishment; weighted means; design-based variance (Taylor series or replicate weights). Use R's `survey` package or Python's `samplics` for design-based variance.
- **NHATS task:** Compute weighted versus unweighted mean clock score by round and age group. Work out which weight applies to which analysis (cross-sectional, longitudinal), and document it in the data dictionary.
- **If you want worked tutorials:** NHANES (public tutorials on weights, strata and PSUs) as a practice dataset.
- **Deliverable:** A table showing how much weighting changes estimates, plus a note on who is lost to attrition.

### Week 11 — Outcomes and circularity

- **Learn:** The NHATS dementia technical paper (domains and algorithm); outcome definition in prediction research; Boolean logic revisited.
- **NHATS task:** Tabulate the dementia classification and its components. Compare how often "deterioration" occurs by clock-score drop versus memory-domain or orientation-domain drop. Write candidate outcomes that do not use the clock, for example: decline in recall or orientation, incident ADL dependence, entry to a nursing facility, death. Write them as a 1–2 page **analysis protocol v0.1** and commit it.
- **Deliverable:** Protocol v0.1 with outcome definitions and the reason each avoids circularity.

### Week 12 — Time-to-event and competing risks · **Gate A2**

- **Learn:** Kaplan–Meier, cumulative incidence, Cox models, competing risks (Austin, Lee and Fine, "Introduction to the analysis of survival data in the presence of competing risks", Circulation 2016); lifelines (Kaplan–Meier, `AalenJohansenFitter`).
- **NHATS task:** Build time-to-event data from the tracker (death, loss to follow-up) and an outcome from Week 11. Plot a Kaplan–Meier curve for death and an Aalen–Johansen cumulative incidence for an outcome with death as the competing event. Compare the naive 1 − KM estimate with the competing-risk estimate.
- **If NHATS dates are awkward:** Practise the API on scikit-survival's built-in datasets, or on NHANES linked mortality.
- **Gate:** One command rebuilds your Phase A2 tables and figures from raw files, with seeds and a config file. Tag the repo `v0.1`.

---

# Part A3 — Linear algebra, probability, statistics, classical ML (Weeks 13–18)

### Week 13 — Vectors and matrices

- **Learn:** 3Blue1Brown Essence of Linear Algebra Chapters 1–4; MML book Chapter 2 exercises; Khan Academy vectors and matrices. Hand drills: multiply a 2×3 matrix by a 3×1 vector and state the output shape.
- **NHATS task:** Build the feature matrix X (participants × features: age, sex, education, clock score, recall, orientation, ADL items, chronic-condition count) and outcome y. Standardise columns. Compute pairwise distances and find each participant's nearest neighbours.
- **Deliverable:** X and y with documented shapes, and a distance function you wrote yourself.

### Week 14 — Dot products, norms, projections, PCA

- **Learn:** 3Blue1Brown Chapters 5–9 (determinant, inverse, dot product, eigenvectors); StatQuest PCA.
- **NHATS task:** Run PCA on physical-performance, functional and cognitive variables. Draw a scree plot and a PC1 vs PC2 scatter coloured by dementia classification. Interpret the loadings clinically.
- **Deliverable:** A PCA notebook with a plain-language interpretation of each of the first three components.

### Week 15 — Probability and diagnostic reasoning

- **Learn:** StatQuest probability, conditional probability and Bayes; OpenIntro Statistics Chapter 3; expectation, variance, the Gaussian. Frame everything as sensitivity, specificity, PPV and NPV.
- **NHATS task:** Treat a low clock score as a screening test for impairment in the recall-based memory domain. (Do not use the dementia classification as the reference; it contains the clock.) Build 2×2 tables, compute sensitivity, specificity and predictive values by age band, and show how PPV changes with prevalence.
- **Deliverable:** A table and a paragraph explaining why the same test performs differently in different age groups.

### Week 16 — Inference and resampling

- **Learn:** OpenIntro Chapters 4–5; StatQuest confidence intervals; ISLP Chapter 5 (bootstrap, cross-validation).
- **NHATS task:** Estimate the mean annual change in clock score with bootstrap intervals. Compare a naive observation-level bootstrap with a participant-level (cluster) bootstrap, and explain the difference in interval width.
- **Deliverable:** A figure comparing the two intervals.

### Week 17 — Regression and classification

- **Learn:** ISLP Chapters 3–4; StatQuest linear and logistic regression; logarithms, exponentials, odds, sigmoid; scikit-learn and statsmodels. Understand ŷ = β₀ + Σ βⱼxⱼ symbol by symbol.
- **NHATS task:** Predict next-round recall or orientation score from baseline features with linear regression. Predict an incident outcome from Week 11 with logistic regression using clock-free baseline features, with participant-level splits (`GroupKFold`). Interpret odds ratios.
- **Deliverable:** Two fitted models, with coefficients explained clinically.

### Week 18 — ML evaluation, regularisation, trees · **Gate A3**

- **Learn:** ISLP Chapters 6 and 8 (ridge, lasso, trees, boosting); ROC-AUC, PR-AUC, calibration, Brier score; class imbalance.
- **NHATS task:** Build the **tabular benchmark** for future cognitive deterioration (logistic regression, lasso, gradient boosting) with participant-level cross-validation, confidence intervals and calibration plots. Then deliberately split at observation level and show how repeated-participant leakage inflates AUC.
- **Gate (unaided):** Explain training loss versus test performance, and why accuracy misleads when outcomes are imbalanced. This benchmark becomes **Model A** in the final comparison.

---

# Part A4 — Calculus, optimisation, backprop, PyTorch (Weeks 19–24)

These are the most important preparatory weeks for CS231n. Do not start Week 25 until the Week 24 gate is passed.

### Week 19 — Derivatives and gradients

- **Learn:** 3Blue1Brown Essence of Calculus Chapters 1–7; Khan Academy differential calculus; CS231n derivatives notes (linked from the CS231n schedule).
- **NHATS task:** Fit a simple linear model on NHATS (current recall → next-round recall) with the closed-form solution. Derive the gradient of the squared error by hand and verify it with finite differences in NumPy.
- **Deliverable:** A handwritten derivation and a notebook where finite-difference and analytic gradients agree.

### Week 20 — Chain rule, multivariable and matrix calculus

- **Learn:** 3Blue1Brown chain rule; *The Matrix Calculus You Need for Deep Learning*; the CS231n linear-backprop handout. Differentiate (wx − y)² with respect to w using the chain rule.
- **NHATS task:** Derive the gradient of the logistic loss with respect to the weight vector for a binary NHATS outcome. Implement logistic regression with gradient descent in NumPy and compare the coefficients with scikit-learn.
- **Deliverable:** A derivation and a NumPy implementation matching scikit-learn.

### Week 21 — Gradient descent and optimisers

- **Learn:** StatQuest gradient descent and stochastic gradient descent; learning rate, local minima, momentum, Adam; L1/L2 penalties as constraints.
- **NHATS task:** Implement full-batch GD, SGD and momentum for linear and logistic regression on NHATS features. Plot loss against step for several learning rates and compare with the ridge solution from Week 18.
- **Deliverable:** A figure showing loss curves for different learning rates, and a paragraph explaining what changing the learning rate does.

### Week 22 — Softmax, cross-entropy, computational graphs

- **Learn:** 3Blue1Brown Neural Networks Chapters 1–4; StatQuest softmax and cross-entropy; Karpathy micrograd, first half.
- **NHATS task:** Build a NumPy softmax classifier for the three-level NHATS dementia classification from **clock-free** features (age, function, comorbidity, memory and orientation items are the label's components, so exclude them too). Calculate gradients by hand for a few examples and check them numerically.
- **Deliverable:** A softmax classifier with hand-verified gradients.

### Week 23 — Backpropagation from scratch

- **Learn:** Karpathy micrograd, full video; CS231n backprop notes and the backprop review notebook.
- **NHATS task:** Build a small MLP in NumPy with manual backprop on a tabular NHATS outcome. Add a gradient check. Build a tiny autograd engine of your own following micrograd.
- **Deliverable:** An MLP trained by your own backprop, with a gradient check that passes.

### Week 24 — PyTorch · **Gate A4 (Stanford readiness test)**

- **Learn:** PyTorch "Learn the Basics" (tensors, autograd, `nn.Module`, `Dataset`/`DataLoader`, training loop).
- **NHATS task:** Reimplement the Week 23 tabular MLP in PyTorch with participant-level splits, mini-batches, early stopping and learning curves. Compare it honestly with the Week 18 gradient-boosting benchmark.
- **Gate (unaided, no notes):**
  1. Write a function returning successive differences of an array of annual scores.
  2. Multiply a 2×3 matrix by a 3×1 vector and state the output shape.
  3. Explain why y = wx + b makes a prediction.
  4. Explain why the sigmoid maps any number into (0, 1).
  5. Differentiate (wx − y)² with respect to w.
  6. Implement gradient descent and explain the effect of the learning rate.
  7. Explain training loss versus test performance, and what leakage is.
  8. Construct a tensor, run a forward pass, compute a loss, call `backward()`.
  9. Compute sensitivity, specificity and PPV from a 2×2 table.

---

# Part B — CS231n core (Weeks 25–30)

### Week 25 — Linear classifiers and k-NN (Lecture 2)

- **Learn:** Lecture 2, Image Classification with Linear Classifiers; CS231n notes on classification and linear classification; an image as a tensor.
- **NHATS task:** Build the clock-image pipeline: load images, resize to 64×64 grayscale, view samples across clock-score levels, create participant-level splits. Implement k-NN and a softmax classifier on raw pixels from scratch to predict clock-score category. Expect weak results and write down why.
- **If compute is limited:** Develop the code on MedMNIST (e.g., PathMNIST) and port it to NHATS.
- **Deliverable:** A reusable image dataset class and a raw-pixel baseline.

### Week 26 — Regularisation and optimisation (Lecture 3)

- **Learn:** Lecture 3; CS231n optimisation notes; "Why Momentum Really Works" (Distill); weight decay and learning-rate schedules.
- **NHATS task:** Compare SGD, momentum and Adam, and learning-rate schedules, on the clock-image linear or small MLP model. Plot convergence, training/validation loss and sensitivity to learning rate.
- **Deliverable:** An optimiser comparison with a short written recommendation.

### Week 27 — Neural networks and backprop (Lecture 4)

- **Learn:** Lecture 4; the backprop review notebook; initialisation and batch norm preview.
- **NHATS task:** Write a two-layer network with manual backward pass on flattened clock images. Run a gradient check against PyTorch autograd, then reproduce it in PyTorch.
- **Deliverable:** A manual and an autograd implementation with matching gradients.

### Week 28 — Convolutional networks (Lecture 5)

- **Learn:** Lecture 5; CS231n convolutional-networks notes; output-size arithmetic by hand (stride, padding, pooling); the reasons flips and large crops may destroy clinically meaningful features in a clock drawing.
- **NHATS task:** Train a small CNN on clock images to predict the clock score (ordinal or regression) with participant-level splits and clinically sensible augmentation (small shifts and scale; no flips). Report quadratic-weighted kappa against the NHATS score and analyse disagreements.
- **Deliverable:** A CNN, a kappa table and an error analysis.

### Week 29 — Architectures and transfer learning (Lecture 6)

- **Learn:** Lecture 6 (batch normalisation, AlexNet, VGG, ResNet); the original ResNet paper; PyTorch transfer-learning tutorial.
- **NHATS task:** Extract frozen embeddings from a pretrained ResNet and compare frozen features, a fine-tuned model and the Week 28 CNN. Save embeddings with participant ID, round and split.
- **If compute is limited:** Run the sweep on MedMNIST first.
- **Deliverable:** An embeddings file and a comparison table.

### Week 30 — Visualisation, evaluation and experiment hygiene · **Gate B**

- **Learn:** Lecture 9 sections on visualising and understanding networks; Grad-CAM and saliency; experiment tracking, checkpoints, config files.
- **NHATS task:** Apply Grad-CAM to the Week 28/29 models. Do they look at the hands, the numerals or the circle? Compare with the clock-scoring rubric. Check calibration and write a failure analysis.
- **Gate (unaided):** Explain convolution, pooling, receptive field and why transfer learning helps. Run your clock pipeline end to end from a config file.

---

# Part C — Sequences and modern representation learning (Weeks 31–40)

### Week 31 — RNNs, LSTMs and GRUs (Lecture 7)

- **Learn:** Lecture 7; Colah's "Understanding LSTM Networks"; Karpathy's "The Unreasonable Effectiveness of Recurrent Neural Networks"; backprop through time; vanishing gradients; padding and masking.
- **NHATS task:** Build sequences of tabular NHATS variables by participant. Train a vanilla RNN, a GRU and an LSTM to predict the next round's recall or orientation. Compare against persistence, last-observation-carried-forward, linear regression and gradient boosting.
- **Deliverable:** A table comparing recurrent models with simple baselines. Simple baselines often win.

### Week 32 — Irregular sampling and missing waves

- **Learn:** GRU-D (Che et al., "Recurrent Neural Networks for Multivariate Time Series with Missing Values"); time-aware LSTM ideas; informative censoring by death and attrition.
- **NHATS task:** Add elapsed-time-since-last-observation and missingness-mask inputs. Evaluate by prediction horizon. Handle death and attrition explicitly rather than dropping those participants.
- **If you want a second dataset for robustness:** PhysioNet/CinC 2012 for irregularly sampled clinical series.
- **Deliverable:** A time-aware recurrent model and an analysis of how much elapsed-time inputs help.

### Week 33 — Attention and transformers (Lecture 8, part 1)

- **Learn:** Lecture 8; "Attention Is All You Need"; Illustrated Transformer; Karpathy "Let's build GPT"; scaled dot-product attention, positional encoding, layer norm by hand.
- **NHATS task:** Build a small transformer encoder over each participant's sequence of rounds (tabular tokens, elapsed time as the positional signal) to predict the next round. Compare with the Week 32 GRU.
- **Deliverable:** A transformer on NHATS sequences and attention weights worked out by hand for one participant.

### Week 34 — Vision transformers (Lecture 8, part 2)

- **Learn:** ViT paper; patch embeddings; linear probing protocol.
- **NHATS task:** Patchify clock images and implement a tiny ViT. Extract pretrained ViT embeddings and compare them with ResNet embeddings using a linear probe for the current clock score. Check whether embedding-score agreement differs between early and later rounds, because of the NHATS coding-era issue described above.
- **Deliverable:** An embedding comparison and attention maps on example clocks.

### Week 35 — Self-supervised learning (Lecture 12)

- **Learn:** Lecture 12; Lilian Weng's self-supervised learning post; SimCLR and DINO papers; augmentation as an assumption about invariance.
- **NHATS task:** Train a small contrastive (SimCLR-style) or DINO-style model on clock images. Choose augmentations deliberately (what changes a clock's clinical meaning, and what does not). Evaluate with linear probes against the current clock score and against a Week 11 outcome.
- **If compute is limited:** Tune on MedMNIST first.
- **Deliverable:** A self-supervised encoder and a linear-probe table against the Week 29 baselines.

### Week 36 — JEPA (I-JEPA, V-JEPA)

- **Learn:** I-JEPA and V-JEPA papers and code; EMA target encoders, collapse and variance monitoring; predicting in representation space instead of pixel space.
- **NHATS task:** Implement a toy I-JEPA on clock images: context encoder, EMA target encoder, predictor, block masking. Measure how many patches are blank (clocks are sparse line drawings) and test masking strategies that avoid empty targets. Monitor embedding variance and rank to detect collapse.
- **Deliverable:** A working toy JEPA and a masking-strategy comparison.

### Week 37 — Autoencoders and VAEs (Lecture 13)

- **Learn:** Lecture 13; Kingma and Welling, [*An Introduction to Variational Autoencoders*](https://arxiv.org/abs/1906.02691); the ELBO blog from the CS231n schedule. Worked numerical examples of KL divergence between Gaussians, the ELBO and the reparameterisation trick.
- **NHATS task:** Train a convolutional autoencoder and a small VAE on clock drawings. Inspect reconstructions and latent traversals, and relate latent dimensions to the clock score. Read the [NHATS RF-VAE study](https://www.nature.com/articles/s41598-024-68481-w) and compare its approach with yours.
- **Deliverable:** A VAE, a latent-space analysis and a written ELBO derivation.

### Week 38 — Diffusion models (Lecture 14)

- **Learn:** Lecture 14; the DDPM paper (Ho et al. 2020); [Hugging Face Diffusion Course](https://huggingface.co/learn/diffusion-course/unit1/1); the Gaussian forward process, noise schedule, noise-prediction loss. Keep this at an introductory level.
- **NHATS task:** Train a tiny diffusion model on downsampled clock drawings, optionally conditioned on score. Sample images and denoise noised real ones. Check whether generated images memorise training clocks (a participant-privacy question).
- **If compute is limited:** Develop on MedMNIST at 28×28 or 64×64.
- **Deliverable:** A diffusion model, samples and a short note on what it can and cannot tell you about cognition.

### Week 39 — World models and latent dynamics (Lecture 17)

- **Learn:** Lecture 17, World Modeling (recording may be limited); Ha and Schmidhuber, "World Models"; V-JEPA; why predict a future embedding rather than future pixels.
- **NHATS task:** Using embeddings from Weeks 29, 35 and 36, train an action-free latent-dynamics model z(t) → z(t+1) with elapsed time as an input. Compare persistence, linear and MLP predictors.
- **If NHATS sequences are too short for this exploratory week:** Use the MIMIC-IV demo or PhysioNet 2012 for a clinical latent-dynamics toy.
- **Deliverable:** A latent-dynamics prototype and a table against persistence.

### Week 40 — Integration and pre-specified protocol · **Gate C**

- **Learn:** TRIPOD+AI reporting guidance and PROBAST+AI (see [tripod-statement.org](https://www.tripod-statement.org/)); events per variable; how to pre-register comparisons.
- **NHATS task:** Write **analysis protocol v1.0**: cohort definition, non-circular outcomes, models A–F (below), metrics (discrimination, calibration, decision curves), participant-level splits, a temporal-validation split (train on earlier rounds, test on later), subgroup analyses, and the sample size you actually have. Freeze it in Git before touching test data.
- **Gate (unaided):** Explain the difference between reconstructing pixels, generating images and predicting representations, and state which of the three the project does and why. This week is also catch-up time.

---

# Part D — Final NHATS research project (Weeks 41–48)

### Week 41 — Assemble the longitudinal image cohort

- **Tasks:** Link clock images to participant IDs and rounds. Count unique participants, images, consecutive pairs and sequences by length. Document gaps between images, deaths, missingness, non-participation, survey design, image quality (blank, rotated, partial) and coding era. Freeze the split file (participant-level, plus the temporal split).
- **Deliverable:** A validated cohort, a data manifest and a frozen split file.

### Week 42 — Baselines I: clinical model and pretrained embeddings

- **Tasks:** Finalise Model A (conventional clock score plus clinical covariates). Extract fixed embeddings from pretrained ResNet, ViT and DINO-family models. Fit linear probes for the current clock score, then for the future outcome. Report effects with confidence intervals.
- **Deliverable:** Baseline tables for Models A and B.

### Week 43 — Baselines II: autoencoder embeddings and representation diagnostics

- **Tasks:** Create VAE/autoencoder embeddings. For every representation, measure within-participant stability across rounds, sensitivity to scan or photo artefacts, correlation with the clock score, and effective dimensionality. Check for coding-era drift.
- **Deliverable:** An embeddings file for every representation and a diagnostics table.

### Week 44 — NHATS-adapted image JEPA, part 1

- **Tasks:** Fine-tune a small pretrained encoder with the I-JEPA objective on training-participant clocks only, with no labels. Use the masking strategy from Week 36. Monitor optimisation, embedding variance and rank. Keep hyperparameter tuning inside the training participants.
- **Deliverable:** A trained JEPA encoder and collapse diagnostics.

### Week 45 — NHATS-adapted image JEPA, part 2

- **Tasks:** Run ablations on masking, EMA schedule and encoder size. Compare frozen versus adapted encoders on the current clock score and on contemporaneous recall and orientation, which are not defined by the clock. Select the encoder using validation participants only.
- **Deliverable:** An ablation table and one selected encoder.

### Week 46 — Temporal latent prediction (Models C and D)

- **Tasks:** Use previous image embeddings and elapsed time to predict the next embedding. Compare persistence, linear, MLP and GRU predictors with the pretrained ViT (Model C) and with the adapted JEPA encoder (Model D). Use only rounds up to t to predict t+1.
- **Deliverable:** A working temporal predictor and latent-loss tables, with the caveat that low latent loss does not demonstrate clinical value.

### Week 47 — Clinical outcomes and multimorbidity (Models B–F)

- **Tasks:** Predict the pre-specified outcomes from the protocol (for example, decline in recall or orientation, incident ADL dependence, nursing-facility entry, death), with competing-risk analysis where relevant. Add NHATS morbidity, frailty and functional variables (Model E). Optionally link NSOC variables if the cohort permits. Report discrimination, calibration and decision curves, with bootstrap confidence intervals for differences between models on the same held-out participants.
- **Deliverable:** Main evaluation and ablation tables.

### Week 48 — Validation and manuscript

- **Tasks:** Run subgroup analyses (age, sex, race and ethnicity, education), attrition sensitivity, outcome-definition sensitivity, the temporal-validation split, and weighted versus unweighted results. Tidy the repo so everything rebuilds with one command. Draft Methods, Results and Limitations following TRIPOD+AI.
- **Deliverable:** A complete research prototype and a short manuscript draft. Report the result whichever direction it goes.

---

## Final experimental comparison

| Model | Image representation | Temporal mechanism |
| --- | --- | --- |
| A | Conventional clock score | Regression / clinical baseline (Week 18 benchmark) |
| B | Pretrained ViT (also ResNet, DINO-family) | Simple future-outcome predictor |
| C | Pretrained ViT | Future-latent MLP or GRU |
| D | NHATS-adapted JEPA | Same future-latent predictor |
| E | Best-performing representation | Future-latent predictor plus NHATS clinical features |
| F (optional) | Best-performing representation | Hierarchical temporal predictor |

A stronger model must beat sensible baselines on the same held-out participants, using outcomes that do not depend on the clock. Low latent loss alone does not count.

## Milestones

| By week | You can independently… |
| --- | --- |
| 6 | Write, test and run small Python programs; load and describe one NHATS round |
| 12 | Merge NHATS rounds, handle missing data and weights, and fit competing-risk curves |
| 18 | Build and honestly evaluate a clinical prediction model, and show leakage |
| 24 | Derive a gradient, implement backprop, and train a PyTorch model |
| 30 | Train a CNN on clock images, extract pretrained embeddings and explain what the model looks at |
| 40 | Explain and implement RNN/LSTM, ViT, contrastive learning, JEPA, VAE and diffusion, and write a frozen protocol |
| 48 | Run a reproducible set of clinically meaningful comparisons and report them |

## Items to verify before relying on them

- NHATS variable names, weight names and which weights suit longitudinal analyses (user guide and technical papers).
- Which clock-image rounds were hand-coded versus model-coded.
- The file formats your NHATS release provides (SAS, Stata, CSV or R).
- CS231n lecture titles in the 2025 videos against the 2026 syllabus.
- Package availability and current APIs (`lifelines`, `scikit-survival`, `samplics`, `medmnist`).
