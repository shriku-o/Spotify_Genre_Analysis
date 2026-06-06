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

## Univariate Analysis
To understand the individual behavior of our primary audio metrics, we examined the isolated distributions of specific song characteristics across the dataset.

<iframe src="assets/danceability_distribution.html" width="800" height="600" frameborder="0"></iframe>

The danceability distribution shows a slightly left-skewed bell curve centered roughly around 0.6. This indicates that a majority of the tracks in the dataset maintain a moderate-to-high danceability rating, with very few tracks falling below 0.2.

<iframe src="assets/energy_distribution.html" width="800" height="600" frameborder="0"></iframe>


The energy distribution shows a distinct left-skew, revealing that streaming platforms host a high concentration of high-energy tracks. There is a notable spike approaching the 0.8 to 0.9 range, showing that high-intensity music heavily populates modern genres.


## Bivariate Analysis
To explore how these individual features interact with one another, we analyzed pairs of columns to look for underlying associations and patterns.

<iframe src="assets/danceability_vs_energy.html" width="800" height="600" frameborder="0"></iframe>


This scatter plot maps the relationship between danceability and energy across tracks. While there is a dense cluster of tracks possessing both high energy and high danceability, the overall distribution suggests a moderate positive association, indicating that highly energetic tracks frequently—but not always—lend themselves well to dancing.

<iframe src="assets/valence_by_genre.html" width="800" height="600" frameborder="0"></iframe>

This visualization explores how music "valence" (the emotional positivity of a track) varies contextually across different musical genres. The variance in distributions highlights clear shifts in emotional tone, showing that track features act as measurable boundaries separating one genre from another.


## Interesting Aggregates
By grouping the dataset by genre and looking at aggregate statistics, we can pinpoint exactly which features act as the strongest differentiators between musical styles.

| track_genre   |   danceability |   energy |   valence |   acousticness |
|:--------------|---------------:|---------:|----------:|---------------:|
| acoustic      |          0.551 |    0.465 |     0.44  |          0.536 |
| afrobeat      |          0.669 |    0.716 |     0.702 |          0.258 |
| alt-rock      |          0.53  |    0.763 |     0.514 |          0.12  |
| alternative   |          0.583 |    0.712 |     0.496 |          0.157 |
| ambient       |          0.373 |    0.267 |     0.174 |          0.742 |
| anime         |          0.539 |    0.712 |     0.452 |          0.237 |
| black-metal   |          0.297 |    0.888 |     0.192 |          0.021 |
| bluegrass     |          0.533 |    0.559 |     0.648 |          0.529 |
| blues         |          0.571 |    0.6   |     0.611 |          0.392 |
| brazil        |          0.564 |    0.626 |     0.466 |          0.329 |

This aggregate table highlights the structural differences between genres. By evaluating the mean values of features like tempo, energy, and danceability side-by-side, we can mathematically track how the algorithm categorizes sonic profiles to distinguish a high-tempo electronic track from a low-energy acoustic track.

---

# Assessment of Missingness


## NMAR Analysis

I believe that the release_date column contains a strong NMAR (Not Missing At Random) component. While our subsequent permutation tests show that its missingness correlates with observed features like popularity, the underlying data-generating process suggests that the missingness ultimately relies on unobserved values or the missing values themselves.

For instance, many missing release dates occur in underground tracks, independent mixtapes, or archival recordings where the precise release date was never formally logged by the creator or has been lost to time. Because the date is missing precisely because the historical date itself is unknown or unrecorded, it fits the definition of NMAR.

To shift this mechanism from NMAR to MAR (Missing At Random), we would want to obtain additional data regarding the distribution chain. Specifically, adding columns for Distribution Type (e.g., major record label vs. independent self-release via aggregators like DistroKid) or Catalog Registration Status would help explain why the metadata is absent, effectively making the missingness explainable by observed variables.

## Missing Dependency

To rigorously evaluate what factors influence whether a song's release date is missing, we performed two permutation tests tracking the relationship between the missingness indicator (release_date_missing) and other track characteristics.

