---
title: Clustering
type: docs
prev: models/pca
next: models/arm
weight: 2
---

## KMeans Clustering

### Definition

text

### Methodology

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

## DBSCAN Clustering

### Definition

text

### Methodology

Same as KMeans clustering.

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

## Hierarchical Clustering

### Definition

text

### Methodology



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