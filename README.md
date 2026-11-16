# Music Popularity Capstone Project: drum-and-bass

**Machine Learning and AI (Essentials), Thrive Campus | Group 4 (Vector Minds) | October 2026**

What makes a drum-and-bass track popular? This project answers three questions for the A&R (promotion) department of a music streaming service, using audio features from Spotify.

| Question | Machine learning task | Notebook part |
|---|---|---|
| What makes a track popular? | Regression: predict popularity (0 to 100) from audio | Part 4 |
| Can we spot a hit before it charts? | Classification: hit or not a hit | Part 5 |
| What moods exist inside the genre? | Clustering: group similar tracks | Part 6 |

Every model result in the notebook ends with a sentence starting "This means the streaming service should...".

---

## Key findings

- **Genre:** drum-and-bass, chosen after testing 108 genres and checking that each genre label held together (k-pop, german, hardcore and dub were rejected because their labels mixed different kinds of music).
- **Data:** 953 cleaned tracks from 174 lead artists.
- **Popularity falls into two groups:** 682 tracks score below 30 and 235 score 40 or more. No track reaches 70.
- **Audio explains only about 18% of popularity** (average R² over ten artist-grouped splits: 0.177 for linear regression, 0.183 for random forest). The typical error is about 13 points on the 0-to-100 scale. Longer and more instrumental tracks tend to be less popular.
- **Hit spotting is cautious but weak:** with a hit defined as popularity of 40 or more (235 tracks, 24.7%), logistic regression is right 57% of the time when it flags a hit (about 25% by chance) but finds only 30% of real hits.
- **Three moods** (Dark & Heavy, Bright & Driving, Lower Gear). Dark & Heavy is the largest (49% of tracks) and has the highest share of hits (29%).
- **Recommendation:** we should treat audio as one weak signal alongside artist, label and release data, use the hit model only as a shortlist and test promotions on the Dark & Heavy mood first.

---

## Repository contents

| File | What it is |
|---|---|
| `Music_Popularity_Capstone_Project_GROUP4.ipynb` | The full analysis. Run this one |
| `Music_Popularity_Capstone_Report_GROUP4.docx` | The written report (non-technical reader) |
| `Music_Popularity_Capstone_Presentation_GROUP4.pptx` | The presentation slides |
| `README.md` | This file |

The dataset is included in this repository.

---

## How to run the notebook

### 1. Get the data (Option 2)

1. Create a free Kaggle account if you don't have one.
2. Download `dataset.csv` from: https://www.kaggle.com/datasets/maharshipandya/-spotify-tracks-dataset
3. **Rename it to `spotify_tracks.csv`.**
4. Save it **in the same folder as the notebook**, not in a subfolder. The notebook looks for it there.

### 2. Install the libraries

The notebook was built with these versions:

```
pip install pandas==2.2.2 numpy==1.26.4 scikit-learn==1.5.1 matplotlib seaborn jupyter
```

### 3. Run it

1. Open `Music_Popularity_Capstone_Project_GROUP4.ipynb` in Jupyter.
2. Choose **Kernel -> Restart & Run All**.
3. Wait for the last cell to finish. Part 1.7 scores 108 genres and takes a few minutes.

The notebook runs from top to bottom with no errors and every random step uses `RANDOM_STATE = 42`, so you should get the same numbers as the report.

### If something goes wrong

| Error you see | What it usually means |
|---|---|
| `FileNotFoundError: spotify_tracks.csv` | The file is not in the same folder as the notebook, or it still has the name `dataset.csv` |
| Filter returns 0 tracks | Spelling. Genres are lowercase, and some use a dash (for example `drum-and-bass`) |
| `NameError` | A cell was run out of order. Choose Restart & Run All |
| The notebook is slow | Part 1.7 scores 108 genres and takes a few minutes. This is normal |

---

## How the notebook is organised

| Part | What happens |
|---|---|
| **Setup** | Libraries, settings, loading the data, a first look |
| **Part 1: Choosing the genre** | Compare all 114 genres, test how well audio predicts popularity in each (keeping each artist on one side of the split), check label quality, and record why other genres were rejected. How drum-and-bass differs from the other genres |
| **Part 2: Cleaning** | A cleaning log with a reason for every decision. New columns: `duration_min`, `lead_artist`, `tempo_was_doubled`, `is_hit` |
| **Part 3: Charts** | Four charts, each with a title, axis labels and a written interpretation |
| **Part 4: Predict popularity** | Regression: baseline, linear regression and random forest, scored on the test set, with coefficients in real units |
| **Part 5: Spot the hits** | Classification: baseline, logistic regression and decision tree, with accuracy, precision, recall and a confusion matrix |
| **Part 6: Find the moods** | K-means clustering on valence and energy, with the number of groups justified |
| **Part 7: Tune** | Grid Search on the decision tree, with before and after reported |

---

## Key decisions

| Decision | What we chose | Why |
|---|---|---|
| Genre | Drum-and-bass | Clean label, stable score, 174 artists |
| Repeated songs | Kept the most popular copy of each song within a genre | Stops a song sitting in both training and testing |
| Tempo | Doubled values from 75 to 92 BPM (121 tracks) | Spotify sometimes measures half speed. Drum-and-bass runs at about 174 BPM |
| Train/test split | Each artist is kept entirely in training or testing | Stops the model scoring well by recognising artists |
| Headline score | Average over ten splits (18%) | One split depends on which artists land in the test set |
| Correlation | Spearman | Several features are very skewed and popularity falls into two groups |
| Hit definition | Popularity of 40 or more | No track reaches 70 and the genre median (19) falls inside the biggest cluster of tracks. The value is one variable, `HIT_LINE` |
| Leakage | `popularity` and `is_hit` are never model inputs | A hit is built from popularity, so including it would give the answer away |
| Classification measures | Precision and recall, with a baseline | Always predicting "not a hit" is right 77% of the time |
| Tuning score | F1 | Accuracy would drift toward never predicting a hit |
| Number of moods | Three | The elbow plot bends there and the silhouette score is highest (0.42, moderate) |

---

## Limitations

- Audio features cannot capture marketing spend, playlist placement, an artist's existing fanbase, release timing or social media. These probably explain most of the 82% of popularity the models could not.
- Popularity is a snapshot of right now, not of all time.
- A "hit" means popularity of 40 or more, within drum-and-bass only. A different line would give different numbers.
- Genre labels come from playlists, so a few tracks may not truly be drum-and-bass.
- Results come from about 950 tracks and 174 artists, and scores moved between splits (for example from -2% to 28% for linear regression), so every number has real uncertainty.
- The tempo fix assumes tracks measured at 75 to 92 BPM were half-time errors.
- The mood groups divide one continuous cloud of tracks and were named from averages and sample titles, not from listening tests.

---

## Team

| Name | Role |
|---|---|
| Regina Robertson | data cleaning |
| [Member name] | [regression] |
| [Member name] | [classification] |
| Obed Kwame Zar | clustering and report |

Every member has commits in this repository.

## Use of an AI assistant

We used an AI assistant (Claude) to explain errors and concepts and to help draft code and wording. We checked every result against our own outputs and can explain every step.

## Data source

Spotify Tracks Dataset, Maharshi Pandya, Kaggle: https://www.kaggle.com/datasets/maharshipandya/-spotify-tracks-dataset
