# P4 — Recommendation Integration

**Owner:** Seungjae Chung

## Overview

P4 integrates P2's MERT-based valence/arousal predictions and P3's CLAP audio-text similarity scores into a mood-based music recommendation system.

The system ranks songs for five moods and activities: Relax, Study, Workout, Happy, and Sad.

## Implementation

- Integrated P2's trained MERT + MLP valence/arousal predictions.
- Integrated P3's real CLAP similarity scores.
- Matched model outputs using song IDs.
- Implemented a weighted ranking algorithm combining emotion similarity and CLAP similarity.
- Tuned the ranking weights using development-set data only.
- Built an interactive mood-selection dropdown.
- Implemented top-10 song recommendations and CSV export.
- Added automated tests for ranking and output validation.

## Ranking Method

Emotion similarity is calculated using normalized Euclidean distance between a song's predicted valence/arousal and the target emotion coordinates.

CLAP similarity scores are normalized separately for each mood.

The combined ranking score is:

`score = w × emotion_similarity + (1 − w) × clap_similarity`

The global weight was selected using mean Spearman rank correlation against ground-truth emotion similarity on the development set.

**Selected weights:**
- Emotion similarity: 0.90
- CLAP similarity: 0.10

## Results

- Successfully aligned P2 and P3 predictions for 500 DEAM songs.
- Used 350 training, 75 development, and 75 held-out test songs.
- Achieved mean development Spearman correlation of 0.6754 with the combined ranking.
- P2 emotion-only ranking achieved 0.6600 under the same metric.
- All five moods produced 10 unique recommendations.
- All automated ranking tests passed.
- Exported 50 development-set recommendations.

## Files

- `ECE539_P4_P2_P3_Integrated.ipynb` — integrated recommendation system and interactive interface.
- `P4_dev_weight_comparison.csv` — development-set ranking weight comparison.
- `P4_P2_P3_DEV_top10.csv` — top-10 development recommendations for all five moods.

## Limitations

- The ranking weights were selected using development-set labels.
- The combined model has not been shown to outperform P2 alone on mood-hit@10.
- Relax and Study remain difficult to distinguish using the available DEAM emotion labels.
- The held-out test split has not been evaluated.

## Next Steps

1. Evaluate the final ranking method on the held-out test set.
2. Compare P2-only, P3-only, and combined recommendation performance.
3. Analyze recommendation quality across the five moods.
4. Prepare the final system demonstration.

**Status:** P2/P3 integration completed and tested. Final evaluation pending.
