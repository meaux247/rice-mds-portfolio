# HARP: Human vs. AI Recordings Predictor

Binary classification system that detects whether a music recording is human-made or AI-generated.

**Course:** COMP 653, Statistical Machine Learning (Summer 2023) | **Team:** 3 members

## The Problem

As AI music generation models improved rapidly in 2023, a natural question emerged: can we tell the difference? HARP approaches this as an audio classification task using spectral analysis and deep learning.

## The Approach

Rather than analyzing raw audio waveforms, HARP converts recordings to mel spectrograms, visual representations of frequency content over time. A key design choice is the **Harmonic-Percussive Source Separation (HPSS)** preprocessing step: each recording is split into its harmonic (tonal) and percussive (rhythmic) components, and each stream is analyzed by its own Vision Transformer.

### Architecture

```
Raw Audio
  ├── Mel Spectrogram → HPSS Decomposition
  │     ├── Harmonic Stream → ViT-L-16 (frozen) → Features
  │     └── Percussive Stream → ViT-L-16 (frozen) → Features
  │                                                    ↓
  │                                              Concatenate
  │                                                    ↓
  │                                         Dense Layer → Sigmoid
  │                                                    ↓
  │                                          Human / AI Prediction
```

- **Transfer learning:** Only 10.3K of 606M parameters are trainable (ViT backbones frozen)
- **Custom augmentation:** TrivialAugment adapted for audio, including noise injection, clipping distortion, speed changes, reverb, time masking, high/low pass filtering
- **Optimizer:** Ranger21 with PyTorch Lightning

## Key Results

| Metric | Validation | Test |
|--------|-----------|------|
| **Accuracy** | 84.9% | **81.9%** |
| **AUROC** | 0.922 | **0.901** |
| F1 | 0.852 | 0.820 |
| Precision | 0.836 | 0.816 |
| Recall | 0.869 | 0.824 |

The 3% val-to-test drop suggests the model was not overfitting. AUROC of 0.90 is well above the 0.50 random baseline.

## Dataset

~16,000 recordings split across train/validation/test:
- **Human recordings:** Free Music Archive (FMA)
- **AI-generated recordings:** Synthetic audio from various generative models

## What I Learned

- Spectral decomposition (HPSS) adds meaningful signal; harmonic and percussive components carry different AI artifacts
- Transfer learning from ImageNet to spectrograms works well, even though the domains are very different
- Audio augmentation is underexplored compared to image augmentation; the custom TrivialAugment pipeline was an interesting part of this project

## Team

- **Taylor Chambers**
- **Andrew Meaux**
- **Chris Leinweber**

## Deliverables

- Final report (PDF)

## Built With

Python, PyTorch, PyTorch Lightning, torchaudio, torchvision (ViT-L-16), librosa, Optuna, Ranger21
