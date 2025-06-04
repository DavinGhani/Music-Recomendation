# Music Recommendation System

This project builds a content-based music recommendation system using a dataset of 200,000 Spotify songs. The system recommends similar songs based on features such as genre, emotion, and key, along with audio attributes like tempo and loudness. The model leverages **TF-IDF Vectorization** and **Cosine Similarity** to measure song similarity.

## Project Overview

This project includes:
- **Data Loading**: Loading a Spotify dataset containing information about various songs.
- **Data Preprocessing**: Cleaning the data and transforming categorical features into usable formats.
- **Feature Engineering**: Combining multiple features (like genre, emotion, key, and explicit) to create a unified representation.
- **Modeling**: Using **TF-IDF Vectorization** to create vector representations of the songs, and **Cosine Similarity** to find similar songs.
- **Recommendation**: Creating an interactive system where users can input a song title and get recommendations based on similarity.

### Dataset

The dataset used in this project is the **200k Spotify Songs Light Dataset**, which contains the following features for each song:
- `artist`: Name of the artist.
- `song`: Title of the song.
- `emotion`: Emotion label (joy, sadness, etc.).
- `Genre`: Genre of the song.
- `Tempo`: Tempo of the song.
- `Loudness`: Loudness of the song.
- `Popularity`: Popularity of the song.
- `Energy`, `Danceability`, `Positiveness`, `Speechiness`, `Liveness`, `Acousticness`, `Instrumentalness`: Various audio-related features.
- `Explicit`: Whether the song contains explicit content.

The dataset is loaded from Kaggle and cleaned for analysis.

### Files

- **`sistem_rekomendasi_musik (3).py`**: Python script that contains the core functionality for data loading, preprocessing, model building, and generating recommendations.
- **`Sistem_Rekomendasi_Musik (2).ipynb`**: Jupyter notebook that provides an interactive interface and step-by-step breakdown of the recommendation system.
- **`requirements.txt`**: List of dependencies required for the project, including libraries like `pandas`, `numpy`, `sklearn`, `seaborn`, and `tensorflow`.

## Setup Instructions

Follow the steps below to set up and run the project.

### 1. Clone the Repository

Clone the repository to your local machine:

```bash
git clone https://github.com/DavinGhani/Music-Recomendation
cd project-name
```

### 2. Install Dependencies
```
# Create a virtual environment (if you don't have one already)
python -m venv venv

# Activate the virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
