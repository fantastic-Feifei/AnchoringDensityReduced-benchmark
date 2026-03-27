# MTS-benchmark

This repository provides a **controlled temporal reasoning dataset** for studying how large language models (LLMs) perform under **mixed temporal information conditions**.

Unlike existing benchmarks that rely on fully explicit timestamps, this dataset **systematically manipulates temporal anchoring density** while preserving identical event structures.  
This enables controlled evaluation of how temporal information availability affects chronological reasoning.

---

## 🔍 Key Features

- **Controlled anchoring density**  
  Temporal expressions are systematically reduced or rewritten to simulate realistic, heterogeneous time conditions.

- **Paired data design**  
  Each narrative has:
  - an **Absolute-Time (AT)** version (fully anchored)
  - a **Mixed-Time (MT)** version (partially de-anchored)  
  → with **identical events and gold chronological order**

- **Sentence-level event ordering task**  
  Each sentence corresponds to one event, and models must recover the correct temporal sequence.

- **Reproducible rewriting rules**  
  Time expression transformations follow deterministic rules with optional GPT-based augmentation.

---

## 📂 Dataset Files

The repository contains three main files:

- `MT_shuffled.csv`  
  Mixed-time setting. A subset of absolute time expressions is rewritten into relative or contextual expressions.

- `AT_shuffled_cleaned.csv`  
  Absolute-time setting. All events retain explicit timestamps.

- `conversion_table.csv`  
  A reference table mapping absolute time expressions to their rewritten forms.

Each dataset consists of biographical passages derived from Wikipedia, annotated with **gold-standard chronological order**.

---

## 🧠 Dataset Design

### Event Structure

Each passage is decomposed into a sequence of event sentences:

- One sentence = one event  
- A gold chronological order is provided  
- Input is a **shuffled sequence**, requiring reconstruction

---

### Anchoring Density (D)

Anchoring density is defined as:

> the proportion of events associated with explicit temporal expressions

- **AT setting** → high density (fully anchored)  
- **MT setting** → reduced density (partial rewriting)

This allows **controlled analysis of temporal constraint availability**.

---

### Why This Dataset Matters

This dataset enables:

- **Isolation of temporal information effects**  
  Changes in performance can be attributed to temporal structure, not content differences

- **Controlled robustness evaluation**  
  Models are tested under progressively weaker temporal constraints

- **Causal-style analysis**  
  By keeping event content fixed, we directly measure how anchoring affects reasoning stability

---

## 📑 Column Descriptions

### Common Columns (`MT_shuffled.csv` & `AT_shuffled_cleaned.csv`)

| Column Name | Description |
|------------|------------|
| `Wikidata ID` | Unique identifier from Wikidata |
| `Name` | Person name |
| `Professions` | Occupation(s) |
| `Wikipedia Title` | Source page |
| `pageContent` | Original extracted passage |
| `clean text` | Preprocessed and segmented text |
| `gold answer` | Correct chronological order (e.g., `0 1 2 3`) |
| `event_count` | Number of events |

---

### Additional Columns in `MT_shuffled.csv`

| Column Name | Description |
|------------|------------|
| `Gpt Modified Context` | Passage with rewritten temporal expressions |
| `Replacement Information` | Details of applied rewrites |
| `Shuffled Gpt Modified Context` | Shuffled input for model evaluation |
| `Shuffle Mapping` | Mapping to original order |

---

### Additional Columns in `AT_shuffled_cleaned.csv`

| Column Name | Description |
|------------|------------|
| `Shuffled gold answer` | Shuffled version of the original passage |
| `Shuffle Mapping gold answer` | Mapping for evaluation |

---

### Conversion Table

| Column Name | Description |
|------------|------------|
| `Original Time` | Original absolute expression |
| `Rewritten Time` | Rewritten version |

---

## 🔧 Time Conversion Rules

To ensure reproducibility, we apply structured rewriting strategies:

### 1. Granularity Preservation

Maintain temporal resolution:

```text
1970 → early 1970s  
Oct. 2, 2003 → early October 2003
