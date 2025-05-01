---
title: Naive Bayes
type: docs
prev: models/_index
next: models/dt
weight: 4
math: true
---

## Overall Goal
The primary aim of this project is to predict whether a song is likely to be included in a playlist, using metadata and social media data, without relying on direct streaming numbers to avoid circular reasoning.

Since many columns in the dataset relate to streaming performance and popularity, using something like Spotify streams as the target would lead to a model that just learns what it already knows. Instead, a new target variable was created to represent the *probability of playlist inclusion*, based on playlist count and reach data across platforms.

For classification purposes, this probability was also binned into two categories: *low* and *high* likelihood of being included in a playlist.

The original dataset included the following features:

![19](/images/intro-eda/19.png)

To calculate playlist inclusion probability, the playlist count and reach columns were normalized, weighted, and combined as follows:

$$ P(\text{track being included in a playlist}) =$$
$$\sigma\left(\left(\alpha\sum_i \frac{\text{C}_i}{max(\text{C}_i)} + \beta\sum_i \frac{\text{R}_i}{max(\text{R}_i)}\right)^{0.5}\right)$$

where

1. $C_i$ is the playlist count column for platform $i$
2. $R_i$ is the playlist reach column for platform $i$
3. $\sigma(x) = \frac{1}{1+e^{-x}}$, or the sigmoid function, is used to map a value between 0 and 1
4. $\alpha$ and $\beta$ are used to tune the weights of count and reach on the score
5. Square root of weighted sum is taken to correct heavy right skew

The target variable was then discretized to form a target label based on quantiles. Note that the number of examples in the *low* category and the number of examples in the *high* category is roughly equal. 

In this section, the goal is to train a variety of Naive Bayes models that accept different forms of input data on social media signals and song metadata (excluding direct streaming features) to predict whether a song is more or less likely to end up on a playlist.



## Overview
### Definition
A Naive Bayes (NB) classifier is a supervised machine learning model that is based on Bayes' Theorem. 

$$P(A|B) = \frac{P(B|A)P(A)}{P(B)}$$

Intuitively, a Naive Bayes classifier works by predicting the class that an unseen example is most likely to belong to given its features. The "naive" assumption is that every feature is independent of every other feature. For an unseen example, Naive Bayes estimates the probabilities of it belonging to each class, and chooses the class with the highest probability as its prediction. 

Mathematically, Naive Bayes is expressed as follows:

Given a feature vector $x = (x_1,...,x_n)$ and classes $C_k$, Naive Bayes tries to find class $k$ that maximizes $P(C_k|x)$. In this case, Bayes Thereom is:

$$P(C_k|x) = \frac{P(x|C_k)P(C_k)}{P(x)}$$

We can ignore $P(x)$, because it does not change across classes for a given $x$.

$$P(C_k|x) \propto P(x|C_k)P(C_k)$$

Since the features are assumed to be independent, this can be expanded as:

$$P(C_k|x) \propto P(C_k) \prod_{i=1}^{n} P(x|C_k)$$

To get the class with the highest probability given the feature-set, we simply find the highest $P(C_k|x)$:

$$\hat{y} = \argmax_k \left[P(C_k) \prod_{i=1}^{n} P(x|C_k)\right]$$

Naive Bayes classifiers trained on discrete features, like Multinomial or Categorical Naive Bayes, often require  a correction mechanism called Laplace Smoothing. Its purpose is to avoid zero probabilities when a specific feature value been observed for a given class in the training data. Since each probability is being multiplied in the step above, if any single probability became zero for a feature, the entire calculated probability for that class would incorrectly become zero. Laplace Smoothing corrects for this by adding a small value to each count:

$$P(x_i|C_k) = \frac{\text{count}(x_i, C_k) + 1}{\text{total count of features in } (C_k) + V}$$

Note that Laplace Smoothing is not needed for Gaussian Naive Bayes, which uses continuous features and models them using Gaussian distributions.

