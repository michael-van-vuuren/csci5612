---
title: Clustering
type: docs
prev: models/pca
next: models/arm
math: true
weight: 2
---

## Overall Goal

Use clustering on PCA-reduced social media metric data in order to discover clusters that coincide with Spotify streaming rank.

## Overview

Clustering is an unsupervised machine learning technique that groups data points in a dataset into distinguishable clusters. In general, clustering algorithms work by comparing the distances of points to each other with a selected distance metric. Data points that are close to each other can form a cluster. Data points that are further apart form distinct clusters. 

Distance metrics are essential in clustering because they determine how similarity between data points is measured, directly influencing how clusters are formed. Algorithms like KMeans, Hierarchical Clustering, and DBSCAN rely on distance calculations to group similar points together. Common metrics include Euclidean distance, which measures straight-line proximity, and Manhattan distance, which sums absolute differences along each dimension. Cosine similarity is often used when direction matters more than magnitude, such as in text or high-dimensional data. The choice of metric affects cluster shape, density, and separation, making it crucial to select one that aligns with the data’s structure.

![DistanceMetrics](/images/kmeans/DistanceMetrics.png)

[Image source](https://medium.com/@jodancker/a-brief-introduction-to-distance-measures-ac89cbd2298)

There are numerous methods for clustering. In the following sections, KMeans, Hierarchical, and DBSCAN clustering will be compared, then applied to the 3D Kernel PCA dataset from the PCA section.

## Comparison of Methods

![ClusteringExamples](/images/kmeans/ClusteringExamples.png)

[Image source](https://medium.com/@sina.nazeri/comparing-the-state-of-the-art-clustering-algorithms-1e65a08157a1)

### KMeans Clustering

*Clustering via centroids and partitioning*

**Steps:**
1. Select number of clusters $k$ 
2. Randomly select $k$ distinct datapoints as initial centroids
3. For each remaining point:
>Assign the point to the cluster with the closest centroid
4. Recalculate the centroid of each cluster as the mean of all points in that cluster
5. Repeat steps 3 & 4 for $n$ iterations or until centroids stop changing
6. Choose the clustering that most evenly distributes the variance among clusters

| **Pros** | **Cons** |
|----------|---------|
| Simple to understand | Works well when clusters are spherical, but fails on more complex shapes |
| Generally good at identifying clusters | Sensitive to outliers |
| Good time complexity | Number of clusters needs to be chosen beforehand |

### Hierarchical Clustering

*Clustering via similarity and hierarchy*

**Steps:**
1. Start with each data point as its own cluster
2. Compute the distance between all clusters (using a linkage method like single, complete, or average)
3. Merge the two closest clusters
4. Repeat steps 2 & 3 until all points belong to a single cluster or the desired number of clusters is reached 
5. Use a dendrogram to decide the optimal number of clusters by cutting at an appropriate height (threshold)

| **Pros** | **Cons** |
|----------|---------|
| Simple to understand | High dimensionality |
| Produces insightful dendrogram visualization | Higher time complexity |
| Number of clusters is discovered | Hard to scale to more features and samples |


### DBSCAN Clustering

*Clustering via density*

**Steps:**
1. Select two parameters:
>Epsilon ($\epsilon$): Defines the neighborhood radius\
>Minimum Samples ($m$): Minimum number of points required to form a dense region
2. Pick an unvisited point and check if it has at least $m$ neighbors within $\epsilon$
>If yes: Start a new cluster and expand it by adding all density-reachable points\
>If no: Label it as noise
3. Repeat step 2 until all points are visited
4. The result is a set of arbitrarily shaped clusters with noise points excluded

| **Pros** | **Cons** |
|----------|---------|
| Works well on complex cluster shapes | Less effective than KMeans on spherical cluster shapes |
| Ignores noise | Behavior determined by \( \epsilon \) and \( m \) |
| Number of clusters is discovered | Less scalable than KMeans |

## KMeans Clustering

### Data Preparation

In the following section, KMeans is applied to the 3D Kernel PCA dataset from the PCA section. The labels `All Time Rank` and `All Time Rank Bin` are saved for comparison to the clustering results. Each principal component is a quantitative feature, so they can be used in KMeans. We normalize each then compute silhouette scores for increasing values of $k$. The $k$'s to be visualized are the highest three from the silhouette score plot ($k$=[2, 3, 4]). We use Scikit-learn to perform KMeans.

>[!NOTE]
>Source code can be found here:\
>[github.com/michael-van-vuuren/csci5612-workspace/models/unsupervised/KMeans.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/main/models/unsupervised/KMeans.ipynb)

**Starting Data:**
![PreKMeansReady](/images/kmeans/PreKMeansReady.png)

**KMeans-Ready Data:**
![KMeansReady](/images/kmeans/KMeansReady.png)

**Silhouette Scores:**
![SilhouetteScores](/images/kmeans/SilhouetteScores.png)

### KMeans Visualizations

>[!TIP]
>For large & interactive visualizations, click the `{{< icon "newtab" >}} View interactive plot` buttons.

#### KMeans Clusters for Selected k-values

We can see from the KMeans scatterplots for different $k$ and the comparison scatterplots below that KMeans does a decent job at finding clusters that match up with our labels `All Time Rank` and `All Time Rank Bin`. For $k$=4, It appears that merging clusters 2 & 4 would coincide with low `All Time Rank Bin`, and merging clusters  1 & 3 would coincide with high `All Time Rank Bin`.

{{< tabs items="Two Clusters (k=2),Three Clusters (k=3),Four Clusters (k=4)" >}}
  {{< tab >}}

  ![2Means](/images/kmeans/2Means.png)

  {{< cards >}}
    {{< card link="/plots/3DClusteredPCAData(k=2).html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
  {{< tab >}}

  ![3Means](/images/kmeans/3Means.png)

  {{< cards >}}
    {{< card link="/plots/3DClusteredPCAData(k=3).html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
  {{< tab >}}

  ![4Means](/images/kmeans/4Means.png)

  {{< cards >}}
    {{< card link="/plots/3DClusteredPCAData(k=4).html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
{{< /tabs >}}

#### Comparison to Labels

{{< cards >}}
  {{< card link="" title="All Time Rank" image="/images/pca/RBFKernelPCAColoredbyAllTimeRank_3D.png" >}}
  {{< card link="" title="Binned All Time Rank" image="/images/pca/RBFKernelPCAColoredbyAllTimeRankBin_3D.png" >}}
{{< /cards >}}

## Hierarchical Clustering

### Data Preparation

In the following section, hierarchical clustering is applied to the 3D Kernel PCA dataset from the PCA section. Instead of applying hierarchical clustering to the entire dataset, around 1500 rows were randomly sampled from the dataset, because using all the rows was too performance intensive, and resulted in unappealing dendrograms. The labels `All Time Rank` and `All Time Rank Bin` are saved for comparison to the clustering results. Each principal component is a quantitative feature, so they can be used in hierarchical clustering. We normalize each then use a SciPy to create a linkage matrix and dendrogram.

>[!NOTE]
>Source code can be found here:\
>[github.com/michael-van-vuuren/csci5612-workspace/models/unsupervised/Hierarchical.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/main/models/unsupervised/Hierarchical.ipynb)

**Starting Data:**
![PreHierarchicalClusteringReady](/images/kmeans/PreKMeansReady.png)

**Hierarchical Clustering-Ready Data:**
![HierarchicalClusteringReady](/images/kmeans/KMeansReady.png)

### Hierarchical Visualizations

>[!TIP]
>For large & interactive visualizations, click the `{{< icon "newtab" >}} View interactive plot` buttons.

#### Hierarchical Clusters

We can see from the dendrogram below with a threshold value of 0.5 that there is one large red group and some smaller separate groups. We can use scatterplots to visualize which groups map to which data points.

![Dendrogram](/images/hierarchical/Dendrogram.png)

We can see from the scatterplots below that hierarchical clustering does a good job at finding clusters that match up with our labels `All Time Rank` and `All Time Rank Bin`. It appears that merging the pink (red in the dendrogram) and gray (green in the dendrogram) clusters would coincide with low `All Time Rank Bin`, and merging the rest of the clusters would coincide with high `All Time Rank Bin`.

{{< tabs items="Hierarchical Clustering (distance threshold=0.5)" >}}
  {{< tab >}}

  ![3DHierarchicalClustering(DistanceThreshold=0.5)](/images/hierarchical/3DHierarchicalClustering(DistanceThreshold=0.5).png)

  {{< cards >}}
    {{< card link="/plots/3DHierarchicalClustering(DistanceThreshold=0.5).html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
{{< /tabs >}}

#### Comparison to Labels

{{< cards >}}
  {{< card link="" title="All Time Rank" image="/images/pca/RBFKernelPCAColoredbyAllTimeRank_3D.png" >}}
  {{< card link="" title="Binned All Time Rank" image="/images/pca/RBFKernelPCAColoredbyAllTimeRankBin_3D.png" >}}
{{< /cards >}}

## DBSCAN Clustering

### Data Preparation

In the following section, density based (DBSCAN) clustering is applied to the 3D Kernel PCA dataset from the PCA section. The labels `All Time Rank` and `All Time Rank Bin` are saved for comparison to the clustering results. Each principal component is a quantitative feature, so they can be used in density based clustering. We normalize each then use Scikit-learn to perform DBSCAN.

>[!NOTE]
>Source code can be found here:\
>[github.com/michael-van-vuuren/csci5612-workspace/models/unsupervised/DBSCAN.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/main/models/unsupervised/DBSCAN.ipynb)

**Starting Data:**
![PreDBSCANReady](/images/kmeans/PreKMeansReady.png)

**DBSCAN-Ready Data:**
![DBSCANReady](/images/kmeans/KMeansReady.png)

### DBSCAN Visualizations

>[!TIP]
>For large & interactive visualizations, click the `{{< icon "newtab" >}} View interactive plot` buttons.

#### DBSCAN Clusters

DBSCAN was applied with a minimum sample size $m$ of 50, and epsilon values of $\epsilon$=[0.05, 0.10, 0.15]. The reason a high minimum sample size and low epsilons (low neighborhood radius) were used is because in our dataset, most of the songs with lower `All Time Rank` scores are grouped at a higher density tip, while most of the songs with higher `All Time Rank` scores are grouped with lower density. Therefore, we can use DBSCAN to identify the high density tip as a cluster, then use the noise as the other cluster to find clusters that match up with `All Time Rank Bin`. The results below show that this is quite effective, especially when $\epsilon$=0.10.

{{< tabs items="DBSCAN (eps=0.05),DBSCAN (eps=0.10),DBSCAN (eps=0.15)" >}}
  {{< tab >}}

  ![3DClusteredPCAData(DBSCANeps=0.05min_samples=50)](/images/dbscan/3DClusteredPCAData(DBSCANeps=0.05min_samples=50).png)

  {{< cards >}}
    {{< card link="/plots/3DClusteredPCAData(DBSCANeps=0.05min_samples=50).html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
  {{< tab >}}

  ![3DClusteredPCAData(DBSCANeps=0.1min_samples=50)](/images/dbscan/3DClusteredPCAData(DBSCANeps=0.1min_samples=50).png)

  {{< cards >}}
    {{< card link="/plots/3DClusteredPCAData(DBSCANeps=0.1min_samples=50).html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
  {{< tab >}}

  ![3DClusteredPCAData(DBSCANeps=0.15min_samples=50)](/images/dbscan/3DClusteredPCAData(DBSCANeps=0.15min_samples=50).png)

  {{< cards >}}
    {{< card link="/plots/3DClusteredPCAData(DBSCANeps=0.15min_samples=50).html" title="View interactive plot" icon="newtab" subtitle="" target="_blank" absolute="true" >}}
  {{< /cards >}}
  {{< /tab >}}
{{< /tabs >}}

#### Comparison to Labels

{{< cards >}}
  {{< card link="" title="All Time Rank" image="/images/pca/RBFKernelPCAColoredbyAllTimeRank_3D.png" >}}
  {{< card link="" title="Binned All Time Rank" image="/images/pca/RBFKernelPCAColoredbyAllTimeRankBin_3D.png" >}}
{{< /cards >}}

## Conclusions

By applying clustering to PCA-reduced social media metric data, we aimed to discover patterns that align with Spotify streaming rank. The analysis compared KMeans, Hierarchical, and DBSCAN clustering to see which method best identified meaningful clusters.

KMeans showed some alignment with Spotify’s All Time Rank by forming distinct groups, but required specifying the number of clusters in advance. Hierarchical clustering provided a clear dendrogram, revealing how tracks could be merged based on similarity, but was computationally expensive for the full dataset.

DBSCAN proved to be the most effective method. Since DBSCAN identifies clusters based on density, it naturally aligned with the structure of the dataset: songs with high-density regions tended to have lower All Time Rank scores, indicating higher popularity, while those in lower-density regions had higher All Time Rank scores, representing less popular tracks. By adjusting the neighborhood radius ($\epsilon$), DBSCAN was able to isolate these patterns effectively, making it the best approach for uncovering clusters that reflect streaming rank.