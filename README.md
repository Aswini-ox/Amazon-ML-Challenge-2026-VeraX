# Amazon ML Challenge 2026 – Business Entity Resolution

## Team VeraX

A machine learning pipeline developed for the **Amazon ML Challenge 2026 – Business Entity Resolution** problem.

The objective of this project is to identify records that refer to the same real-world business entity across different data sources, despite variations in names, addresses, phone numbers, websites, locations, transliterations, formatting, and other noisy attributes.

Our solution follows an end-to-end entity-resolution pipeline consisting of **data preparation, normalization, blocking/candidate generation, feature construction, similarity scoring, prediction, and final result generation**.

---

## 👥 Team

| Role   | Name          |
| ------ | ------------- |
| Team   | **VeraX**     |
| Member | Aisharya .B   |
| Member | Charumathi .S |
| Member | Aswini.R I    |

---

# 🎯 Problem Overview

Business information collected from different sources rarely follows a consistent format.

The same business may appear with:

* Different spellings
* Abbreviations
* Different address formats
* Missing attributes
* Different phone-number formats
* Different website representations
* Transliteration differences
* Noisy or irrelevant tokens
* Country- or locale-specific variations

For example:

```text
Source A:
ABC Technologies Pvt. Ltd.
Chennai, Tamil Nadu

Source B:
A.B.C Technologies Private Limited
Chennai TN
```

Although the records look different, they may represent the same business entity.

The goal of **Business Entity Resolution** is to determine whether records refer to the same underlying entity.

---

# 💡 Solution Approach

Our pipeline processes business records through multiple stages:

```text
Raw Business Data
        │
        ▼
Data Preparation
        │
        ▼
Normalization
        │
        ├── Name normalization
        ├── Address normalization
        ├── Phone normalization
        ├── Website normalization
        └── Locale / language handling
        │
        ▼
Candidate Generation / Blocking
        │
        ▼
Candidate Pair Construction
        │
        ▼
Feature Engineering
        │
        ├── String similarity
        ├── Token similarity
        ├── Field-level comparisons
        ├── Address similarity
        ├── Phone similarity
        └── Other matching signals
        │
        ▼
Model Training / Scoring
        │
        ▼
Prediction
        │
        ▼
Final Entity Matches
        │
        ▼
matching_results.tsv
```

---

# 🏗️ Repository Structure

The repository follows the required structure for the Amazon ML Challenge submission.

```text
Amazon-ML-Challenge-2026-VeraX/
│
├── output/
│   │
│   ├── matching_results.tsv
│   └── candidate_pairs.tsv
│
├── code/
│   │
│   └── business_entity_resolution/
│       │
│       ├── src/
│       │   │
│       │   ├── apply_indic_dict.py
│       │   ├── build_features.py
│       │   ├── build_normalized.py
│       │   ├── candidates.py
│       │   ├── config.py
│       │   ├── features.py
│       │   ├── indic_dictionary.py
│       │   ├── metrics.py
│       │   ├── normalization.py
│       │   ├── predict.py
│       │   ├── prepare_data.py
│       │   ├── retrieval.py
│       │   ├── run_all.py
│       │   ├── train.py
│       │   │
│       │   ├── v2/
│       │   │   ├── configs_v2.py
│       │   │   ├── confirm_v2.py
│       │   │   ├── eval_v2.py
│       │   │   ├── featx.py
│       │   │   ├── make_qf.py
│       │   │   ├── prevalence.py
│       │   │   ├── predict_v2.py
│       │   │   ├── run_v2.py
│       │   │   ├── runlib.py
│       │   │   ├── scorer.py
│       │   │   ├── stack_v2.py
│       │   │   ├── stress_build.py
│       │   │   ├── stress_eval.py
│       │   │   ├── train_v2.py
│       │   │   └── v2_prepare.py
│       │   │
│       │   └── v4/
│       │       ├── assemble.py
│       │       ├── compare_fr.py
│       │       ├── diag_shift.py
│       │       ├── fr_locale.py
│       │       ├── fr_pseudo.py
│       │       ├── mine_noise_tokens.py
│       │       ├── tx.py
│       │       ├── tx_errors.py
│       │       ├── tx_pseudo.py
│       │       └── v4_prepare.py
│       │
│       ├── README.md
│       └── requirements.txt
│
└── Documentation_template.md
```

