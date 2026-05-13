# Anchoring Density Control Benchmark Dataset

This repository provides a **controlled temporal reasoning benchmark** for analyzing how large language models (LLMs) perform under varying levels of **temporal anchoring density**.

Unlike existing benchmarks that rely on densely grounded explicit timestamps, MTS-benchmark treats **anchoring density as a controllable structural variable**. By systematically reducing explicit temporal anchors while preserving identical events and gold chronological orderings, the benchmark enables controlled analysis of how temporal grounding affects chronological reconstruction behavior.

The benchmark consists of paired narrative settings:

- **Explicit-Time (ET)** → fully anchored narratives
- **Density-Reduced (DR)** → partially de-anchored hybrid-time narratives

This paired design isolates the effect of temporal information availability from semantic variation.

---

# 🔍 Benchmark Design Principles

- **Anchoring density control**  
  The benchmark systematically manipulates the density of explicit temporal anchors while preserving event semantics and chronological structure.

- **Paired ET/DR evaluation setting**  
  Each narrative is provided in both:
  - an Explicit-Time (ET) version
  - a Density-Reduced (DR) version

  allowing direct comparison under controlled temporal grounding conditions.

- **Sentence-level event ordering task**  
  Each sentence corresponds to one event, and models must recover the correct chronological sequence.

- **Structured temporal rewriting**  
  Temporal expression transformations follow controlled rewriting strategies with optional GPT-based augmentation.

---

# 📊 Controlled Variables

The benchmark is designed around controlled manipulation of temporal information structure:

| Variable | Description |
|---|---|
| Anchoring Density (D) | Ratio of explicit temporal anchors among all temporal expressions |
| Anchoring Specificity (G) | Temporal granularity (year / month / day) |
| Structural Load (L) | Number of events within a passage |

This design enables systematic robustness analysis under varying temporal grounding conditions.

---

# 📂 Dataset Files

The repository contains three main files:

- `DR_shuffled.csv`  
  Density-Reduced setting. A subset of explicit temporal expressions is rewritten into relative or contextual temporal expressions.

- `ET_shuffled_cleaned.csv`  
  Explicit-Time setting. All events retain explicit calendar-based timestamps.

- `conversion_table.csv`  
  A reference table mapping explicit temporal expressions to rewritten hybrid-time expressions.

Each dataset consists of biographical passages derived from Wikipedia, annotated with gold-standard chronological orderings.

---

# 🧠 Dataset Design

## Event Structure

Each passage is decomposed into a sequence of event sentences:

- One sentence = one event
- A gold chronological order is provided
- Input is presented as a shuffled sequence requiring chronological reconstruction

---

## Anchoring Density (D)

Anchoring density is defined as:

> the proportion of explicit temporal anchors among all temporal expressions

- **ET setting** → high anchoring density (fully anchored)
- **DR setting** → reduced anchoring density (partial rewriting)

This enables controlled analysis of how temporal constraint availability affects chronological reasoning behavior.

---

## Why This Benchmark Matters

This benchmark enables:

- **Isolation of temporal information effects**  
  Performance differences can be attributed to temporal structure rather than semantic content variation.

- **Controlled robustness evaluation**  
  Models are evaluated under progressively weaker temporal grounding conditions.

- **Structural analysis of temporal reasoning**  
  By preserving event semantics and gold chronological orderings, the benchmark directly measures how temporal anchors influence reasoning stability.

---

# 📑 Column Descriptions

## Common Columns (`DR_shuffled.csv` & `ET_shuffled_cleaned.csv`)

| Column Name | Description |
|---|---|
| `Wikidata ID` | Unique identifier from Wikidata |
| `Name` | Person name |
| `Professions` | Occupation(s) |
| `Wikipedia Title` | Source page |
| `pageContent` | Original extracted passage |
| `clean text` | Preprocessed and segmented text |
| `gold answer` | Correct chronological order (e.g., `0 1 2 3`) |
| `event_count` | Number of events |

---

## Additional Columns in `DR_shuffled.csv`

| Column Name | Description |
|---|---|
| `GPT Modified Context` | Passage with rewritten temporal expressions |
| `Replacement Information` | Details of applied temporal rewrites |
| `Shuffled GPT Modified Context` | Shuffled input for model evaluation |
| `Shuffle Mapping` | Mapping to original order |

---

## Additional Columns in `ET_shuffled_cleaned.csv`

| Column Name | Description |
|---|---|
| `Shuffled gold answer` | Shuffled version of the original passage |
| `Shuffle Mapping gold answer` | Mapping for evaluation |

---

## Conversion Table

| Column Name | Description |
|---|---|
| `Original Time` | Original explicit temporal expression |
| `Rewritten Time` | Rewritten hybrid-time expression |

---

# 🔧 Temporal Rewriting Rules

To ensure reproducibility, we apply structured temporal rewriting strategies.

## 1. Granularity Preservation

Maintain temporal resolution consistency:

```text
1970 → early 1970s
Oct. 2, 2003 → early October 2003
