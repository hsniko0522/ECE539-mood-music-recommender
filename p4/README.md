# P4 — Recommendation Integration

**Owner:** Seungjae Chung

## Overview

This module implements the recommendation ranking and interactive interface for the Mood Music Recommender project.

It combines predicted musical emotion (valence and arousal) with CLAP audio-text similarity scores to rank songs according to a selected mood or activity.

## Current Implementation

- Implemented a weighted ranking algorithm combining emotion similarity and CLAP-style similarity scores.
- Created an interactive dropdown with five options: Relax, Study, Workout, Happy, and Sad.
- Implemented top-10 song recommendations.
- Added input validation and automated tests.
- Added CSV export functionality.

## Testing Results

- Successfully processed 100 simulated songs.
- All five mood/activity options returned 10 unique recommendations.
- Automated ranking and input-validation checks passed.
- Exported 50 recommendations across the five options.

## Notebook

`ECE539_P4_Integration.ipynb`

Open the notebook in Google Colab and select **Runtime → Run all**.

No GPU or external dataset is required for the current prototype.

## Limitations

The current prototype uses simulated valence/arousal predictions and CLAP similarity scores rather than actual model outputs.

Therefore, the tests verify the functionality of the ranking pipeline, not the accuracy or quality of music recommendations.

## Next Steps

1. Obtain the shared DEAM song IDs and data splits from P1.
2. Integrate predicted valence/arousal outputs from P2 (MERT + MLP).
3. Integrate audio-text similarity scores from P3 (CLAP).
4. Test the integrated system with actual DEAM songs.
5. Evaluate recommendation quality using mood-hit@10.

**Status:** In Progress — standalone prototype implemented and tested.
