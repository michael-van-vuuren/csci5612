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

**Starting Data:**
![Pre_PCA](/images/pca/Pre_PCA.png)

**PCA-Ready Data:**
![PCA_Ready](/images/pca/PCA_Ready.png)

### Visualizations

>[!TIP]
>For large & interactive visualizations, click the `{{< icon "newtab" >}} View interactive plot` buttons.

#### Two Principal Components

{{< tabs items="Colored by All Time Rank,Colored by High Playlist Probability" >}}
  {{< tab >}}
  Spotify song ranks range from **1** to **~4500** and are based on numerous metrics, such as streams and listening time.

  ![PCA(ColoredbyAllTimeRank)_2D](/images/pca/PCA(ColoredbyAllTimeRank)_2D.png)

  {{< cards >}}
    {{< card link="/plots/PCA(ColoredbyAllTimeRank)_2D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}

  {{< tab >}}
  Points are divided into groups with low/high (**0**/**1**) probability of being included on a playlist.

  ![PCA(ColoredbyHighPlaylistProbability)_2D](/images/pca/PCA(ColoredbyHighPlaylistProbability)_2D.png)

  {{< cards >}}
    {{< card link="/plots/PCA(ColoredbyHighPlaylistProbability)_2D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
{{< /tabs >}}

#### Three Principal Components

{{< tabs items="Colored by All Time Rank,Colored by High Playlist Probability" >}}
  {{< tab >}}
  Spotify song ranks range from **1** to **~4500** and are based on numerous metrics, such as streams and listening time.

  ![PCA(ColoredbyAllTimeRank)_3D](/images/pca/PCA(ColoredbyAllTimeRank)_3D.png)

  {{< cards >}}
    {{< card link="/plots/PCA(ColoredbyAllTimeRank)_3D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}

  {{< tab >}}
  Points are divided into groups with low/high (**0**/**1**) probability of being included on a playlist.

  ![PCA(ColoredbyHighPlaylistProbability)_3D](/images/pca/PCA(ColoredbyHighPlaylistProbability)_3D.png)

  {{< cards >}}
    {{< card link="/plots/PCA(ColoredbyHighPlaylistProbability)_3D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
{{< /tabs >}}

### Eigenvalues, Variance, & Loadings

Top 3 Eigenvalues:

![Top_3_Eigenvalues](/images/pca/Top_3_Eigenvalues.png)

Explained Variance Ratio (EVR), Cumulative EVR, and Loadings:

![EVR_CUMEVR_Loadings](/images/pca/EVR_CUMEVR_Loadings.png)