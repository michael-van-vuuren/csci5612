---
title: Principal Component Analysis (PCA)
type: docs
prev: models/_index
next: models/clustering
math: true
weight: 1
---

## Overall Goal

Use PCA to reduce the dimensionality of the dataset so that unsupervised clustering models are more effective and easier to visualize.

## Overview

PCA is a way to simplify a dataset while retaining its most important information. In a dataset, eigenvectors represent the directions of greatest variation, while each corresponding eigenvalue indicates how much variance that direction captures. The eigenvectors are sorted in descending order based on their eigenvalues, and the top $N$ eigenvectors are retained. The dataset is then projected onto the new space formed by these eigenvectors.

For example, in a dataset with 100 features, PCA might find that three eigenvectors capture 95% of the variance. The dataset can then be mapped onto the 3D space defined by these eigenvectors, reducing it from 100 features to just 3 while still preserving most of the original information. It is important to note that these eigenvectors do not correspond to individual features from the original dataset but rather represent linear combinations of those features.

## Data Preparation

The features we use for PCA are all related to social media: `YouTube Views`, `YouTube Likes`, `TikTok Posts`, `TikTok Likes`, and `TikTok Views`. Following the PCA transformation, the data will be colored according to the continuous `All Time Rank` label and the discrete `All Time Rank Bin` label. By doing this, we are using social media metrics as our features and streaming success as our response. 

Notice that none of the columns are categorical. PCA cannot be applied to categorical variables because there is no way to measure their variance (e.g., cannot measure variance between a book and a chair). Before applying PCA to this feature-set, every column must be normalized to have a mean of 0 and a standard deviation of 1, so that variance can accurately be measured. 

>[!NOTE]
>Source code can be found here:\
>[github.com/michael-van-vuuren/csci5612-workspace/models/unsupervised/PCA.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/main/models/unsupervised/PCA.ipynb)

**Starting Data:**
![PrePCAReady](/images/pca/PrePCAReady.png)

**PCA-Ready Data:**
![PCAReady](/images/pca/PCAReady.png)

## PCA Visualizations

In the following section, PCA is applied to the PCA-Ready dataset, and the 2D and 3D PCA reduced data is shown. In both cases, the data is colored according to the continuous `All Time Rank` label and the discrete `All Time Rank Bin` label. Use the tabs to switch between the two. 

>[!TIP]
>For large & interactive visualizations, click the `{{< icon "newtab" >}} View interactive plot` buttons.

### Two Principal Components

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

### Three Principal Components

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

### Eigenvalues, Variance, and Loadings

![PCAVarianceLoadings](/images/pca/PCAVarianceLoadings.png)

We can see the following from the three charts above:

**Information Retained After PCA:**\
2D Dataset: ~85%\
3D Dataset: ~95%

**Dimensions Needed for 95% Variance:**\
To retain at least 95% of the variance in the dataset, the number of principal components required is three. This is clearly visible from the Cumulative Explained Variance Ratio bar chart above. This is determined by summing the cumulative explained variance ratios until reaching 95%.

**Top Three Eigenvalues:**\
The top three eigenvalues of the dataset are 2.42, 1.82, 0.58.

**Loadings:**\
The loadings show the weights of each feature in a given principal component. It is interesting to see that the first principal component has a strong positive association with TikTok metrics, while the second principal component has a positive association with YouTube metrics. The third principal component has a strong negative association with TikTok post count, but a slight positive association with TikTok like count and TikTok view count.

Although PCA was effective at reducing the features of the dataset, the current projection of data points onto 2D and 3D space is too cramped. It would be difficult to find a clustering algorithm that can determine whether a song has a low or a high all time rank. Therefore, in the next section, Kernel PCA is used to help spread the data points out and make the data more effective for clustering. Kernel PCA works well when the relationships between features in the underlying dataset are nonlinear. It first projects data onto a higher-dimensional space, then performs PCA in that space.

## Kernel PCA Visualizations

In the following section, Kernel PCA with a radial basis function (RBF) kernel is applied to the PCA-Ready dataset, and the 2D and 3D PCA reduced data is shown. Notice that the data is more spread and circular. These effects are due to the RBF kernel. In both cases, the data is colored according to the continuous `All Time Rank` label and the discrete `All Time Rank Bin` label. Use the tabs to switch between the two. 

### Two Principal Components

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

### Three Principal Components

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

## Conclusions

After performing PCA, we found that only three dimensions were needed to retain 95% of the variance of the original dataset. We also saw how using Kernel PCA helped separate the data points more than regular PCA. The 3D Kernel PCA dataset has a high concentration of low ranked songs on one "tip" of the data, and a high concentration of high ranked songs away from that tip. This makes it easier to create cluster boundries. Therefore, we will use it for the clustering performed in the following section. 
