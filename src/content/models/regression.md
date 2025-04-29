---
title: Logistic Regression
type: docs
prev: models/dt
next: models/svm
weight: 6
math: true
---

## Overall Goal
For the Logistic Regression model, the goal is to compare its classification performance to that of the Multinomial Naive Bayes model. To ensure a fair comparison, both models use the exact same set of features, which includes binned and counted representations of the original quantitative dataset.

As with the other models, the task is to predict whether a song is likely to be included in a playlist. For details on how the target label was constructed and how the features were prepared, see the Naive Bayes tab.



## Overview
### Definition
To understand logistic regression, it is necessary to first understand linear regression.
 
#### Linear Regression
Linear regression predicts a continuous output using one or more input features. It assumes a linear relationship: $y = \beta_0 + \beta_1x_1 + ... + \beta_nx_n + \epsilon$. It minimizes the sum of squared errors to find the best-fitting line. This works well for predicting continuous values, but it does not work for classification, since the predicted values are unbounded.

#### Logistic Regression
Logistic regression is used for binary classification, predicting probabilities between 0 and 1. It models the log-odds of the target as a linear combination of features and applies the sigmoid function to map values to probabilities. It uses maximum likelihood estimation (MLE) to find parameters that maximize the likelihood of the observed labels. Instead of minimizing squared errors (like linear regression), logistic regression maximizes the likelihood that the predicted probabilities match the actual labels. The loss function derived from MLE is the log loss (cross-entropy). 

![Regressions](/images/lr/reg-im.png)

[Image source](https://datasciencedojo.com/blog/linear-regression-vs-logistic-regression/)

### Comparison
#### Linear vs Logistic Regression
Both use linear combinations of inputs. Linear regression outputs continuous values and uses least squares; logistic regression outputs probabilities and uses MLE. Linear regression has unbounded output, while logistic regression uses the sigmoid function to bound output between 0 and 1:

$$\sigma(z) = \frac{1}{1+e^{-z}}$$

#### Logistic Regression vs Naive Bayes
Logistic regression and Naive Bayes are both used to predict categories, like *yes* or *no*. Logistic regression learns the best boundary between classes by looking at how the features relate to the outcome. On the other hand, Naive Bayes uses probability rules and assumes all features are independent. Naive Bayes is usually faster and works well with small or simple data, but logistic regression often gives better results when there's more data or features are related. So logistic regression is more flexible and often more precise, while Naive Bayes is simpler and quicker.



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

{{< tabs items="Logistic Regression,Multinomial Naive Bayes" >}}

  {{< tab >}}
  The datasets are intentionally the same for comparison purposes.

  Training data:
  ![Train](/images/dataprep/mnb-train.png)
  Testing data:
  ![Test](/images/dataprep/mnb-test.png)
  {{< /tab >}}
  {{< tab >}}
  The datasets are intentionally the same for comparison purposes.

  Training data:
  ![Train](/images/dataprep/mnb-train.png)
  Testing data:
  ![Test](/images/dataprep/mnb-test.png)
  {{< /tab >}}

{{< /tabs >}}

{{% /details %}}

## Logistic Regression Model 

> [!NOTE]
> Source code for all the Logistic Regression and Multinomial Naive Bayes models can be found here:\
> 1: [github.com/michael-van-vuuren/csci5612-workspace/models/supervised/logistic-regression.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/6e61b27c4879f04762414e87597b84e05766a65b/models/supervised/logistic-regression.ipynb)\
> 2: [github.com/michael-van-vuuren/csci5612-workspace/models/supervised/naive-bayes.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/6e61b27c4879f04762414e87597b84e05766a65b/models/supervised/naive-bayes.ipynb)

### Logistic Regression
#### Feature Selection
For the Logistic Regression model, the goal was to compare model performance to the Multinomial Naive Bayes model. Thus, the features used by the two models are identical (See the Naive Bayes tab for more information). 

#### Results
|       **Model**     |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|---------------------|------------|-------------|----------|------------|
| Logistic Regression |   0.7692   |   0.8146    |  0.8127  |   0.8136   |

The Logistic Regression model performed reasonably well, with an accuracy of 0.7692, precision of 0.8146, recall of 0.8127, and an F1 score of 0.8136. The results show that logistic regression is decent at predicting playlist inclusion, even when using the count data that was made for the Multinomial Naive Bayes model.

![ConfusionMatrix](/images/lr/lr-conf.png)

#### Comparison to Multinomial Naive Bayes
|       **Model**         |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|-------------------------|------------|-------------|----------|------------|
| Multinomial Naive Bayes |   0.7707   |   0.8278    |  0.7956  |   0.8114   |

When comparing Logistic Regression to the Multinomial Naive Bayes model, the two models are quite similar in performance. The Logistic Regression had slightly lower accuracy (0.7692 compared to 0.7707), but higher precision (0.8146 versus 0.8278) and recall (0.8127 versus 0.7956). The F1 score for Logistic Regression (0.8136) was just a bit higher than the Multinomial Naive Bayes model (0.8114).

These results indicate that both models perform similarly, but Logistic Regression tends to be slightly more balanced in precision and recall, whereas Naive Bayes has a slightly higher precision with a bit lower recall. The similar overall F1 scores suggest that either model could work effectively for the task, but the choice between them may come down to preference for precision or recall, or on the efficiency required for the model.

![ConfusionMatrix](/images/nb/mnb-conf.png)



## Conclusions
The Logistic Regression and Multinomial Naive Bayes models performed similarly, with very close accuracy, precision, recall, and F1 scores. This suggests that both models are effective for predicting playlist inclusion. The choice between them would depend on the specific requirements of the task. However, overall the Decision Trees are the most accurate. 

As with the Naive Bayes models, the results above show that metadata like `Days Since Release` and number of `Releases`, and social media data like `YouTube Likes` and `Shazam Counts` can provide meaningful insight into playlist probability. It also confirms that the custom target label, which is based on playlist count and reach, is a viable alternative to using direct streaming stats, helping us avoid circular reasoning while still capturing real trends.
