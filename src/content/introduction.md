---
title: Introduction
type: introduction
---

## Topic

Streaming platforms have changed how songs gain popularity and stay relevant. In the past, record labels decided which artists and songs would reach mainstream audiences. They had a monopoly on song distribution through radio airplay, physical media, and promotional campaigns, which made it difficult for independent artists to gain popularity. 

With the rise of social media, music promotion has become easy. In addition, the rise of platforms like Spotify, Apple Music, and YouTube have allowed artists to release and distribute music to a global audience, without needing to go through a record label. Social media has emerged as a powerful force in shaping which songs gain popularity. TikTok, in particular, has become a crucial driver of viral music success, where songs can gain massive popularity through challenges, dance trends, and posts. The stats show that social media trends directly impact chart performance. In 2024, 13 out of the 16 songs that reached No.1 on the U.S. Billboard Hot 100 were linked to TikTok trends ([Music Business Worldwide](https://www.musicbusinessworldwide.com/tiktok-reveals-its-top-songs-of-2024-says-that-13-of-16-no-1-hits-in-the-us-this-year-are-linked-to-trends-on-its-platform/)).

![23](/images/intro-eda/23.png)
*Paradigm shift in music distribution and consumption ([The Honest Broker](https://www.honest-broker.com/p/results-of-my-survey-who-deserves))*

Since the rise of digital streaming, playlists on platforms like Spotify or Apple Music have become a popular way to listen to and share music. The inclusion of a song on a playlist helps it reach larger audiences, stay relevant over time, and generate consistent streams. This project explores whether a song’s likelihood of being added to a playlist can be predicted using  social media data and metadata, without relying on direct streaming numbers. For example, if a song has a lot of "Shazams" (music recognition app), what can be said about its likelihood of being added to a playlist?

![Playlists](/images/intro-eda/playlists.png)

*Popular playlists on Spotify ([Spotify](https://www.spotify.com))*

The first part of the project involves using unsupervised methods to gain an understanding of the dataset:

- **Principal Component Analysis (PCA)** simplifies the dataset by highlighting the most important features. This helps reveal patterns that are easier to analyze and use in later steps.

- **Clustering** techniques such as K-Means and density-based clutering are applied to group songs based on similarities in social media and streaming data. These unsupervised clustering models use the PCA data from the previous section. The clusters reveal the natural groupings of low and high playlist likelihood songs.

- **Association rule mining** is used to reveal relationships between social media behaviors and song popularity, which helps with understanding the factors associated with a song's popularity.

The core of the project involves building models to predict playlist inclusion. A custom target variable (discussed in the section [Creating Target Variable {{< icon "newtab" >}}](https://michael-van-vuuren.github.io/csci5612/eda/#creating-target-variable) under the EDA tab) was created to reflect playlist inclusion, avoiding streaming numbers to reduce bias. Three different methods are explored:

- **Naive Bayes** predicts playlist likelihood based on social media and metadata.

- **Decision Trees** compare the impact of different input types by testing three versions: metadata alone, metadata with social media, and metadata with streaming statistics.

- **Logistic Regression** serves as a benchmark model, using the same features as Naive Bayes to evaluate classification performance.

As a bonus, genre prediction is explored, since genre is a useful feature for playlist models but is often missing in datasets. This section includes:

- **Support Vector Machines (SVMs)** that classify songs by genre based solely on their lyrics.

- An **ensemble model** that uses gradient boosting to improve on the genre prediction performance of the SVMs by combining multiple decision trees in series.

### Why?

Songs that stay popular over time are more likely to succeed. Getting on playlists helps songs reach more people and get more streams. Songs that do well on playlists have replayability. Going viral on social media like TikTok can help, but real success comes from keeping people interested. Knowing what makes a song good for playlists can help artists and music platforms make better choices about promoting and sharing music.

### Who is affected?

- **Artists and Producers:** Understanding what makes a song playlist-worthy can guide creative and promotion decisions.
- **Record Labels:** Predicting playlist inclusion can help with marketing strategies.
- **Streaming Platforms:** Improving recommendation algorithms based on playlist behavior can increase listening time.
- **Listeners:** Playlists with good songs enhance the user experience.

### Related work

Several studies and industry reports have explored factors influencing song popularity, such as the impact of streaming algorithms, social media trends, and audio features. 

**Hit Song Prediction:** Machine learning has been applied to predict a song’s success based on audio features, lyrics, and listener engagement. A study from "Frontiers in Artificial Intelligence" found that neural data significantly improves hit song classification accuracy ([Frontiers](https://www.frontiersin.org/journals/artificial-intelligence/articles/10.3389/frai.2023.1154663/full)).

**Social Media Influence:** Platforms like TikTok and YouTube have reshaped music discovery, often determining which songs become hits. Research from "Neuroscience News" found that social networks can increase music popularity prediction accuracy by 50% ([Neuroscience News](https://neurosciencenews.com/social-connections-music-26294/)).

This project builds upon these studies by integrating streaming statistics, social media engagement metrics, and metadata to predict a song's probability of being added to playlists, an important metric to determine the long-term engagement of a song.

## Primary Questions
![Questions](/images/intro-eda/goals.png)

The first two questions are focused on the main topic of examining the relationship between social media and music streaming. The third question is a bonus section that explores using text classification models to predict a song's genre from its lyrics, which would be useful for future music classification models. 

Below are the pages that address each question:

|  **Question**  |**Pages**|
|----------------|---------|
|     **1.**     | [Naive Bayes Classifiers {{< icon "newtab" >}}](https://michael-van-vuuren.github.io/csci5612/models/nb/)<br>[Decision Trees {{< icon "newtab" >}}](https://michael-van-vuuren.github.io/csci5612/models/dt/)<br>[Logistic Regression Classifier {{< icon "newtab" >}}](https://michael-van-vuuren.github.io/csci5612/models/regression/) |
|     **2.**     | [ARM {{< icon "newtab" >}}](https://michael-van-vuuren.github.io/csci5612/models/arm/) |
|     **3.**     | [Support Vector Machines {{< icon "newtab" >}}](https://michael-van-vuuren.github.io/csci5612/models/svm/)<br>[Ensemble Learner {{< icon "newtab" >}}](https://michael-van-vuuren.github.io/csci5612/models/ensemble/) |

The data is also explored in the [Exploratory Data Analysis (EDA) {{< icon "newtab" >}}](https://michael-van-vuuren.github.io/csci5612/eda/#exploratory-data-analysis-eda), [Principal Component Analysis (PCA) {{< icon "newtab" >}}](https://michael-van-vuuren.github.io/csci5612/models/pca/), and [Clustering {{< icon "newtab" >}}](https://michael-van-vuuren.github.io/csci5612/models/clustering/) sections.

## Additional Questions

Some of these questions are left unanswered for a future project.

1. What features correlate with high playlist inclusion probability?
2. How do social media engagement metrics (e.g. TikTok posts, YouTube likes) influence playlist-worthiness?
3. Does playlist inclusion probability differ across genres and regions?
4. How do streaming performance metrics (e.g. Spotify popularity, YouTube views) relate to playlist inclusion?
5. Does song length affect playlist-worthiness?
6. Do newer songs have a higher probability of being included in playlists compared to older tracks?
7. What impact does a song’s initial viral success have on its long-term playlist performance?
8. What differences exist in playlist curation strategies across streaming platforms?
9. How accurately can machine learning models predict whether a song will be added to a playlist?
10. What characteristics distinguish user-generated playlists from editorially curated ones?

## Visualizations

>[!NOTE]
>To see how these visualizations were created, visit [here](https://michael-van-vuuren.github.io/csci5612/eda/#exploratory-data-analysis-eda).

**1. Number of Track Registrations by Country** 

The dataset contains registration country information for each of the tracks, which approximitely indicates the region of the artist who produced the track. A map showing how many tracks are registered in each country is useful to get an idea of where the artists of the most popular songs are located and how generalizable to decisions guided by the data are. Based on this, more data can be gathered for countries that are lacking in registration counts.

![25](/images/intro-eda/25.png)

**2. Distributions of Playlist Probability Scores by Days Since Release**

Understanding how release date affects the likelihood of a song being included in a playlist is important. From the visualization below, it is clear that older songs have a greater likelihood of being included in playlists.

![26](/images/intro-eda/26.png)

**3. Streaming and Social Media Song Platform Popularity**

The pie charts below show which platforms generate the most streams and views for all the songs in the dataset. In terms of streaming, Spotify dominates in total streams. In terms of social media, TikTok dominates in total views.

![27](/images/intro-eda/27.png)

**4. Playlist Probability vs Song Length**

Understanding how the length of a song affects its playlist-worthiness is useful. From the scatterplot below, it seems that there is a possible positive correlation between playlist-worthiness and song length

![28](/images/intro-eda/28.png)

**5. Correlations Between Streaming and Social Media Metrics**

It is important to be aware of how different features in the dataset correlate with each other. Highly correlated features can be dropped so as to leave a single feature to lower dimensionality, and weakly correlated features might provide useful information when used together. The pairplot below was log-transformed because the distributions for each feature had heavy right skews. By transforming the data, it becomes easier to see the relationships between each feature, since they are more centered. Certain features like `Spotify Streams` and `Shazam Counts` appear highly correlated; others are less so. The vertical and horizontal lines visible in each plot are a result of the imputation from earlier in which missing values of a feature were replaced by the feature's median.  

![29](/images/intro-eda/29.png)

**6. Correlation Amounts Between Streaming and Social Media Metrics**

This heatmap is supplemental to the pairplot above. It provides correlation scores for each pair of metrics. Pairs that intersect at darker squares have greater correlations. 

![31](/images/intro-eda/31.png)

**7. How Playlist Probability Changes Based on Number of TikTok Posts**

This visualization identifies how the number of TikTok posts that include a song affects its probability of being included in a playlist. From the boxenplots below, the extent to which TikTok popularity affects playlist-worthiness is clear: More posts means a higher value and variance in playlist probability.

![30](/images/intro-eda/30.png)

**8. Playlist Probability by Release Year**

The line plot below shows that older songs are indeed more likely to be included in playlists. 

![32](/images/intro-eda/32.png)

**9. The Most Popular Artists**

The visualization below summarizes the top 20 most popular artists across streaming and social media platforms. Some of the results and surprising, while others are expected.

![33](/images/intro-eda/33.png)

**10. Genre Word Cloud**

The most common genres are visible in this word cloud. It appears that most of the songs are either hip hop, electronic, pop, and pop rock. This makes sense because these genres are easy to get into and are popular. 

![34](/images/intro-eda/34.png)