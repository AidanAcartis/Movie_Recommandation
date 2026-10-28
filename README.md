# README

**Movie Recommendation with Scalable Matrix Factorization (MovieLens 25M)**

This repository contains the code and experiments for a from-scratch recommender system built on MovieLens data, going from a bias-only baseline to a latent factor model trained with Alternating Least Squares (ALS), and scaled to 25 million ratings.

## Contents

| Notebook | What it does |
|---|---|
| `Prac1-2-3-4(split-data_plot-data).ipynb` | Data loading and indexing (Practicals 1–4), train/test split, and dataset plots (degree distributions, CCDFs, rating histogram). |
| `practical4_movielens25m.ipynb` | Full ALS pipeline on MovieLens 25M: parallel Numba kernels, bias-only vs. latent factor comparison, overfitting study over `K`, `τ` and user activity, 2D movie and genre embeddings, synthetic-user recommendations, polarizing movies, and a norm-consistency sanity check. |

## Key results

- Best model (`K = 20`, `τ = 10`): **test RMSE ≈ 0.780**, vs. **0.860** for the bias-only baseline.
- Overfitting is concentrated on users with few ratings; it grows with `K` and shrinks with `τ`.
- Training on the full 25M ratings takes about 4 minutes on a Colab CPU runtime, using CSR storage and parallel compiled kernels.

## Implementation notes

- Ratings are stored in two CSR matrices (`int32` indices, `float32` values), about 360 MB for 25M ratings.
- All ALS inner loops (bias updates, factor updates, RMSE) are written as Numba `prange` kernels; each `K×K` system is solved with a hand-written Cholesky factorization.
- The compiled implementation matches the pure Python reference to numerical precision, with a measured speed-up of roughly `150×` on a 5,000-user subsample.

## Data

The notebooks expect the MovieLens datasets on Google Drive:

- `ml-latest-small` (610 users, 100k ratings) for development.
- `ml-25m` (162k users, 25M ratings) for the main experiments, with the 90/10 per-user split files `4train_ratings_90.csv` and `4test_ratings_10.csv`.

## Figures

All figures are written to a `figures/` folder next to the notebook. The LaTeX report (`main.tex`) picks them up automatically, and falls back to a placeholder box if a figure is missing.

## Running

1. Mount Google Drive and set the folder paths at the top of each notebook.
2. Run the split/plot notebook first if you need to regenerate the train/test files or the dataset plots.
3. Run `practical4_movielens25m.ipynb` end to end for the full experiment grid, the 2D embeddings, the synthetic-user recommendations, and the polarizing-movie analysis.

## Reference

The LaTeX report summarizing this work is in `main.tex` (AIMS-ICML style), with the bibliography in `references.bib`.