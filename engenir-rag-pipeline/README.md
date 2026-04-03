# EnGenIR: Engineering Generative Transformers with Information Retrieval

A Retrieval-Augmented Generation (RAG) system for answering technical questions about pipeline engineering standards.

**Course:** DSCI 535, Data Science Capstone / D2K Lab (Summer 2023) | **Team:** 6 members

## The Problem

Facilities and pipeline engineers routinely reference dense technical specifications (ASME B31.4, ASME B31.8, API 5L) to answer design and compliance questions. These documents are hundreds of pages of highly specific technical language. In 2023, general-purpose LLMs couldn't reliably answer domain-specific engineering questions because the knowledge was too specialized for accurate zero-shot retrieval.

## The Approach

EnGenIR is a RAG pipeline that embeds engineering specification documents, retrieves relevant passages for user queries, and constructs enhanced prompts for LLM response generation. The system was built as the capstone project for the Rice University D2K Lab.

### Pipeline

```
Engineering PDFs (ASME, API)
  → PDF Parsing (PyPDF, pdfminer, pymupdf comparison)
  → Text Chunking
  → Embedding (4 models benchmarked)
  → Vector Database (Pinecone)
  → Query → Semantic Search → Top-K Retrieval
  → Prompt Construction (4 strategies)
  → LLM Response (GPT-3.5 / GPT-4)
  → Answer with Source Citations
```

### What We Benchmarked

**Embedding models** (evaluated on 216 QA pairs using Recall@k and nDCG@k):

| Model | Recall@10 |
|-------|-----------|
| **E5-Large-v2** | **84.0%** |
| Ada-002 (OpenAI) | 82.1% |
| Instructor-Large | 76.9% |
| TF-IDF Baseline | 51.4% |

**Prompt strategies:** Zero-shot, RAG with sources, custom prompt templates, and chain-of-thought with query decomposition.

## Key Results

We evaluated EnGenIR against zero-shot GPT-4 and Claude 2 on 23 expert-curated engineering questions. Six team members independently rated each response on accuracy, relevance, and readability (1-3 scale).

| Model | Accuracy | Relevance | Readability |
|-------|----------|-----------|-------------|
| **EnGenIR** | **2.54** | **2.82** | 2.88 |
| GPT-4 (zero-shot) | 1.41 | 2.24 | **2.98** |
| Claude 2 (zero-shot) | 1.33 | 2.54 | 2.94 |

EnGenIR outperformed both LLMs on domain-specific accuracy in our internal evaluation, which is the expected outcome of a well-designed RAG system. The comparison demonstrates that retrieval augmentation helps in specialized domains, not that EnGenIR is a "better" model than GPT-4 or Claude 2, both of which produced more readable responses as expected from large general-purpose models.

### Limitations

- Evaluation was conducted by team members, not independent domain experts
- Sample size was 23 questions (small N, though 6 raters per question gives 138 data points)

## What I Learned

- RAG effectiveness depends heavily on the retrieval step. Getting the right chunks to the LLM is the most important factor
- Embedding model selection has a real impact: E5-Large-v2 beat OpenAI's Ada-002 on our benchmark
- Chain-of-thought prompting with query decomposition improved accuracy on multi-step engineering questions
- PDF parsing is non-trivial; different loaders handle tables, headers, and formatting differently

## Team

Rice University D2K Lab, Summer 2023 (6 members):
- **Andrew Meaux**
- **Rhett Johnson**
- **Chris Leinweber**
- **Diyaz Yespayev**
- **Ruslan Kharko**
- **Zhanibek Issabekov**

## Deliverables

- Final report (PDF)

## Built With

Python, LangChain, OpenAI (GPT-3.5/4, Ada-002), Anthropic (Claude 2), Microsoft E5-Large-v2, Pinecone, PyPDF/pdfminer