---

# 📂 Output Files

## `output/matching_results.tsv`

This file contains the final entity-resolution predictions produced by the pipeline.

It is the final matching output submitted to the challenge leaderboard.

---

## `output/candidate_pairs.tsv`

This file contains the candidate pairs produced during the blocking/candidate-generation stage.

Candidate generation reduces the number of possible comparisons by selecting potentially matching records before detailed similarity evaluation.

---

# 🔎 Candidate Generation / Blocking

Comparing every business record with every other record can become computationally expensive.

Therefore, the solution uses a **blocking strategy** to generate a manageable candidate set.

Conceptually:

```text
All Possible Record Pairs
          │
          │ Blocking
          ▼
Potential Candidate Pairs
          │
          ▼
Detailed Similarity Evaluation
          │
          ▼
Final Matches
```

Blocking can use normalized attributes and retrieval signals to identify records that are sufficiently similar to warrant further comparison.

This allows the matching stage to operate on a substantially smaller search space.

---

# 🧹 Data Normalization

Business data can contain significant formatting variation.

The normalization stage is designed to reduce irrelevant differences before comparison.

Examples include:

```text
"ABC Technologies Pvt. Ltd."
            ↓
"abc technologies"
```

and normalization of:

* Case
* Punctuation
* Whitespace
* Common business suffixes
* Phone-number formatting
* Website formatting
* Address representation
* Locale-specific variations
* Indic-language/transliterated information

The repository includes dedicated normalization and Indic-language processing modules.

---

# 🧩 Feature Engineering

The matching system generates comparison features between candidate records.

The feature layer can incorporate signals such as:

* Business-name similarity
* Token similarity
* Character-level similarity
* Address similarity
* Phone similarity
* Website similarity
* Exact/near-exact field agreement
* Normalized-field comparison
* Locale-specific signals
* Retrieval-based similarity

These features provide the downstream scoring/modeling stages with information about how closely two records correspond.

---

# 🤖 Model / Scoring Pipeline

After candidate generation and feature construction, candidate pairs are evaluated using the project's scoring and prediction components.

The general flow is:

```text
Candidate Pair
      │
      ▼
Feature Extraction
      │
      ▼
Similarity Signals
      │
      ▼
Scoring / Model
      │
      ▼
Match Decision
```

The repository contains training, evaluation, scoring, and prediction modules, including the `v2` pipeline components.

---

# 🌐 Locale and Indic-Language Handling

Business information may contain multilingual or transliterated data.

The project therefore includes dedicated components for handling Indic-language information and normalization.

Relevant modules include:

```text
src/indic_dictionary.py
src/apply_indic_dict.py
```

Additional locale-specific processing is implemented under:

```text
src/v4/
```

This helps reduce mismatches caused by differences in language, transliteration, and regional formatting.

---

# 🧪 Pipeline Components

## Data Preparation

```text
prepare_data.py
```

Responsible for preparing input data for subsequent processing stages.

---

## Normalization

```text
normalization.py
build_normalized.py
```

Handles transformation of raw business attributes into normalized representations suitable for comparison.

---

## Candidate Generation

```text
candidates.py
retrieval.py
```

Responsible for retrieving and generating potential matching candidate pairs.

---

## Feature Construction

```text
features.py
build_features.py
```

Creates comparison features from candidate records.

---

## Training

```text
train.py
```

Contains the training-stage implementation used by the pipeline.

---

## Prediction

```text
predict.py
```

Generates matching predictions from processed candidate records/features.

---

## Metrics

```text
metrics.py
```

Provides evaluation-related functionality for assessing matching performance.

---

# 🧪 V2 Pipeline

The repository also contains an extended V2 implementation under:

```text
src/v2/
```

Important components include:

```text
configs_v2.py
featx.py
train_v2.py
predict_v2.py
scorer.py
stack_v2.py
eval_v2.py
run_v2.py
```

