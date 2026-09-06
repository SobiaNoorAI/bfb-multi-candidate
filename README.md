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
Quick Start
from bfb_generator import BFBCandidateGenerator

# Define your pattern
copy_numbers = [2, 4, 6, 4, 2]
foldbacks = [False, True, False, True, False]

# Generate candidates
generator = BFBCandidateGenerator(copy_numbers, foldbacks)
results = generator.run(max_cycles=3)

# View top candidates
generator.print_summary(top_n=5)
