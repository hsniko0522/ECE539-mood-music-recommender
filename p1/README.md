(https://drive.google.com/drive/folders/1BvSJx0KPNfJgRjHMGdZJdhw1j9BqbudH?usp=sharing)

## Overview:
This prepares the DEAM dataset and implements baseline models to predict valence and arousal using openSMILE acoustic features.

## Data Preperation:

- Selected 500 songs from the dataset.
- Averaged 260 acoustic features across time windows for each song.
- Created reproducible training with 70/15/15 splits.
- Used random seed 539.

## Models:

- Linear Regression
- Ridge Regression (alpha=100)

## Development Results:

| Model | Valence R² | Valence RMSE | Arousal R² | Arousal RMSE |
|---|---:|---:|---:|---:|
| Linear Regression | -1.6053 | 1.7405 | -2.0066 | 2.1371 |
| Ridge (alpha=100) | 0.2128 | 0.9567 | 0.3843 | 0.9671 |

Ridge regression improved development performance and reduced overfitting compared with ordinary linear regression.

## Notebook:
Run the ECE539_P1_DEAM (2).ipynb in colab. This notebook requires access to the deam files in google drive the path is /content/drive/MyDrive/ECE539_P1, so users must updatate the project_dir to match their own Google Drive.

## Limitations:
The inital experiment uses 500 songs. Splits are song-level rather than artist grouped. The test set is reserved for final evaluation after model selection.
