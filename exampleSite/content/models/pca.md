---
title: PCA
type: docs
prev: models/_index
next: models/clustering
weight: 1
---

## Principal Component Analysis

### Definition

text

### Methodology

text

### Visualizations

#### Two Principal Components

{{< tabs items="Colored by All Time Rank,Colored by High Playlist Probability" >}}
  {{< tab >}}
  Spotify song ranks range from **1** to **~4500** and are based on numerous metrics, such as streams and listening time.

  ![PCA(ColoredbyAllTimeRank)_2D](images/pca/PCA(ColoredbyAllTimeRank)_2D.png)

  {{< cards >}}
    {{< card link="/plots/PCA(ColoredbyAllTimeRank)_2D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}

  {{< tab >}}
  Points are divided into groups with low/high (**0**/**1**) probability of being included on a playlist.

  ![PCA(ColoredbyHighPlaylistProbability)_2D](images/pca/PCA(ColoredbyHighPlaylistProbability)_2D.png)

  {{< cards >}}
    {{< card link="/plots/PCA(ColoredbyHighPlaylistProbability)_2D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
{{< /tabs >}}

#### Three Principal Components

{{< tabs items="Colored by All Time Rank,Colored by High Playlist Probability" >}}
  {{< tab >}}
  Spotify song ranks range from **1** to **~4500** and are based on numerous metrics, such as streams and listening time.

  ![PCA(ColoredbyAllTimeRank)_3D](images/pca/PCA(ColoredbyAllTimeRank)_3D.png)

  {{< cards >}}
    {{< card link="/plots/PCA(ColoredbyAllTimeRank)_3D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}

  {{< tab >}}
  Points are divided into groups with low/high (**0**/**1**) probability of being included on a playlist.

  ![PCA(ColoredbyHighPlaylistProbability)_3D](images/pca/PCA(ColoredbyHighPlaylistProbability)_3D.png)

  {{< cards >}}
    {{< card link="/plots/PCA(ColoredbyHighPlaylistProbability)_3D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
{{< /tabs >}}

### Variance and Loadings

![EVR_CUMEVR_Loadings](images/pca/EVR_CUMEVR_Loadings.png)