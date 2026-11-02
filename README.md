# Spotify Music Recommendation System Using Predictive Analysis

A Python-based, content-based music recommendation project that uses
Spotify track audio features and cosine similarity to suggest songs for
users based on their listening history.

## Project Overview

Music libraries can contain thousands of tracks, making it difficult for
listeners to discover songs with similar musical characteristics. This
project builds a recommendation pipeline from a static Spotify
listening-history dataset. It summarizes each user's listening history
into an average audio-feature profile, compares that profile with
candidate tracks, and produces a ranked list of recommendations.

**Important:** This is an offline educational project. It does not
connect to a live Spotify account or Spotify API, stream music, or
measure real user satisfaction.

## Features

-   Loads and inspects the Spotify CSV dataset.
-   Checks for missing values and duplicate records.
-   Fills missing text fields and numerical values as implemented in the
    notebook.
-   Selects and standardizes nine numerical audio features.
-   Calculates cosine similarity between standardized feature vectors.
-   Creates an average audio-feature preference profile for each user.
-   Generates the top 10 recommendations per user.
-   Exports recommendations to a CSV file.
-   Explores the dataset and plots playlist counts, audio-feature
    distributions, artists, recommendation scores, and feature
    correlations.

## Repository Files

  ---------------------------------------------------------------------------------
  File                                          Description
  --------------------------------------------- -----------------------------------
  `Spotify_recommendation_system.ipynb`         Jupyter Notebook containing
                                                preprocessing, exploratory
                                                analysis, recommendation logic, and
                                                visualizations.

  `spotify_data(3).csv`                         Input dataset containing track,
                                                user, playlist, and audio-feature
                                                information. For easier execution,
                                                rename it to `spotify_data.csv` or
                                                update the notebook's input path.

  `spotify_recommendations(1).csv`              Generated recommendation output for
                                                users. The notebook writes a file
                                                named
                                                `spotify_recommendations.csv`.

  `Spotify_Recommendation_System_Report.docx`   Project report describing the
                                                dataset, methodology, results,
                                                limitations, and future
                                                enhancements.

  `README.md`                                   Project overview and run
                                                instructions.
  ---------------------------------------------------------------------------------

## Dataset

The supplied input dataset contains **28,469 rows and 22 columns**,
including:

-   Identifiers and descriptions: `user_id`, `artistname`, `trackname`,
    `playlistname`, `song_id`, and `id`
-   Audio features: `danceability`, `energy`, `loudness`, `speechiness`,
    `acousticness`, `instrumentalness`, `liveness`, `valence`, and
    `tempo`
-   Additional metadata: `key`, `mode`, `duration_ms`, `time_signature`,
    `type`, `analysis_url`, and `uri`

The project report describes 250 distinct users, 21,454 distinct track
names, 6,185 distinct artists, and 1,078 distinct playlists in the
original dataset.

## Technology Stack

-   **Python** --- core programming language
-   **Jupyter Notebook** --- interactive development and execution
-   **Pandas** --- loading, cleaning, grouping, and exporting tabular
    data
-   **NumPy** --- numerical operations
-   **Scikit-learn** --- `StandardScaler` and `cosine_similarity`
-   **Matplotlib** --- charts and plots
-   **Seaborn** --- audio-feature correlation heatmap

## Recommendation Method

### 1. Preprocessing

The notebook loads the CSV into a Pandas DataFrame, inspects its
structure, checks missing values and duplicate rows, fills missing
artist/track/playlist text with `Unknown`, fills missing numerical
values with column medians, and removes selected unnecessary columns
(`analysis_url`, `uri`, and `type`).

### 2. Feature selection and scaling

Nine audio features are used to represent each track:

`danceability`, `energy`, `loudness`, `speechiness`, `acousticness`,
`instrumentalness`, `liveness`, `valence`, and `tempo`.

`StandardScaler` standardizes these features so that their different
numerical ranges have less influence on similarity calculations.

### 3. Cosine similarity

Cosine similarity compares the direction of two feature vectors. A
higher score indicates that the vectors are more similar under the
selected features.

### 4. User profile and ranking

For each user, the notebook averages the standardized audio features of
tracks in that user's history to create a preference profile. It scores
candidate tracks against that profile, excludes tracks already in the
user's history, sorts candidates by score, and selects up to 10
recommendations.

## Setup and Installation

Python 3.10 or later is a reasonable starting point. Install the
libraries used by the notebook:

``` bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
```

Alternatively, open the notebook in **Google Colab** and upload the
input CSV file.

## How to Run

### Option A: Run locally

1.  Place the notebook and input CSV in the same project folder.

2.  Rename `spotify_data(3).csv` to `spotify_data.csv`, **or** change
    the notebook's CSV path from `/content/spotify_data.csv` to the
    local file path, for example:

    ``` python
    df = pd.read_csv("spotify_data.csv")
    ```

3.  Start Jupyter:

    ``` bash
    jupyter notebook
    ```

4.  Open `Spotify_recommendation_system.ipynb`.

5.  Run the notebook cells from top to bottom.

6.  After the export cell completes, check the working directory for
    `spotify_recommendations.csv`.

### Option B: Run in Google Colab

1.  Upload `Spotify_recommendation_system.ipynb` to Google Colab.
2.  Upload the input CSV and ensure the notebook can access it at
    `/content/spotify_data.csv`.
3.  If your uploaded file has a different name, either rename it or
    update the `pd.read_csv(...)` line.
4.  Run all cells in order.
5.  Download `spotify_recommendations.csv` from the Colab file panel
    after the notebook creates it.

## Output

The notebook exports a CSV with these five columns:

  -----------------------------------------------------------------------
  Column                              Meaning
  ----------------------------------- -----------------------------------
  `user_id`                           User for whom recommendations were
                                      generated

  `recommendation_rank`               Rank of the recommendation for that
                                      user

  `trackname`                         Recommended track title

  `artistname`                        Artist name

  `recommendation_score`              Cosine similarity between the
                                      candidate track and the user's
                                      average feature profile
  -----------------------------------------------------------------------

The supplied recommendations file contains **2,500 rows**, corresponding
to 10 recommendation rows for each of 250 users. The report records a
mean recommendation score of **0.9369**.

A high recommendation score means feature-vector similarity under this
calculation; it **does not** mean 93.69% accuracy or prove that a user
will like the track.

## Evaluation Notes and Limitations

-   The current implementation does not use explicit relevance labels,
    ratings, clicks, or a held-out test set. Therefore, the similarity
    score is not model accuracy, and accuracy/precision/recall/F1 are
    not directly applicable as currently implemented.
-   The report identifies **342 repeated user-track-artist
    combinations** in the generated output. Candidate tracks are not
    fully deduplicated before the top 10 are selected, so repeated
    recommendations may appear.
-   Averaging a user's listening history can hide different genres or
    listening moods.
-   The system uses nine audio features and does not incorporate lyrics,
    genre, artist relationships, or direct feedback.
-   The notebook constructs a **28,469 × 28,469** pairwise similarity
    matrix, which can require substantial memory as the dataset grows.

## Future Enhancements

-   Deduplicate recommendations by track ID or by track and artist
    before selecting the top 10.
-   Add collaborative filtering to create a hybrid recommendation model.
-   Incorporate additional metadata such as genre, release year, or
    language when available.
-   Use likes, skips, and playlist additions as feedback.
-   Evaluate ranking quality with a chronological holdout and metrics
    such as Precision@K, Recall@K, MAP@K, or NDCG@K.
-   Use nearest-neighbour search instead of storing a full pairwise
    similarity matrix for larger datasets.
-   Build a simple web interface for selecting a user or seed track and
    viewing recommendations.

## References

-   Spotify for Developers --- [Get Track's Audio
    Features](https://developer.spotify.com/documentation/web-api/reference/get-audio-features)
-   Scikit-learn ---
    [StandardScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)
-   Scikit-learn ---
    [cosine_similarity](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html)
-   Pandas --- [DataFrame
    documentation](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)
-   Matplotlib --- [Documentation](https://matplotlib.org/stable/)
-   Jupyter --- [Documentation](https://docs.jupyter.org/)

## Authors

Course-end project: **Spotify Music Recommendation System Using
Predictive Analysis**\
Department of Artificial Intelligence and Data Science\
Chaitanya Bharathi Institute of Technology, Hyderabad

Project report authors: - Gondela Durga Gnana Deekshitha - Gundlapally
Ananya - Vemula Akshaya

Mentor: Mrs. V. Vasuki Rohini Devi

------------------------------------------------------------------------

This README is based on the uploaded notebook, CSV files, and project
report. Update file names and paths if you rename files or reorganize
the project.