### Comparison
|**Type**|**Input**|**Assumed Input Distribution**|**Use Case**|
|-----------------|------------|-------------|----------|
|**MultinomialNB**|Discrete counts|Multinomial|Text classification (e.g. counts of words)|
|**GaussianNB**|Continuous values|Gaussian (mean of 0, standard deviation of 1)|Numerical features (e.g. temperature)|
|**BernoulliNB**|Binary (0/1) features|Bernoulli|True/false features (e.g. light on/off, word exists or not)|
|**CategoricalNB**|Categorical (binned) features|Categorical|Discrete features (e.g. colors, rankings)|

In general, Naive Bayes is an efficient, scalable, and powerful machine learning model. It is frequently used in big data applications such as text classification, recommendation systems, and medical prediction models. 

> [!TIP]
> For more information about Naive Bayes and its Scikit-learn implementations, visit https://scikit-learn.org/stable/modules/naive_bayes.html

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

{{< tabs items="Multinomial,Gaussian,Categorical" >}}

  {{< tab >}}
  Training data:
  ![Train](/images/dataprep/mnb-train.png)
  Testing data:
  ![Test](/images/dataprep/mnb-test.png)
  {{< /tab >}}
  {{< tab >}}
  Training data:
  ![Train](/images/dataprep/gnb-train.png)
  Testing data:
  ![Test](/images/dataprep/gnb-test.png)
  {{< /tab >}}
  {{< tab >}}
  Training data:
  ![Train](/images/dataprep/cnb-train.png)
  Testing data:
  ![Test](/images/dataprep/cnb-test.png)
  {{< /tab >}}

{{< /tabs >}}

{{% /details %}}

### Feature Selection
For all of the Naive Bayes models, the goal was to use social media data along with metadata to train classifiers to predict whether a song was more or less likely to appear in a playlist. 

Thus, it was important that the features selected did not contain any streaming data. Initially, all features that were considered social media data and metadata were included. A correlation heatmap was created to understand how the features interacted with each other and the label. A single feature was chosen for any two features with very high correlation, such as `YouTube Views` and `YouTube Likes`. 

![Features](/images/nb/nb-features.png)

Each Naive Bayes model, apart from Gaussian Naive Bayes, required a pre-modeling transformation to make the features compatible with the model type. For example, Categorical Naive Bayes expects categorical data, but the current data is quantitative. In this case, a transformation was applied before the data was used an input in the Categorical Naive Bayes model.

## Naive Bayes Models

> [!NOTE]
> Source code for all three Naive Bayes models can be found here:\
> [github.com/michael-van-vuuren/csci5612-workspace/models/supervised/naive-bayes.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/6e61b27c4879f04762414e87597b84e05766a65b/models/supervised/naive-bayes.ipynb)

### Multinomial Naive Bayes
#### Pre-Modeling Transformation
Multinomial Naive Bayes is traditionally used for text data, such as word counts in documents, and requires input in the form of count data. To make the quantitative dataset compatible, each numeric column was binned into five categories: low, low-medium, medium, medium-high, and high. Then, for each row, the number of times each category appeared was tallied, converting the continuous data into a count-based format suitable for the model. 

![MNBDataset](/images/nb/mnb-data.png)

Care was taken to ensure the bin distributions were roughly even (via quantile-based discretization) so that the model could properly distinguish between categories.

![Barchart](/images/nb/mnb-barchart.png)

#### Results

|       **Model**         |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|-------------------------|------------|-------------|----------|------------|
| Multinomial Naive Bayes |   0.7707   |   0.8278    |  0.7956  |   0.8114   |

The Multinomial Naive Bayes model achieves an F1 Score of 0.8114, indicating a strong balance between precision and recall. While the accuracy of 0.7707 may not seem outstanding at first glance, it outperforms what would be expected from a random classifier.

The confusion matrix below shows that the model correctly identifies both true positives and true negatives more frequently than it makes false predictions. This suggests that the model has learned meaningful patterns in the data rather than making guesses. The precision of 0.8278 implies that when the model predicts a positive class, it is correct more than 82% of the time. Meanwhile, a recall of 0.7956 indicates that it successfully captures the majority of actual positive cases.

