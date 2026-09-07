# BFB Candidate Generator

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Colab](https://img.shields.io/badge/Colab-Open-orange.svg)](https://colab.research.google.com/)

A Python prototype for generating multiple BFB (Breakage-Fusion-Bridge) sequence candidates from copy number patterns.

## 📖 Background

The **BFBArchitect** tool uses an Integer Linear Program (ILP) to find a single consensus BFB sequence that best fits observed genomic data. However, biological data is inherently ambiguous—**multiple sequences can produce the exact same copy number pattern**.

This project explores generating **ALL possible BFB sequences** and ranking them by likelihood, providing geneticists with a confidence distribution rather than a single deterministic guess.

### The Problem
Input Pattern: [2, 4, 6, 4, 2]
Foldbacks: [False, True, False, True, False]

Possible BFB sequences that explain this pattern:

[A, B, C] → 100% match

[A, C, B] → 100% match

[B, A, C] → 100% match

Which one is correct? Current tools output only one. This project outputs **ALL**.

## 🎯 Key Features

- ✅ **Generate all possible BFB sequences** up to N cycles
- ✅ **Score and rank** sequences by how well they match observed data
- ✅ **Identify multiple valid explanations** for the same pattern
- ✅ **Export results** to text file for further analysis
- ✅ **Lightweight** - pure Python, no dependencies

## 📋 Table of Contents

- [Installation](#installation)
- [Quick Start](#quick-start)
- [Usage Examples](#usage-examples)
- [How It Works](#how-it-works)
- [Example Output](#example-output)
- [File Structure](#file-structure)
- [Technical Details](#technical-details)
- [References](#references)
- [Author](#author)

## 🚀 Installation

```bash
# Clone the repository
git clone https://github.com/SobiaNoorAI/bfb-candidate-generator.git
cd bfb-candidate-generator

# No dependencies needed! Just Python 3.8+
python bfb_generator.py
```
## Quick Start
```bash
from bfb_generator import BFBCandidateGenerator

# Define your pattern
copy_numbers = [2, 4, 6, 4, 2]
foldbacks = [False, True, False, True, False]

# Generate candidates
generator = BFBCandidateGenerator(copy_numbers, foldbacks)
results = generator.run(max_cycles=3)

# View top candidates
generator.print_summary(top_n=5)
```
## Usage Examples
Example 1: Basic Pattern
```bash
# Simple pattern with multiple possible sequences
copy_numbers = [2, 4, 6, 4, 2]
foldbacks = [False, True, False, True, False]

generator = BFBCandidateGenerator(copy_numbers, foldbacks)
generator.run(max_cycles=3)
generator.print_summary(top_n=5)
```
Example 2: Complex Pattern
```bash
# More complex pattern
copy_numbers = [1, 2, 4, 8, 4, 2, 1]
foldbacks = [False, False, True, False, True, False, False]

generator = BFBCandidateGenerator(copy_numbers, foldbacks)
results = generator.run(max_cycles=4)
generator.print_summary(top_n=5)

# Export results to file
generator.export_sequences("bfb_candidates.txt")
```
Example 3: Find All Perfect Matches
```bash
# Find all sequences with 100% match
copy_numbers = [2, 4, 6, 4, 2]
foldbacks = [False, True, False, True, False]

generator = BFBCandidateGenerator(copy_numbers, foldbacks)
results = generator.run(max_cycles=4)

perfect_matches = [c for c in results if c['score'] >= 0.99]
print(f"Found {len(perfect_matches)} perfect matches:")
for match in perfect_matches:
    print(f"  {' -> '.join(match['sequence'])}")
```

🔬 How It Works
BFB Cycle Types
The simulator models three basic BFB cycle types:

|Cycle	| Transformation |	Example|
|-------|----------------|---------|
|**A**	| Simple duplication |	[1,2] → [1,2,1,2]|
|**B**	| Duplication with break	| [1,2,3,4] → [1,2,3,4,3,4]|
|**C**	|Reverse duplication |	[1,2,3] → [1,2,3,3,2,1]|

## Scoring Algorithm
1. Generate all possible sequences up to N cycles

2. Simulate what each sequence would produce

3. Compare predicted vs observed copy numbers

4. Score = (matches) / (total positions compared)

5. Rank candidates by score (highest first)

## Visual Pipeline

```bash
Input Pattern
    ↓
Generate All Sequences
    ↓
Simulate Each Sequence
    ↓
Score vs. Observed Pattern
    ↓
Rank by Score (Highest First)
    ↓
Output: Ranked Candidates
```