## Permutation Test 1: Release Date Missingness vs. Popularity 
- Null Hypothesis (H_0): The distribution of popularity is the same regardless of whether release_date is missing or present.
- Alternative Hypothesis (H_1): The distribution of popularity is different when release_date is missing compared to when it is present.
- Test Statistic: The difference in mean popularity between the two groups (Missing Mean - Not Missing Mean).
- Significance Level: 0.05
  
## Result and Interpretation:
Running this permutation test yielded an empirical p-value of 0.0. Out of all 1,000 shuffled trials, not a single permuted difference came close to our extreme observed difference. Therefore, we reject the null hypothesis at the 5% significance level. We conclude that the missingness of release_date depends on popularity. This confirms that the missingness of this column is MAR conditional on popularity, as less mainstream or obscure tracks are systematically less likely to have complete metadata.


<iframe src="missingness_popularity.html" width="800" height="600" frameborder="0"></iframe> 


## Permutation Test 2: Release Date Missingness vs. Tempo 
- Null Hypothesis (H_0): The distribution of tempo is the same regardless of whether release_date is missing or present.Alternative
- Hypothesis (H_1): The distribution of tempo is different when release_date is missing compared to when it is present.
- Test Statistic: The difference in mean tempo between the two groups.
- Significance Level: 0.05
  
## Result and Interpretation:
Intuitively, a song's musical speed (Beats Per Minute) should have no structural impact on whether an administrative metadata field is filled out. The permutation test confirmed this intuition, yielding a high p-value (significantly greater than 0.05). Consequently, we fail to reject the null hypothesis. We conclude that the missingness of release_date does not depend on tempo.

<iframe src="permutation_dist_popularity.html" width="800" height="600" frameborder="0"></iframe>


---


# Hypothesis Testing

To evaluate whether genres are truly separated by quantifiable audio characteristics, we performed a statistical hypothesis test analyzing the energy feature between two major contemporary genres: Pop and Rock.
# Stating the Hypotheses
- Null Hypothesis (H_0): In the underlying population of music, the true mean energy of Pop tracks is equal to the true mean energy of Rock tracks. Any observed difference in our sample is entirely due to random sampling variation.
- Alternative Hypothesis (H_1): The true mean energy of Pop tracks is different from the true mean energy of Rock tracks. (Two-tailed test).
- Significance Level: We select 0.05 as our threshold for statistical significance.
# Justification of Choices
- Choice of Test: We chose a Permutation Test rather than a traditional parametric two-sample t-test. Spotify's audio features (like energy) are algorithmically engineered metrics bounded strictly between 0 and 1. Because their distributions can be highly skewed or multi-modal, a permutation test is an ideal non-parametric approach. It does not assume mathematical normality, relying instead on the empirical distribution of our actual data.
- Choice of Test Statistic: The difference in sample means (Mean_pop) - (Mean_rock) is an intuitive and mathematically sound test statistic. It directly answers our question by quantifying the average distance in intensity between the two groups.
# Results and Test Interpretation:
After running 1,000 permutations where genre labels were randomly shuffled across tracks, we observed the following outcomes:
- Observed Difference:
<iframe src="hypothesis_test_energy.html" width="800" height="600" frameborder="0"></iframe>
- Resulting p-value: 0.0

Because our calculated p-value is less than our significance level of 0.05, we reject the null hypothesis.

# Statistical Conclusion
The data provide strong evidence that the energy levels of Pop and Rock songs are systematically different. The permutation test demonstrates that the lower average energy score observed in Pop relative to Rock is highly unlikely to be the result of random sampling luck. While these results strongly support a structural distinction between the genres, we cannot conclude with 100% absolute certainty that this represents an unyielding rule of musicology. Because our dataset consists of a fixed snapshot of Spotify's catalog rather than a randomized controlled trial, these statistical findings suggest a meaningful correlation in how these genres are engineered and classified, rather than an absolute truth.



