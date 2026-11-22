# P4 — Recommendation Integration

**Owner:** Seungjae Chung

## Overview

This module implements the recommendation ranking and interactive interface for the Mood Music Recommender project.

It combines predicted musical emotion (valence and arousal) with CLAP audio-text similarity scores to rank songs according to a selected mood or activity.

Currently, P1's trained Ridge regression model provides emotion predictions, while CLAP similarity scores are simulated until P3 integration.

## Current Implementation

- Integrated P1's trained Ridge regression model for valence/arousal prediction.
- Implemented a weighted ranking algorithm combining emotion similarity and CLAP-style similarity scores.
- Created an interactive dropdown with five options: Relax, Study, Workout, Happy, and Sad.
- Implemented top-10 song recommendations.
- Added input validation and automated tests.
- Added CSV export functionality.

## Testing Results

- Generated Ridge model predictions for 75 DEAM development songs.
- Combined real emotion predictions with simulated CLAP similarity scores.
- All five mood/activity options returned 10 unique recommendations.
- Automated ranking and input-validation tests passed.
- Exported 50 recommendations across the five options.

These tests verify pipeline functionality, not final recommendation quality.

## Notebook

`ECE539_P4_Integration (2).ipynb`

## Limitations

- P1's Ridge regression model currently serves as the emotion prediction baseline.
- P2's MERT + MLP predictions have not yet been integrated.
- P3's CLAP similarity scores are simulated.
- Recommendation quality has not yet been evaluated using actual CLAP outputs.

## Next Steps

1. Replace P1 Ridge predictions with P2's MERT + MLP predictions.
2. Integrate actual CLAP similarity scores from P3.
3. Verify song ID alignment across model outputs.
4. Test the complete recommendation pipeline.
5. Evaluate recommendation quality using mood-hit@10.
6. Prepare the integrated system for the final demonstration.

**Status:** In Progress — P1 integration completed and tested; P2 and P3 integration pending.