These modules provide additional feature, training, scoring, evaluation, and execution functionality.

---

# 🧪 V4 Pipeline

Additional processing and experimentation components are maintained under:

```text
src/v4/
```

The V4 directory contains modules for:

* Locale processing
* Pseudo-label related processing
* Noise-token analysis
* Transformation
* Error handling
* Comparison
* Assembly
* Diagnostic analysis

Examples:

```text
fr_locale.py
fr_pseudo.py
mine_noise_tokens.py
tx.py
tx_errors.py
tx_pseudo.py
compare_fr.py
assemble.py
```

---

# 🛠️ Technology Stack

The project is implemented primarily in **Python**.

Core technologies and libraries include:

* Python
* Pandas
* NumPy
* Scikit-learn
* Machine-learning utilities
* String and text similarity processing
* Data preprocessing utilities
* TSV-based input/output processing

Exact dependency versions are provided in:

```text
code/business_entity_resolution/requirements.txt
```

---

# ⚙️ Installation

Clone the repository:

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd Amazon-ML-Challenge-2026-VeraX
```

Create a virtual environment:

### Windows

```bash
python -m venv .venv
.venv\Scripts\activate
```

### Linux / macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r code/business_entity_resolution/requirements.txt
```

---

# ▶️ Running the Pipeline

The main pipeline entry point is:

```text
code/business_entity_resolution/src/run_all.py
```

Run it using:

```bash
python code/business_entity_resolution/src/run_all.py
```

For individual stages, the relevant modules are available inside:

```text
code/business_entity_resolution/src/
```

The detailed execution instructions are also provided in:

```text
code/business_entity_resolution/README.md
```

---

# 🔄 End-to-End Reproduction

The intended reproduction flow is:

```text
1. Prepare input data
        ↓
2. Normalize business attributes
        ↓
3. Generate candidate pairs
        ↓
4. Build comparison features
        ↓
5. Train / load scoring model
        ↓
6. Generate predictions
        ↓
7. Produce matching_results.tsv
        ↓
8. Produce candidate_pairs.tsv
```

The pipeline source code and dependency file are contained within:

```text
code/business_entity_resolution/
```

---

# 📊 Final Submission Artifacts

The submission package contains:

```text
output/
├── matching_results.tsv
└── candidate_pairs.tsv
```

These correspond to:

### Final Matches

```text
matching_results.tsv
```

The final business entity matching predictions.

### Blocking Candidates

```text
candidate_pairs.tsv
```

The candidate set generated during the blocking stage.

---

# 📖 Methodology Documentation

The completed methodology document is available at:

```text
Documentation_template.md
```

It describes the approach, methodology, implementation, and relevant details of the submitted solution.

---

# 🔐 Security / Credentials

No API keys, passwords, access tokens, or other confidential credentials should be committed to this repository.

Before publishing the repository, verify that no sensitive information is present in:

* Source code
* Configuration files
* Environment files
* Logs
* Notebooks
* Output files

---

# 📌 Reproducibility

The repository is organized so that the core solution code, dependencies, outputs, and methodology documentation are maintained together.

The primary reproducibility resources are:

```text
code/business_entity_resolution/README.md
code/business_entity_resolution/requirements.txt
code/business_entity_resolution/src/
```

---

# 🏆 Amazon ML Challenge 2026

**Challenge:** Amazon ML Challenge 2026

**Problem:** Business Entity Resolution

**Team:** VeraX

**Team Members:**

* Aisharya .B
* Charumathi .S
* Aswini.R I

---

## ⭐ Project Summary

This project addresses the challenge of resolving business identities across noisy and heterogeneous datasets.

The solution combines:

```text
Data Preparation
      +
Normalization
      +
Blocking / Candidate Generation
      +
Feature Engineering
      +
Similarity / Scoring
      +
Prediction
      =
Business Entity Resolution
```

The resulting pipeline produces a final set of entity matches together with the candidate pairs used during the blocking stage.

---

## 📄 License

This repository is created for the **Amazon ML Challenge 2026** submission.

Please ensure that all third-party libraries, datasets, models, and code used in the project comply with their respective licenses and the challenge's rules.
