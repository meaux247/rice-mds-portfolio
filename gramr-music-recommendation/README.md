# GraMR: Graph-based Music Recommendations

A heterogeneous graph neural network for playlist generation, trained on 100,000 Spotify playlists and 700,000 songs.

**Course:** COMP 559, Machine Learning with Graphs (Spring 2023) | **Team:** 2 members

## The Problem

Music recommendation is fundamentally a graph problem. Songs connect to playlists, playlists connect to genres, and listeners traverse these connections when discovering music. Traditional collaborative filtering flattens this into a co-occurrence matrix; GraMR models these connections directly as a graph.

## The Approach

We modeled the Spotify Million Playlist Dataset as a **heterogeneous bipartite graph** with song nodes and playlist nodes connected by membership edges. A Graph Neural Network (GNN) learns embeddings for each song based on its neighborhood (which playlists it appears in, and what other songs share those playlists).

### Architecture

```
Spotify Data (100K playlists, 681K songs)
  → Heterogeneous Bipartite Graph (songs ↔ playlists)
  → SAGEConv GNN (message passing on heterogeneous edges)
  → Learned Song Embeddings (latent space)
  → Link Prediction (does song X belong in playlist Y?)
  → 4 Playlist Generation Algorithms
```

### Link Prediction

The GNN was trained for edge-level link prediction: given a song and a playlist, predict whether the song belongs in that playlist.

- **Split:** 70/15/15 train/val/test using PyTorch Geometric's `RandomLinkSplit`
- **Architecture:** SAGEConv on heterogeneous graph (`HeteroData`)
- **Training:** 5 epochs on ~9.2M training edges
- **Validation ROC-AUC:** **0.82**

### Playlist Generation

Four algorithms generate novel playlists from the learned embeddings:

1. **Most-Similar:** For each seed song, find the K nearest neighbors in embedding space
2. **Less-Random Walk:** Weighted random walk through the embedding space, biased toward similarity
3. **Weighted Random Walk:** Walk with probability proportional to edge weights
4. **Pure Random Walk:** Unbiased exploration from seed songs

Each algorithm takes a set of seed songs (e.g., 3 songs you like) and generates a playlist of N songs. The Spotify API resolves song IDs back to human-readable track names.

## Key Results

| Metric | Value |
|--------|-------|
| **Link Prediction AUC** | **0.82** |
| Training edges | 9.2M |
| Song nodes | 681,660 |
| Playlist nodes | 100,000 |

An AUC of 0.82 means the model correctly ranks a true playlist-song pair above a random pair 82% of the time.

### Limitations

- Only 5 training epochs (loss was still decreasing, and more epochs might improve results)
- AUC reported on validation set, not held-out test set
- Genre distribution in the dataset is highly imbalanced (jazz: 91K, pop: 16), so genre features are unreliable for rare categories
- The generated playlists were evaluated qualitatively, not with quantitative diversity/novelty metrics

## What I Learned

- PyTorch Geometric's heterogeneous graph support (`HeteroData`, `to_hetero`) is powerful but has a steep learning curve
- Graph neural networks capture the collaborative signal in playlist data: songs that co-occur in many playlists get similar embeddings without explicit feature engineering
- The choice of playlist generation algorithm matters more than small improvements in link prediction accuracy; the walk-based methods produce more diverse playlists than pure nearest-neighbor

## Team

COMP 559, Machine Learning with Graphs (2 members):
- **Andrew Meaux**
- **Chris Leinweber**

## Deliverables

- Final paper (PDF)

## Built With

Python, PyTorch, PyTorch Geometric (SAGEConv, HeteroData, RandomLinkSplit), Spotify API (spotipy), NetworkX, scikit-learn
