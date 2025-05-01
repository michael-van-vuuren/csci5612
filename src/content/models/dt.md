---
title: Decision Tree
type: docs
prev: models/nb
next: models/regression
weight: 5
math: true
---

## Overall Goal
For the Decision Tree models, the goal is to compare how different types of features impact classification performance. Specifically, three models were trained with varying inputs:

1. Model 1 (Control): Only metadata about each song
2. Model 2: Metadata + social media data
3. Model 3: Metadata + streaming data

As with the Naive Bayes models, the task is to predict whether a song is likely to be included in a playlist. For more information about how the target label was calculated, visit the Naive Bayes tab.

## Overview
### Definition
A Decision Tree (DT) classifier is a supervised machine learning model that uses a tree-like structure to make decisions. Each internal node represents a decision point based on the value of a chosen feature. These decision points split into branches, which can represent binary outcomes (true/false) or different categories, depending on the type of feature. The leaf nodes of the tree represent predictions, typically the majority class of samples that fall into that leaf.

![DT](/images/dt/dt-im1.png)

[Image source](https://why-change.com/2021/11/13/how-to-create-decision-trees-for-business-rules-analysis/)

Three key equations define the methods decision trees use to determine what feature to split on. These are the Gini Index, Entropy, and Information Gain. Suppose a target label has $k$ classes. Then we have the following:

1. **Gini Index**: This measures the purity of a node. A node is more pure when most of its samples belong to a single class. A Gini of 0 is pure, while a Gini of 1 is maximally impure. 

$$\text{Gini}(D_{\text{node}}) = 1 - \sum_{i=1}^{k}p_i^2$$

where $p_i$ is the proportion of class $i$ in the dataset.

2. **Entropy**: This measures the amount of disorder in a node. An entropy of 0 is fully ordered, while an entropy of $log_2(k)$ is maximally disordered. 

$$\text{Entropy}(D_{\text{node}}) = - \sum_{i=1}^{k} \log_2(p_i)$$

where $p_i$ is the proportion of class $i$ in the dataset.

3. **Information Gain**: This measures how much the entropy of a parent node decreases after making a node splitting decision. It is important to understand that information gain can be defined to use Entropy or Gini index.

$$\text{IG}(D, F)$$
$$= \text{Entropy}(D)\ - \sum_{v \in Values(F)} \frac{|D_v|}{|D|} \text{Entropy}(D_v)$$

where $F$ is the feature being used to split, $D_v$ is the subset of samples where $F = v$, and $|D|$ is the total number of samples.

Decision trees form non-linear boundaries that look like this:

![DT](/images/dt/dt-im2.png)

[Image source](https://paulvanderlaken.com/2020/03/31/visualizing-decision-tree-partition-and-decision-boundaries/)

Decision Trees are popular for their robustness, non-linearity, and interpretability, making them easy to understand and visualize. They perform well on a variety of datasets, from small to large, and are widely used in business, healthcare, and marketing. Their ability to explain how decisions are made makes them a preferred choice when the reasoning behind predictions needs to be transparent, such as in customer segmentation or risk assessment. 

## Questions
### Example
Here is a simple example of using Entropy and Information Gain to create a decision tree:

|**Row**|**Feature 1**|**Label**|
|-|-|-|
|1|High|Yes|
|2|High|Yes|
|3|Low|No|
|4|Medium|No|
|5|Medium|Yes|

{{% steps %}}

### Compute Entropy of Parent Node
$p_{yes}=\frac{3}{5}, p_{no}=\frac{2}{5}$

$\text{Entropy}(D)$\
$ = -\left(\frac{3}{5}\log_2\frac{3}{5}+\frac{2}{5}\log_2\frac{2}{5}\right)$\
$= -\left(0.6\cdot(-0.737)+0.4\cdot(-1.322)\right) \approx 0.971$

### Split by Feature 1 values and compute Entropy Child Nodes
Feature 1 has 3 values: Low, Medium, High

For Low, 

$p_{yes}=0, p_{no}=1$

$\text{Entropy}(D_{\text{Low}})$\
$ = -\left(0\log_20+1\log_21\right) = 0$

For Medium,  

$p_{yes}=\frac{1}{2}, p_{no}=\frac{1}{2}$

$\text{Entropy}(D_{\text{Medium}})$\
$ = -\left(0.5\log_20.5+0.5\log_20.5\right) = 1$

For High,  

$p_{yes}=1, p_{no}=0$

$\text{Entropy}(D_{\text{High}})$\
$ = -\left(1\log_21+0\log_20\right) = 0$

### Calculate the weighted Entropy after splitting

$\text{Weighted Entropy}$\
$ = \frac{2}{5} \cdot 0 + \frac{2}{5} \cdot 1 +\frac{1}{5} \cdot 0 = 0.4$

### Calculate the Information Gain

$IG(D, \text{Feature 1})$\
$ = \text{Entropy}(D) - \text{Weighted Entropy}$\
$ = 0.971 - 0.4$\
$ = 0.571$

{{% / steps %}}

Thus, splitting on Feature 1 offers an Information Gain of 0.571, which is quite high. If there were additional features, this would be performed for all of them, and the feature resulting in the maximum amount of Information Gain would be chosen to be split on.

### How decision trees can grow infinitely and overfitting
Decision trees are powerful but often overfit. A decision tree will keep splitting until no leaf nodes are impure, unless it is pruned or restricted. A tree without a depth limit of other criteria will overfit to its training data. This can be prevented via active pruning during training (max depth limits, minimum sample for splitting, etc.), or after training (by removing relatively useless branches).



## Data Preparation
While the datasets created from the unsupervised were almost complete, they needed some polishing and preparation before being fed into supervised learning models. The preparation steps taken are outlined below.

> [!NOTE]
> Source code can be found here:\
> [github.com/michael-van-vuuren/csci5612-workspace/models/supervised/form-dataset.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/6e61b27c4879f04762414e87597b84e05766a65b/models/supervised/form-dataset.ipynb)

### Dataset

{{% steps %}}

### Starter Dataset
Begin with the [dataset](https://github.com/michael-van-vuuren/csci5612-workspace/blob/6e61b27c4879f04762414e87597b84e05766a65b/data/arm-ready.csv) previously prepared for the Association Rule Mining section. 

### Augment
Augment the dataset with potentially insightful feature combinations.

{{% details title="Augmentations" closed="true" %}}

```python {filename=""}
# TikTok
df['TikTok Views per Post'] = df['TikTok Views'] / df['TikTok Posts']
df['TikTok Impact'] = df['TikTok Views per Post'] / (df['YouTube Views'] + 1)
df['TikTok View-to-Like Ratio'] = df['TikTok Views'] / (df['TikTok Likes'] + 1)
df['TikTok Likes per Post'] = df['TikTok Likes'] / (df['TikTok Posts'] + 1)

# YouTube
df['YouTube View-to-Like Ratio'] = df['YouTube Views'] / (df['YouTube Likes'] + 1)

# Shazam
df['Shazam Conversion Rate'] = df['Shazam Counts'] / (df['YouTube Views'] + df['TikTok Views'] + 1)
```

{{% /details %}}

### Clean
Clean up certain features such as genre and release date, and explicitly create a `Label` column to prepare for modeling (details in Jupyter Notebook).

### Drop rows that were previously imputed
After trial and error, the missing values that were previously imputed via median replacement were found to be more harmful than helpful for the models, so we remove them here (details in Jupyter Notebook). Additionally, they potentially violated the disjoint set requirement of train-test data, because the imputation was perfomed before performing any train-test split. There are still 3311 rows in the dataset. 

{{% /steps %}}

**Starting Data:**
![PreARMReady](/images/arm/PreARMReady.png)

**Model-Ready Data:**
![Clean](/images/dataprep/clean.png)

### Train-Test Split
80% of the samples were used for training and the remaining 20% were used for testing, allowing the model to be evaluated on unseen data. To ensure a fair comparison, all Naive Bayes, Decision Tree, and Logistic Regression models used the same 80/20 split. 

A more thorough approach would be to use cross-validation, where the data is split into multiple training and validation sets, called folds. This would give an idea of how the model performs with random variation in the training set. However, the models below use a standard train-test split, so the model results could vary based on the training set.

![Split](/images/dataprep/split.png)

[Image source](https://learningds.org/ch/16/ms_cv.html)

> [!IMPORTANT]
> Train-test splitting was performed after pre-modeling transformations were applied for each model, but prior to model training. See below for train-test snippets for each model.

{{% details title="Train-Test Snippets" closed="true" %}}

{{< tabs items="Via Metadata Only,Via Social Media Data & Metadata,Via Streaming Data & Metadata" >}}

  {{< tab >}}
  Training data:
  ![Train](/images/dataprep/dt1-train.png)
  Testing data:
  ![Test](/images/dataprep/dt1-test.png)
  {{< /tab >}}
  {{< tab >}}
  Training data:
  ![Train](/images/dataprep/dt2-train.png)
  Testing data:
  ![Test](/images/dataprep/dt2-test.png)
  {{< /tab >}}
  {{< tab >}}
  Training data:
  ![Train](/images/dataprep/dt3-train.png)
  Testing data:
  ![Test](/images/dataprep/dt3-test.png)
  {{< /tab >}}

{{< /tabs >}}

{{% /details %}}



## Decision Tree Models
For the three Decision Tree models, the goal was to compare model performance based on the feature set used. As a control, the first decision tree used only metadata about each song. The second decision tree used metadata and social media data about each song. The third decision tree used metadata and streaming data about each song.  

> [!NOTE]
> Source code for all three Decision Tree models can be found here:\
> 1: [github.com/michael-van-vuuren/csci5612-workspace/models/supervised/decision-tree-metadata.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/6e61b27c4879f04762414e87597b84e05766a65b/models/supervised/decision-tree-metadata.ipynb)\
> 2: [github.com/michael-van-vuuren/csci5612-workspace/models/supervised/decision-tree-metadata-social-media.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/6e61b27c4879f04762414e87597b84e05766a65b/models/supervised/decision-tree-metadata-social-media.ipynb)\
> 3: [github.com/michael-van-vuuren/csci5612-workspace/models/supervised/decision-tree-metadata-streaming.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/6e61b27c4879f04762414e87597b84e05766a65b/models/supervised/decision-tree-metadata-streaming.ipynb)

### Via Metadata Only
#### Feature Selection
All features that were considered metadata were included. A correlation heatmap was created to understand how the features interacted with each other and the label. A single feature was chosen for any two features with very high correlation.

![Features](/images/dt/dt1-features.png)

While sklearn's `DecisionTreeClassifier` can handle categorical data, it requires that the categorical feature to use one-hot encoding. Thus, the categorical metadata feature `Genre` which was also included, was encoded accordingly. 

#### Results
|    **Approach**   |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|-------------------|------------|-------------|----------|------------|
| Via Metadata Only |   0.7572   |   0.7948    |  0.8200  |   0.8072   |

This baseline Decision Tree model was trained using only song metadata, such as genre, release timing, artist followers, track length, and whether the song is marked explicit. It achieved an F1 Score of 0.8072, with solid performance in recall (0.8200) and precision (0.7948).

While these results show that basic metadata carries meaningful information about playlist potential, the model lacks visibility into how the song is actually received by listeners. Without any social media or streaming context, it can only estimate likelihood based on internal traits like popularity of the artist, release activity, or song characteristics.

This makes the metadata-only model a good starting point, but it also highlights the limits of using metadata alone. It misses out on key external signals, like TikTok trends or streaming traction, that influence playlist inclusion. The next models reveal that adding those layers of data leads to better predictive performance.

![ConfusionMatrix](/images/dt/dt1-conf.png)

It is interesting to see in the decision tree and feature importance chart that `Releases`, which is an approximation of the number of times a song has been released (CDs, vinyl, and digital releases), is by far the most important feature. The decision tree shows that when the number of releases is lower, it is more likely that the playlist inclusion probability is low. This makes intuitive sense, since songs that are released more are likely more viral, and therefore appear in a greater number of playlists. 

![Tree](/images/dt/dt1-tree.png)

![Importance](/images/dt/dt1-imp.png)

#### Hyperparameter tuning
This decision tree was tuned by varying the Minimum Samples Split (MSS) parameter, which is the minimum number of samples required to split an internal node. From the chart below, it appears that an MSS of 65 results in the greatest test F1 Score, which was used in the decision tree above. 

![Tuning](/images/dt/dt1-tuning.png)

> [!NOTE]
> This methodology of hyperparameter tuning can be improved by using cross-validation, instead of the same train-test split for every hyperparameter variation; this is just for demonstration.

### Via Social Media Data and Metadata
#### Feature Selection
All features that were considered social media data and metadata were included. A correlation heatmap was created to understand how the features interacted with each other and the label. A single feature was chosen for any two features with very high correlation.

![Features](/images/dt/dt2-features.png)

While sklearn's `DecisionTreeClassifier` can handle categorical data, it requires that the categorical feature to use one-hot encoding. Thus, the categorical metadata feature `Genre` which was also included, was encoded accordingly. 

#### Results
|       **Approach**               |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|----------------------------------|------------|-------------|----------|------------|
| Via Social Media Data & Metadata |   0.8250   |   0.8734    |  0.8394  |   0.8561   |

Adding social media signals, like TikTok views, likes, posts, YouTube likes, and Shazam activity, to the song metadata led to a significant performance boost. The model’s F1 Score rose to 0.8561, with both precision (0.8734) and recall (0.8394) showing clear improvement over the metadata-only version.

This jump highlights how social media presence plays a major role in playlist inclusion. Songs with strong engagement on platforms like TikTok or high Shazam conversion rates are more likely to gain traction, and the model effectively picked up on those patterns.

Compared to using metadata alone, this model was better at identifying songs that not only have potential based on intrinsic features, but are also have an online presence, making it a stronger predictor of playlist inclusion.

![ConfusionMatrix](/images/dt/dt2-conf.png)

It is interesting to see in the decision tree and feature importance chart that `Shazam Counts`, which is the number of times a song has been "Shazamed", or searched for using the Shazam song recognition application, is at the top. The decision tree shows that when the number of Shazams is lower, it is more likely that the playlist inclusion probability is low. This makes sense, since songs that are searched for less are less likely to be included in playlists. Below that is again `Releases`, then `Followers`, `Days Since Release`, and finally YouTube and TikTok social media metrics and some `Genre` information.

![Tree](/images/dt/dt2-tree.png)

![Importance](/images/dt/dt2-imp.png)

#### Hyperparameter tuning
This decision tree was tuned by varying the Max Depth (MD) parameter, which is the maximum depth the tree is allowed to grow to. From the chart below, it appears that an MD of 5 results in the greatest test F1 Score, which was used in the decision tree above. 

![Tuning](/images/dt/dt2-tuning.png)

> [!NOTE]
> This methodology of hyperparameter tuning can be improved by using cross-validation, instead of the same train-test split for every hyperparameter variation; this is just for demonstration.

### Via Streaming Data and Metadata
#### Feature Selection
All features that were considered streaming data and metadata were included. A correlation heatmap was created to understand how the features interacted with each other and the label. A single feature was chosen for any two features with very high correlation.

![Features](/images/dt/dt3-features.png)

While sklearn's `DecisionTreeClassifier` can handle categorical data, it requires that the categorical feature to use one-hot encoding. Thus, the categorical metadata feature `Genre` which was also included, was encoded accordingly. 

#### Results
|       **Approach**            |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|-------------------------------|------------|-------------|----------|------------|
| Via Streaming Data & Metadata |   0.8778   |   0.9044    |  0.8978  |   0.9011   |

When streaming data, like Spotify streams, Pandora activity, track rankings, and AirPlay spins, was added to the metadata, the model achieved its best performance yet. With an F1 Score of 0.9011, it showed high precision (0.9044) and recall (0.8978), indicating it was both accurate and consistent in predicting playlist inclusion.

This result is not surprising, since streaming metrics directly reflect listener behavior and popularity across platforms. By combining this with metadata, the model was highly effective at predicting playlist inclusion.

These results confirm that streaming data is a strong indicator of playlist success, but also highlight why it was intentionally excluded in earlier models to avoid circular reasoning; while the streaming data is highly effective at predicting the target label, it is less applicable than using social media data, if the goal is to predict how social media song trends play into playlist inclusion.

![ConfusionMatrix](/images/dt/dt3-conf.png)

It is not surprising to see in the decision tree and feature importance chart that `Spotify Streams`, which is the number of streams a song has been played on Spotify, is at the top. The decision tree shows that when the number of Spotify streams is lower, it is more likely that the playlist inclusion probability is low. This makes sense, because songs that have less plays likely appear on less playlists. Below that is `AirPlay Spins`, then `Days Since Release`, `Length`, `Releases`, `Pandora Track Stations`, and other less important features.

![Tree](/images/dt/dt3-tree.png)

![Importance](/images/dt/dt3-imp.png)

#### Hyperparameter tuning
This decision tree was tuned by varying the Minimum Samples Leaf (MSL) parameter, which is the minimum number of samples required to be at a leaf node. From the chart below, it appears that an MSL of 17 results in the greatest test F1 Score, which was used in the decision tree above. 

![Tuning](/images/dt/dt3-tuning.png)

> [!NOTE]
> This methodology of hyperparameter tuning can be improved by using cross-validation, instead of the same train-test split for every hyperparameter variation; this is just for demonstration.

## Conclusions
The Decision Tree models revealed how different layers of data impact predicting whether a song will be included in a playlist.

With just metadata, the model performed reasonably well, showing that features like the number of releases or artist followers can offer an initial sense of a song’s potential. However, this approach overlooks broader listener engagement, making it a good starting point but not enough on its own.

When social media data was incorporated, the model improved significantly. Engagement on platforms like TikTok and Shazam provided deeper insights into how listeners interact with a song. This suggests that online trends and user behavior are strong predictors of playlist inclusion, especially when combined with song metadata.

Finally, including streaming data produced the best results, as it directly reflects how often a song is being played. However, using streaming data in this context risks circular reasoning since playlist inclusion can influence streaming numbers. For this reason, social media data, though slightly less precise, may be more valuable for early predictions or scouting tracks for future playlists.

Overall, the Decision Tree models support the main goal of this project: predicting playlist inclusion without relying on direct streaming data. By focusing on metadata and social signals, we can effectively spot tracks with playlist potential based on data other than streaming data. By using a Random Forest classifier and/or ensemble methods, the result metrics could be improved even further.
