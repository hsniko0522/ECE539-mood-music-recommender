# P2 — Frozen MERT features + trained valence/arousal head

**Owner:** Shuai Hao

## Overview
Component C1 (frozen MERT-v1-95M) turns each DEAM song into a 13 × 768 embedding; component C2 (an MLP we train,
768 → 256 → 2, ~197k parameters) predicts song-level valence and arousal on DEAM's 1–9 scale.
This is the team-trained neural component of the project.

## Method
- Audio resampled to 24 kHz mono and cut into 5 s chunks (MERT's pre-training context); every layer is averaged over all frames.
- Features for all 1,802 DEAM songs are cached on Drive (~8 min on a Colab T4).
- MLP trained with MSE + Adam and early stopping on dev MSE; inputs and targets standardized with train-split statistics only.
- MERT layer and regularization chosen on the dev split; the test split is untouched.

## Results (dev R², valence / arousal)
On P1's 500-song split (350/75/75), same MLP head, only the input features change:

| Features | Model | Valence R² | Arousal R² |
|---|---|---:|---:|
| openSMILE (P1) | Ridge (α = 100) | 0.223 | 0.384 |
| openSMILE (P1) | MLP head | 0.204 | 0.469 |
| MERT layer 8 | Ridge (α = 1000) | 0.372 | 0.545 |
| **MERT layer 8** | **MLP head (C2)** | **0.426** | **0.546** |

On all 1,802 songs with an artist-grouped 1,212/300/290 split, MERT layer 9 + MLP reaches 0.502 / 0.588.

## Files
- `p2_mert_va_head.ipynb` — full pipeline (run in Colab with a T4 GPU)
- `p2_va_for_p4.csv` — `song_id, split, valence, arousal` predictions for P4's ranker
- `va_predictions.csv` — predictions with true labels, for evaluation
- `va_head.pt`, `va_head_config.json` — trained head and its settings
- `dev_summary.csv`, `layer_sweep_dev.png`, `training_curves_and_dev_scatter.png` — results and figures

## Notebook setup
Needs `DEAM_audio.zip` in `MyDrive/ECE539-mood-music/data/` (cell 1a can download it) and a shortcut to P1's `ECE539_P1` folder.
If the shortcut is not at `MyDrive/ECE539_P1`, edit `P1_DIR` in the setup cell.

## Limitations
- With 350 training songs the head overfits after ~3 epochs; more data helps more than tuning.
- P1's split is song-level, so 25 artists appear in more than one partition, which can make dev scores optimistic.