Overall, the Multinomial Naive Bayes model worked quite well, especially considering the data it was given, which was just counts of the number of features considered 'low', 'low-medium', 'medium', 'medium-high', and 'high'. 

![ConfusionMatrix](/images/nb/mnb-conf.png)

### Gaussian Naive Bayes
#### Pre-Modeling Transformation
The dataset was already compatible with a Gaussian Naive Bayes model, as it consisted of quantitative features suited for continuous input. No categorical transformation was required. However, to better meet the model's assumption of normally distributed features, a log transformation was selectively applied to columns with high skewness. In addition, a standard scaler was used to normalize the features, ensuring they followed a distribution closer to the standard normal.

#### Results

|       **Model**      |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|----------------------|------------|-------------|----------|------------|
| Gaussian Naive Bayes |   0.7934   |   0.8827    |  0.7689  |   0.8218   |

The Gaussian Naive Bayes model, which was trained on continuous data rather than categorical feature counts, showed a slight shift in performance metrics. It achieved an F1 Score of 0.8218, outperforming the Multinomial model. This suggests a modest improvement in the model's overall balance between precision and recall.

One of the most significant improvements is in precision, which rose to 0.8827. This indicates that the Gaussian model is better at minimizing false positives. In other words, when it predicts a positive class, it is correct nearly 88% of the time. This suggests that the additional detail preserved in the continuous input features allowed the model to make more confident and accurate positive predictions.

However, this increase in precision came at a slight cost to recall, which dropped from 0.7956 in the Multinomial model to 0.7689. This means the Gaussian model missed a few more actual positive cases, making it slightly more conservative in its predictions.

The accuracy also improved, rising to 0.7934, showing that the Gaussian Naive Bayes model was able to leverage the continuous data more effectively. Overall, the results suggest that providing the model with more nuanced, continuous input data enabled it to make more precise classifications.

![ConfusionMatrix](/images/nb/gnb-conf.png)

### Categorical Naive Bayes
#### Pre-Modeling Transformation
Categorical Naive Bayes is designed for categorical input features and does not require count data like the Multinomial variant. To make the quantitative dataset compatible, each numeric column was binned into discrete categories using an adjusted Sturges' rule to determine the number of bins based on the size of the dataset.

Unlike the Multinomial model, no tallying of category counts was performed. Each feature retained its binned categorical value directly.

Again, care was taken to ensure the bin distributions were roughly even so that the model could properly distinguish between categories.

![Barchart](/images/nb/cnb-barchart.png)

#### Results

|        **Model**        |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|-------------------------|------------|-------------|----------|------------|
| Categorical Naive Bayes |   0.8084   |   0.8817    |  0.7981  |   0.8378   |

The Categorical Naive Bayes model, trained on binned versions of continuous features, delivered the strongest overall performance with an F1 Score of 0.8378. It maintained high precision at 0.8817, nearly matching the Gaussian model, while improving recall to 0.7981, slightly better than the recall of the Multinomial Naive Bayes model.

Its accuracy of 0.8084 was also the highest among the three models, suggesting that binning preserved important feature information while reducing noise. Overall, the model effectively balanced precision and recall, showing that discretizing continuous data can improve performance without sacrificing much detail.

![ConfusionMatrix](/images/nb/cnb-conf.png)



## Conclusions
The models show that it's possible to predict whether a song is likely to end up on a playlist using just social media signals and metadata, without relying on streaming numbers. All three Naive Bayes models performed well, with the Categorical Naive Bayes model giving the best results overall.

This suggests that metadata like `Days Since Release` and number of `Releases`, and social media data like `YouTube Likes` and `Shazam Counts` can provide meaningful insight into playlist probability. It also confirms that the custom target label, which is based on playlist count and reach, is a viable alternative to using direct streaming stats, helping us avoid circular reasoning while still capturing real trends.
