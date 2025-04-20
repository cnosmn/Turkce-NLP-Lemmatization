# Turkish Text Lemmatization

[![Python](https://img.shields.io/badge/Python-3.9-blue.svg)](https://www.python.org/downloads/release/python-390/)
[![NLTK](https://img.shields.io/badge/NLTK-3.6-green.svg)](https://www.nltk.org/)
[![Zeyrek](https://img.shields.io/badge/Zeyrek-0.1.5-yellow.svg)](https://github.com/obulat/zeyrek)
[![FuzzyWuzzy](https://img.shields.io/badge/FuzzyWuzzy-0.18-orange.svg)](https://github.com/seatgeek/fuzzywuzzy)

## Overview

This project implements Turkish language text lemmatization using the Zeyrek morphological analyzer. It demonstrates how to process Turkish text by tokenizing sentences, removing punctuation, extracting word roots (lemmas), filtering stopwords, and performing similarity analysis between texts. The system can be applied to various natural language processing tasks including semantic matching and legal text analysis.

## Table of Contents

- [Turkish Text Lemmatization](#turkish-text-lemmatization)
  - [Overview](#overview)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Features](#features)
  - [Turkish Language Processing](#turkish-language-processing)
  - [Methodology](#methodology)
  - [Key Components](#key-components)
    - [1. Morphological Analysis](#1-morphological-analysis)
    - [2. Lemmatization](#2-lemmatization)
    - [3. Advanced Text Processing](#3-advanced-text-processing)
  - [Examples](#examples)
    - [Basic Lemmatization](#basic-lemmatization)
    - [Sentence Processing](#sentence-processing)
  - [Legal Text Application](#legal-text-application)
  - [Installation](#installation)
  - [Usage](#usage)
    - [Basic Lemmatization](#basic-lemmatization-1)
    - [Legal Text Matching](#legal-text-matching)
  - [Dependencies](#dependencies)

## Introduction

Processing morphologically rich languages like Turkish presents unique challenges in natural language processing. Turkish is an agglutinative language where words can have many suffixes, making standard stemming approaches less effective. This project uses Zeyrek, a Python implementation of the Zemberek Turkish morphological analyzer, to properly lemmatize Turkish text to its root forms.

## Features

- Turkish word morphological analysis
- Root word (lemma) extraction
- Multiple lemma candidates handling
- Sentence tokenization and word tokenization
- Punctuation removal
- Stopword filtering
- Text similarity analysis with FuzzyWuzzy
- Legal text matching implementation

## Turkish Language Processing

Turkish presents several distinct challenges for NLP:

1. **Agglutination**: Turkish words can have multiple suffixes attached to a root, creating very long words with complex meanings
2. **Vowel Harmony**: Suffixes must harmonize with the vowels in the root word
3. **Rich Morphology**: A single Turkish word can express what might require an entire phrase in English

The Zeyrek analyzer addresses these challenges by implementing a morphological analysis approach specifically designed for Turkish.

## Methodology

The process follows these key steps:

1. **Tokenization**: 
   - Breaking sentences into individual words using NLTK's word_tokenize

2. **Punctuation Removal**:
   - Removing standard punctuation marks to clean the text

3. **Lemmatization**:
   - Using Zeyrek to analyze each token and extract its root form (lemma)
   - Handling words with multiple possible lemmas

4. **Stopword Removal**:
   - Filtering out common Turkish words that don't contribute significant meaning

5. **Similarity Analysis** (in the legal application):
   - Comparing processed text against a corpus of legal texts
   - Finding the best matching legal article based on similarity scores

## Key Components

### 1. Morphological Analysis

The system provides detailed morphological analysis of Turkish words:

```python
for parse in analyzer.analyze('benim')[0]:
    print(parse)
```

Output shows different possible interpretations:
- Noun form with possessive suffix
- Pronoun form with genitive case
- Verb derivation forms

### 2. Lemmatization

The code demonstrates extracting lemmas (root forms) from Turkish words:

```python
analyzer.lemmatize('benim')  # Returns [('benim', ['ben'])]
```

### 3. Advanced Text Processing

The project includes functions for processing complete texts:

- `lemma()`: Processes a single Turkish sentence through the complete pipeline
- `kanun_lemma()`: Processes legal texts and builds a dictionary for matching

## Examples

### Basic Lemmatization

```python
analyzer.lemmatize('seviyorum')  # Returns [('seviyorum', ['sevmek'])]
```

### Sentence Processing

Sample input: "ben oyun oynamayı çok seviyorum"

Lemmatized output:
```
[('ben', ['ben']), ('oyun', ['oymak', 'oy', 'oyun']), ('oynamayı', ['oynamak']), ('çok', ['çok']), ('seviyorum', ['sevmek'])]
```

## Legal Text Application

The project includes a specialized application for legal text matching. Given an input query, it:

1. Processes a collection of legal articles using the same lemmatization pipeline
2. Compares the processed query against the processed legal texts
3. Returns the most similar legal article using fuzzy matching

Example:
```python
# Input
lemma('kasten birinin vücuduna zarar vermek.')

# Output
# ... (processing steps)
# en yüksek benzerlikli madde: madde 86
```

This demonstrates the system's ability to match a natural language query to the most relevant legal article.

## Installation

1. Clone this repository
```bash
git clone https://github.com/cnosmn/Turkce-NLP-Lemmatization.git
cd Turkce-NLP-Lemmatization
```

2. Install required packages
```bash
pip install nltk zeyrek fuzzywuzzy python-Levenshtein
```

3. Download necessary NLTK data
```python
import nltk
nltk.download('punkt')
nltk.download('stopwords')
```

## Usage

### Basic Lemmatization
```python
import zeyrek
analyzer = zeyrek.MorphAnalyzer()

# Get lemmas for a single word
lemmas = analyzer.lemmatize('çalışıyorum')
print(lemmas)  # [('çalışıyorum', ['çalışmak'])]

# Process a sentence
sentence = "Ankara'da yaşıyorum ve çalışıyorum"
words = sentence.split()
for word in words:
    print(analyzer.lemmatize(word))
```

### Legal Text Matching
```python
# Define your legal corpus
kanun_cumleler = {
    "Taksirle başkasının vücuduna acı veren veya sağlığının ya da algılama yeteneğinin bozulmasına nedenolan kişi, üç aydan bir yıla kadar hapis veya adlî para cezası ile cezalandırılır.": "madde 89",
    "Kasten başkasının vücuduna acı veren veya sağlığının ya da algılama yeteneğinin bozulmasına neden olan kişi, bir yıldan üç yıla kadar hapis cezası ile cezalandırılır": "madde 86"
}

# Query the system
lemma('kasten birinin vücuduna zarar vermek.')
```

## Dependencies

- Python 3.x
- NLTK: For tokenization and stopwords
- Zeyrek: For Turkish morphological analysis
- FuzzyWuzzy: For string matching and similarity analysis
- String: For punctuation handling