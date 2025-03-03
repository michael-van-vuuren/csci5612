---
title: Clustering
type: docs
prev: models/pca
next: models/arm
math: true
weight: 2
---

## Definition

Clustering is an unsupervised machine learning technique that groups data points in a dataset into distinguishable clusters. In general, clustering algorithms work by comparing the distances of points to each other. Data points that are close to each other can form a cluster. Data points that are further apart form distinct clusters. There are numerous methods for clustering. In the following sections, KMeans, Hierarchical, and DBSCAN clustering will be compared, then applied to the 3D Kernel PCA dataset from the PCA section.

## Comparison of Methods

### KMeans Clustering

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

>[!NOTE]
>Source code can be found here:\
>[github.com/michael-van-vuuren/csci5612-workspace/models/unsupervised/Hierarchical.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/main/models/unsupervised/Hierarchical.ipynb)

**Starting Data:**
![PreHierarchicalClusteringReady](/images/kmeans/PreKMeansReady.png)

**Hierarchical Clustering-Ready Data:**
![HierarchicalClusteringReady](/images/kmeans/KMeansReady.png)

![Dendrogram](/images/hierarchical/Dendrogram.png)

### Hierarchical Visualizations

>[!TIP]
>For large & interactive visualizations, click the `{{< icon "newtab" >}} View interactive plot` buttons.

#### Hierarchical Clusters

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
