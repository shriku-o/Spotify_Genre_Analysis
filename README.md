# Introduction

Given my personal love for music, I was interested in exploring what kind of data music streaming services like Spotify and Apple Music use to determine a user's recommendations. As someone who listens to a wide range of music, I was curious how the algorithm would look at and differentiate between the music types.

Music streaming platforms collect a plethora of data beyond titles and artist names, as they measure characteristics like energy, tempo, acousticness, and danceability to help recommend music to users. These audio features help listeners differentiate between songs and connect with them on a deeper level.

**In this project, I will be exploring how songs from different genres vary in their audio features and how these characteristics compare across genres. My central question is:**

## Within the chosen genres, which audio features best separate one genre from another?

This is an important question because music genres are often subjective, with terms like “energetic” or “calm” being difficult to measure directly. By analyzing measurable audio features, we can determine whether genres truly differ in quantifiable ways and identify which characteristics are most useful for distinguishing them. This type of analysis is valuable for companies like Spotify and other music streaming platforms, as understanding music trends and listener preferences can help improve recommendation systems and create a more personalized user experience.

---

# Data Cleaning and Exploratory Data Analysis

Before beginning our analysis, we have to first ensure that the data is accurate and meaningful via cleaning. We start by removing the index column ('Unnamed: 0') because it doesn't contain anything significant. I then checked for missing values across the columns and checked if they needed to be adjusted.

A column that stood out was the 'tempo' column, as a missing value within this column would impact an important audio feature when comparing genres. Another column that I had to adjust was the 'track_id' column so that each song could only appear once in the dataset. Preventing repeated tracks ensures that they do not disproportionately influence the genre statistics.

Lastly, the new column I added to the dataset was 'duration_min', which converts the song duration from milliseconds into minutes, making the analysis easier and more practical.

After cleaning, the dataset still retained 100,000 unique songs across many different genres, making the analysis valid and well-suited for exploring the differences between genres.

---

# 📊 Univariate Analysis (Interactive Visualizations)

To explore the distribution of key audio features, interactive Plotly histograms were created. These allow zooming, hovering, and deeper inspection of the data.

<iframe src="assets/danceability_distribution.html" width="800" height="600" frameborder="0"></iframe>

<iframe src="assets/energy_distribution.html" width="800" height="600" frameborder="0"></iframe>

---

# 🎨 Bivariate Analysis

To better understand relationships between audio features and genres, bivariate visualizations were created.

## 🎵 Danceability vs Energy Across Genres

This scatter plot shows how songs cluster based on danceability and energy, with coloring by genre.

<iframe src="assets/danceability_vs_energy.html" width="800" height="600" frameborder="0"></iframe>

---

## 🎭 Valence Distribution Across Selected Genres

This boxplot compares the emotional tone (valence) across pop, rock, jazz, and classical music.

<iframe src="assets/valence_by_genre.html" width="800" height="600" frameborder="0"></iframe>

---

# ⚠️ Notes on Viewing

If the visualizations do not render directly in GitHub:

- Open the `.html` files in the repository
- Or download and open them in your browser
