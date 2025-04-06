---
title: Naive Bayes
type: docs
prev: models/_index
next: models/dt
weight: 4
---

## Overall Goal
text



## Overview
### Definition
- describe what naive bayes (nb) is in general
- describe when and why nb is used
- explain smoothing and why it’s necessary

### Comparison
- explain and compare these nb types from sklearn:
  - multinomial nb
  - gaussian nb
  - bernoulli nb 
  - categorical nb
- discuss when each flavor is appropriate

- include relevant images to help illustrate concepts



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
To evaluate model performance on a classification task, the dataset was randomly split into disjoint training and testing sets using an 80/20 ratio—80% for training and 20% for testing. This approach helps ensure both sets are representative of the overall data distribution while remaining completely separate.

Using disjoint sets is critical for classification, as it ensures the model is evaluated on data it hasn't seen during training. This prevents data leakage, which would artificially inflate performance metrics and give a misleading sense of the model’s ability to generalize to new, unseen examples.

The same train-test split was used across all Naive Bayes, Decision Tree, and Logistic Regression models on this page to ensure a fair comparison. By testing each model on the same data, we remove any differences that could come from using different splits.

A more thorough approach would be to use cross-validation, where the data is split into multiple training and validation sets. This allows the model to be evaluated on different subsets of the data and gives a more reliable measure of how well it performs in general. After cross-validation, the final model can be tested on a separate testing set to assess its performance on truly unseen data. However, the models below use a standard train-test split, so the reported performance metrics could be slightly higher or lower than if cross-validation were used.

![Split](/images/dataprep/split.png)

[Image source](https://learningds.org/ch/16/ms_cv.html)

> [!IMPORTANT]
> Train-test splitting was performed after pre-modeling transformations were applied for each model, but prior to model training.

### Feature Selection
- how the features were chosen

- create and insert:
  - link to prepared data
  - images of relevant dataframes (cleaned data, train, test sets)



## Naive Bayes Models
- high level overview of what each model is for

### Multinomial Naive Bayes
#### Pre-Modeling Transformation
- how data was transformed

#### Results
- generate and include:
  - confusion matrices
  - accuracy scores
- use plots/tables/graphics to visualize results
- compare and discuss the output of all models

### Gaussian Naive Bayes
#### Pre-Modeling Transformation
- how data was transformed

#### Results
- generate and include:
  - confusion matrices
  - accuracy scores
- use plots/tables/graphics to visualize results
- compare and discuss the output of all models

### Categorical Naive Bayes
#### Pre-Modeling Transformation
- how data was transformed

#### Results
- generate and include:
  - confusion matrices
  - accuracy scores
- use plots/tables/graphics to visualize results
- compare and discuss the output of all models



## Conclusions
- describe what insights or predictions your models provide
- connect conclusions to your main project topic
