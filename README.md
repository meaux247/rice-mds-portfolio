# Rice University: Master of Data Science Portfolio

A collection of projects from my time in the MDS program (2022-2023) at Rice University, Houston TX. These are academic deliverables (final reports) showcasing the problems I found interesting across NLP, graph neural networks, multimodal deep learning, and audio classification. The papers tell the full story; the READMEs provide context and summaries.

---

## Projects

### [HARP: Human vs. AI Recordings Predictor](./harp-ai-music-detection/)
Can a model distinguish AI-generated music from human recordings? We built a dual-stream Vision Transformer that analyzes harmonic and percussive spectrograms separately, achieving **0.90 AUROC** on held-out test data.

**Course:** COMP 653, Statistical Machine Learning (Summer 2023) | **Team:** 3 members

---

### [EnGenIR: RAG for Pipeline Engineering](./engenir-rag-pipeline/)
A Retrieval-Augmented Generation system that answers technical questions about pipeline engineering standards (ASME, API). By embedding domain documents and using chain-of-thought prompting, EnGenIR outperformed zero-shot GPT-4 and Claude 2 on domain-specific accuracy in our internal evaluation.

**Course:** DSCI 535, Data Science Capstone / D2K Lab (Summer 2023) | **Team:** 6 members

---

### [VIBE: Visual Input to Beat Explorations](./vibe-album-to-music/)
What if you could turn album artwork into music? VIBE chains CLIP zero-shot genre classification, GPT-3.5 lyric generation, and OpenAI Jukebox to create an image-to-audio pipeline. CLIP achieved **78.6% top-3 genre accuracy** across 15 genres.

**Course:** COMP 646, Deep Learning for Vision and Language (Spring 2023) | **Team:** 4 members

---

### [GraMR: Graph-based Music Recommendations](./gramr-music-recommendation/)
A heterogeneous graph neural network trained on 100,000 Spotify playlists and 700,000 songs for link prediction. Four playlist generation algorithms produce novel playlists from learned song embeddings.

**Course:** COMP 559, Machine Learning with Graphs (Spring 2023) | **Team:** 2 members

---

## About These Projects

These were group projects completed during a one-year professional master's program. Each folder contains the final report as the primary deliverable. The READMEs provide context and summaries; the papers contain the full methodology and results.

## About the Program

The [Master of Data Science](https://csweb.rice.edu/academics/graduate-programs/professional-master-data-science) at Rice University is a professional program covering machine learning, statistics, big data systems, deep learning, and data ethics.
