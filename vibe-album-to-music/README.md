# VIBE: Visual Input to Beat Explorations

A multimodal AI pipeline that generates music from album cover artwork.

**Course:** COMP 646, Deep Learning for Vision and Language (Spring 2023) | **Team:** 4 members

## The Problem

Album artwork is a visual expression of musical identity, with genre, mood, era, and aesthetic all encoded in the cover design. Can a machine read those visual signals and produce music that matches? VIBE explores this question by chaining computer vision, natural language processing, and audio generation into a single end-to-end pipeline.

## The Approach

VIBE is a three-stage pipeline connecting pretrained models across three modalities:

### Pipeline

```
Album Cover Image
  → CLIP Zero-Shot Genre Classification (15 genres)
  → Predicted Genre + Artist Context
  → GPT-3.5 Lyric Generation (3 prompt strategies)
  → Artist + Genre + Lyrics
  → OpenAI Jukebox Music Generation (60-second clips)
  → Generated Music
```

### Stage 1: Visual Genre Classification

We used OpenAI's CLIP model for zero-shot genre classification with no fine-tuning, just crafted text prompts describing each genre. Three prompt strategies were tested:

| Strategy | Top-1 Accuracy | Top-3 Accuracy |
|----------|---------------|---------------|
| Baseline CLIP | Low | N/A |
| Score-balanced | Moderate | N/A |
| **Context-optimized** | **56.3%** | **78.6%** |

With 15 genres and a random baseline of 6.7%, the context-optimized approach performs well. A human benchmark was also conducted for comparison (75 samples, 5 per genre).

**Dataset:** 143,109 albums, 6,573 artists, 15 parent genres from Spotify metadata.

### Stage 2: Lyric Generation

GPT-3.5-turbo generates lyrics conditioned on artist style and predicted genre. Three prompt iterations were developed, progressively adding musical context (tempo, mood, structure) to improve lyrical coherence.

### Stage 3: Music Generation

OpenAI's Jukebox model generates 60-second audio clips conditioned on artist, genre, and lyrics. The three-stage Jukebox architecture (base layer → two upsample layers) produces full audio at increasing fidelity.

## Generated Samples

The pipeline produced listenable music. Audio samples include:
- Garth Brooks (Country), generated from album art analysis
- Madonna (Pop/Jazz crossover)
- Rolling Stones (Rock/Hip-hop fusion)
- And several others across genres

## What I Learned

- Zero-shot CLIP is effective at visual genre classification; the key is prompt engineering, not fine-tuning
- Multimodal pipelines are fragile: errors compound across stages. A misclassified genre cascades into wrong lyrics and wrong music
- Jukebox produces coherent audio given just text conditioning, but quality varies widely across genres
- The most interesting results came from genre mismatches; when CLIP classified a rock album as jazz, the generated music had an unexpected fusion quality

## Limitations

- No quantitative evaluation of generated music quality (no listener study or MOS scores)
- This is a systems integration project; no models were trained, only chained
- CLIP genre classification is inherently noisy since many albums genuinely span multiple genres
- Jukebox generation is slow and compute-intensive

## Team

COMP 646, Deep Learning for Vision and Language (4 members):
- **Taylor Chambers**
- **Andrew Meaux**
- **Chris Leinweber**
- **Huafeng Liu**

## Deliverables

- Final report (PDF)

## Built With

Python, OpenAI CLIP, OpenAI GPT-3.5, OpenAI Jukebox, Spotify API, PyTorch
