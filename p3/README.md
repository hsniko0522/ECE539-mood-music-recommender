# P3 — Frozen CLAP audio–text scoring

**Owner:** Thi Ngo

## Overview
Frozen LAION-CLAP scores how well each DEAM song matches a text prompt for each option (Relax, Study, Workout, Happy, Sad). P4's ranker combines these `clap_<Mood>` scores with P2's valence/arousal.

## Method
- Audio is resampled to 48 kHz and cut into 10 s windows. A song's score is the mean cosine between its window embeddings and the prompt.
- 6 candidate prompts per option plus an ensemble. The prompt chosen for each option is the one whose score best matches DEAM's valence/arousal (Spearman ρ), measured on train + dev only.
- Scores are centred: each song's mean over the five options is subtracted, which removes a per-song bias.

## Checkpoint change
The proposal's `laion/larger_clap_music` has a broken text encoder on Hugging Face: unrelated sentences all have cosine 0.999. This happens with transformers 4.x and 5.x and with both weight files, so every option would return the same songs. We use `laion/larger_clap_music_and_speech` instead, which has the same architecture. The notebook stops with an error if the text embeddings collapse.

## Results (train + dev)
| Option | Selected prompt | ρ | AUC |
|---|---|---:|---:|
| Relax | relaxing | 0.26 | 0.59 |
| Study | instrumental music without vocals, steady and calm | 0.20 | 0.54 |
| Workout | pumping rock music for running | 0.48 | 0.72 |
| Happy | cheerful music for a party with friends | 0.49 | 0.74 |
| Sad | slow, minor-key music with mournful strings | 0.55 | 0.76 |

The selected prompts beat P4's placeholder prompts for every option, and centred scores beat raw (ρ 0.48 vs 0.39). On dev, mood-hit@10 is 0.42 for CLAP alone, 0.56 for P2 alone, 0.50 for the two combined, and 0.22 by chance.

## Files
- `p3_clap_scoring.ipynb`: full pipeline (executed)
- `p3_clap_for_p4.csv`: `song_id, split, clap_*` for P4
- `p3_clap_config.json`: model, selected prompts, settings
- `prompt_*.csv`, `scoring_mode_comparison.csv`, `complementarity_with_p2_dev.csv`, `*.png`: comparisons and figures

## Notebook setup
Needs `DEAM_audio.zip` in `MyDrive/ECE539-mood-music/data/`. The split comes from P1's folder, or from `p2/va_predictions.csv` if that folder is missing. Executed locally on CPU (500 songs, about 25 min).

## Limitations
- DEAM has no activity labels, so Relax and Study cannot be told apart (their hit@10 is near chance).
- The metrics are reported on the same songs used to choose the prompts. The test split is held out.
