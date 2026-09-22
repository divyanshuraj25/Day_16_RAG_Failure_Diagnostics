# Day 16 – Diagnosing RAG Failure Modes 🔍

This project focuses on diagnosing common failure modes in a
Retrieval-Augmented Generation (RAG) pipeline.

## 🎯 Objective

The goal was to evaluate a RAG system across different failure modes
and understand whether errors originate from retrieval or answer generation.

## 🔄 RAG Pipeline

PDF
↓
Text Extraction
↓
Chunking
↓
Embeddings
↓
FAISS Retrieval
↓
Top-K Chunks
↓
Gemini Answer Generation
↓
Failure Analysis

## 🧪 Experiments

The experiment used 15 diagnostic queries targeting different RAG
failure scenarios:

- Retrieval Failure
- Context Window / Chunk Boundary Issues
- Answer-Context Mismatch
- Vague Context Retrieval
- Correct Chunk Retrieved but Wrong Answer Generated

For every query, retrieved chunks and retrieval distances were logged.

## 📊 Evaluation

Retrieval quality and answer quality were evaluated separately
using a 1–5 scorecard.

### Results

- Average Retrieval Quality: **2.57 / 5**
- Average Answer Quality: **3.00 / 5**

## 🔎 Key Finding

One important failure occurred when the correct chunk was retrieved,
but the generated answer incorrectly stated that the information was
not available in the retrieved context.

This demonstrates that good retrieval alone does not guarantee
correct RAG answers.

## 🛠️ Fixes Implemented

### 1. Similarity Threshold

A cosine-similarity threshold of **0.35** was introduced to reject
weakly related retrieved chunks.

### 2. Grounded Answer Prompt

A stricter prompt was implemented to:

- Use only retrieved context
- Avoid outside knowledge
- Avoid hallucinated information
- Answer directly when the information exists
- Return insufficient-information when the context genuinely lacks the answer

## 📁 Files

- `Day_16_RAG_Failure_Analysis.md` – Detailed experiment report
- `day16_master_results.json` – Complete 15-query retrieval log
- `diagnostic_results.json` – Gemini generation results
- `results.json` – Retrieval results

## ⚠️ Note

Gemini Free Tier rate limits affected answer generation after the
initial successful queries. Retrieval evaluation was completed for
all 15 queries, while only the successfully processed generation
results were recorded.

## 🧠 Learning Outcome

This experiment helped understand that RAG quality depends on multiple
stages:

**Retrieval Quality ≠ Answer Quality**

A RAG system can fail because the correct information was not retrieved,
or because the correct information was retrieved but not correctly
used during answer generation.
