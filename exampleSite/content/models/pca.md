---
title: PCA
type: docs
prev: models/_index
next: models/clustering
weight: 1
---

## Principal Component Analysis (PCA)

### Definition

text

### Methodology

**Starting Data:**
![PrePCAReady](/images/pca/PrePCAReady.png)

**PCA-Ready Data:**
![PCAReady](/images/pca/PCAReady.png)

### PCA Visualizations

>[!TIP]
>For large & interactive visualizations, click the `{{< icon "newtab" >}} View interactive plot` buttons.

#### Two Principal Components

{{< tabs items="Colored by All Time Rank,Colored by Binned All Time Rank" >}}
  {{< tab >}}
  Spotify song ranks range from **1** (highest) to **~4500** (lowest) and are based on numerous metrics, such as streams and listening time.

  ![PCAColoredbyAllTimeRank_2D](/images/pca/PCAColoredbyAllTimeRank_2D.png)

  {{< cards >}}
    {{< card link="/plots/PCAColoredbyAllTimeRank_2D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
  {{< tab >}}
  The 50% of songs with the highest ranks are colored in green. The 50% of songs with the lowest ranks are colored in black.

  ![PCAColoredbyAllTimeRankBin_2D](/images/pca/PCAColoredbyAllTimeRankBin_2D.png)

  {{< cards >}}
    {{< card link="/plots/PCAColoredbyAllTimeRankBin_2D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
{{< /tabs >}}

#### Three Principal Components

{{< tabs items="Colored by All Time Rank,Colored by Binned All Time Rank" >}}
  {{< tab >}}
  Spotify song ranks range from **1** (highest) to **~4500** (lowest) and are based on numerous metrics, such as streams and listening time.

  ![PCAColoredbyAllTimeRank_3D](/images/pca/PCAColoredbyAllTimeRank_3D.png)

  {{< cards >}}
    {{< card link="/plots/PCAColoredbyAllTimeRank_3D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
  {{< tab >}}
  The 50% of songs with the highest ranks are colored in green. The 50% of songs with the lowest ranks are colored in black.

  ![PCAColoredbyAllTimeRankBin_3D](/images/pca/PCAColoredbyAllTimeRankBin_3D.png)

  {{< cards >}}
    {{< card link="/plots/PCAColoredbyAllTimeRankBin_3D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
{{< /tabs >}}

#### Eigenvalues, Variance, & Loadings

![PCAVarianceLoadings](/images/pca/PCAVarianceLoadings.png)

### Kernel PCA Visualizations

#### Two Principal Components

{{< tabs items="Colored by All Time Rank,Colored by Binned All Time Rank" >}}
  {{< tab >}}
  Spotify song ranks range from **1** (highest) to **~4500** (lowest) and are based on numerous metrics, such as streams and listening time.

  ![RBFKernelPCAColoredbyAllTimeRank_2D](/images/pca/RBFKernelPCAColoredbyAllTimeRank_2D.png)

  {{< cards >}}
    {{< card link="/plots/RBFKernelPCAColoredbyAllTimeRank_2D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
  {{< tab >}}
  The 50% of songs with the highest ranks are colored in green. The 50% of songs with the lowest ranks are colored in black.

  ![RBFKernelPCAColoredbyAllTimeRankBin_2D](/images/pca/RBFKernelPCAColoredbyAllTimeRankBin_2D.png)

  {{< cards >}}
    {{< card link="/plots/RBFKernelPCAColoredbyAllTimeRankBin_2D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
{{< /tabs >}}

#### Three Principal Components

{{< tabs items="Colored by All Time Rank,Colored by Binned All Time Rank" >}}
  {{< tab >}}
  Spotify song ranks range from **1** (highest) to **~4500** (lowest) and are based on numerous metrics, such as streams and listening time.

  ![RBFKernelPCAColoredbyAllTimeRank_3D](/images/pca/RBFKernelPCAColoredbyAllTimeRank_3D.png)

  {{< cards >}}
    {{< card link="/plots/RBFKernelPCAColoredbyAllTimeRank_3D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
  {{< tab >}}
  The 50% of songs with the highest ranks are colored in green. The 50% of songs with the lowest ranks are colored in black.

  ![RBFKernelPCAColoredbyAllTimeRankBin_3D](/images/pca/RBFKernelPCAColoredbyAllTimeRankBin_3D.png)

  {{< cards >}}
    {{< card link="/plots/RBFKernelPCAColoredbyAllTimeRankBin_3D.html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
{{< /tabs >}}