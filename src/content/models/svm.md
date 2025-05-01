---
title: Support Vector Machine (SVM)
type: docs
prev: models/regression
next: models/ensemble
weight: 7
math: true
---

## Overall Goal

The goal of this section is to predict a song’s genre based on its lyrics. While this goal is not aimed at estimating a song’s likelihood of appearing in a playlist, it is closely related. Genre is a valuable feature for playlist prediction models, but it is often missing from datasets. By creating a model that predicts a song's genre from its lyrics, songs with missing genres can be appropriately labeled, and the performance of the playlist likelihood models can be improved.



## Overview

### Definition

A Support Vector Machine (SVM) is a powerful linear machine learning method that divides data points using a hyperplane. A hyperplane is a linear boundary that separates data points in any dimensional space into two groups. Although SVMs create linear hyperplanes, they can create what appear to be non-linear divisions using kernel functions, which transform data into a space where it becomes linearly separable, even if it was not before. 

![SVM2](/images/svm/svm-im2.png)

[Image source](https://botpenguin.com/glossary/kernel-method)

In a hard margin SVM, the algorithm seeks the hyperplane that perfectly separates the classes with the maximum margin, and allows for no misclassifications. If the data clusters are close, then the margin can be really small or impossible to create. On the other hand, a soft margin SVM relaxes this constraint, and allows for some misclassified points (called slack points) in the training data in exchange for a bigger margin. In most cases, soft margin SVM is used because it is more robust.

![SVM1](/images/svm/svm-im1.png)

[Image source](https://www.ibm.com/think/topics/support-vector-machine)

SVMs work well on high dimensional data, so they are commonly used for text classification (which is covered in this section), image classification, and finance models. 

### Questions

{{% steps %}}

### Why is an SVM a linear separator?

An SVM is a linear separator because it uses the following linear equation to find an optimal dividing hyperplane:

$$w^Tx + b = 0$$ 

where 
- $w$ is a weight vector that represents the hyperplane's normal vector
- $x$ is a specific data point
- $b$ is a bias term that shifts the hyperplane

### How does the kernel function work?

As mentioned earlier, a kernel function can be used by an SVM to transform its input data points into a high-dimensional space where a hyperplane can divide them by their classes. This allows SVMs to behave non-linearly even though they always use linear hyperplanes. It takes two input data points, $x$ and $y$, and outputs a measure of similarity between them in the transformed feature space. This is done for all pairs of data points in the dataset.

### Kernel functions

What do the main kernel functions look like?

- **Linear:** $$K(x, y) = x \cdot y$$
- **Radial basis function (RBF):** $$K(x, y) = exp(-\gamma||x - y||^2)$$
where $\gamma$ controls the influence of each training sample (often a Gaussian RBF kernel is used).
- **Polynomial:** $$K(x, y) = (x \cdot y + c)^d$$
where $c$ is a constant and $d$ is the degree of the polynomial.

### The dot product

Why is the dot product so critical to the use of the kernel?

Dot products are used to measure the similarity between two vectors. The kernel functions all use dot products between $x$ and $y$, but they apply additional transformations to measure the dot product in a transformed space. This is called the "kernel trick", and it allows SVMs to operate in a transformed feature space without needing to project all of its samples into that space.

### Example 

In this example, three points in two dimensional space are casted into higher-dimensional space using a polynomial kernel with r = 1 and d = 2. These three points are implicitly mapped into a six-dimensional feature space via the kernel trick, without computing their projections explicitly:

$$p_1 = (1, 0),\ p_2 = (0, 1),\ p_3 = (1, 1)$$

Now, we can expand the kernel from earlier with the values of $r$ and $d$:

$$K(x, y) = (x \cdot y + 1)^2$$
$$K(x, y) = x_1^2y_1^2 + x_2^2y_2^2 + 2x_1y_1x_2y_2 +\\
2x_1y_1 + 2x_2y_2 + 1$$

This can be factored into a dot product between $\phi(x)$ and $\phi(y)$:

$$K(x, y) = \phi(x)^T\phi(y)$$

where
$$
\phi(x) = 
\begin{bmatrix}
1 \\ \sqrt{2}x_1 \\ \sqrt{2}x_2 \\ x_1^2 \\ \sqrt{2}x_1x_2 \\ x_2^2
\end{bmatrix}
\quad
\phi(y) = 
\begin{bmatrix}
1 \\ \sqrt{2}y_1 \\ \sqrt{2}y_2 \\ y_1^2 \\ \sqrt{2}y_1y_2 \\ y_2^2
\end{bmatrix}
$$

Now, the mappings of the points from 2D to 6D can be computed:

$$\phi(p_1) =\quad
\begin{bmatrix}
1 \\ \sqrt{2}(1) \\ \sqrt{2}(0) \\ (1)^2 \\ \sqrt{2}(1)(0) \\ (0)^2
\end{bmatrix} = 
\begin{pmatrix}
1 \\ \sqrt{2} \\ 0 \\ 1 \\ 0 \\ 0
\end{pmatrix}$$

$$\phi(p_2) =\quad
\begin{bmatrix}
1 \\ \sqrt{2}(0) \\ \sqrt{2}(1) \\ (0)^2 \\ \sqrt{2}(0)(1) \\ (1)^2
\end{bmatrix} = 
\begin{pmatrix}
1 \\ 0 \\ \sqrt{2} \\ 0 \\ 0 \\ 1
\end{pmatrix}$$

$$\phi(p_3) =\quad
\begin{bmatrix}
1 \\ \sqrt{2}(1) \\ \sqrt{2}(1) \\ (1)^2 \\ \sqrt{2}(1)(1) \\ (1)^2
\end{bmatrix} = 
\begin{pmatrix}
1 \\ \sqrt{2} \\ \sqrt{2} \\ 1 \\ \sqrt{2} \\ 1
\end{pmatrix}$$

These new points exist in 6D space, and could be linearly separable if they were not in 2D space.

{{% /steps %}}

## Data Preparation

This section used lyric and genre data from the Million Song Dataset website.

>[!NOTE]
>The Million Song Dataset website: http://millionsongdataset.com/\
>The lyric data (downloaded as an SQLite database): http://millionsongdataset.com/musixmatch/\
>The genre data (CD2C downloaded as a text file): https://www.tagtraum.com/msd_genre_datasets.html

Both the lyric and genre datasets had a `track_id` column, which was used to combine them. At a high level, the following steps were taken:
1. The lyrics were cleaned (removed stop words, non-english words, non-nouns, uncommon words, and short words)
2. The cleaned lyrics were formatted as "text documents," treating each track as an individual document
3. These documents were paired with genre labels to create supervised learning samples
4. The dataset was balanced across genre classes
5. Samples were split into features and labels, then those were each divided into training and test sets
6. The feature sets were vectorized using the "Bag of Words" model, and then converted into TF-IDF matrices

> [!NOTE]
> Source code can be found here:\
> [github.com/michael-van-vuuren/csci5612-workspace/models/supervised/svm-and-boosting.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/d577d96020be7ac2e551e71ffbf006b0dd59b00c/models/supervised/svm-and-boosting.ipynb)

**Starting Data:**
![PreBoWReady](/images/svm/PreBoWReady.png)

This dataset had 19,045,332 rows and 237,662 unique tracks.

**Model-Ready Data (lyrics still need to be vectorized):**
![BoWReady](/images/svm/BoWReady.png)

### Train-Test Split
80% of the samples were used for training and the remaining 20% were used for testing, allowing the model to be evaluated on unseen data. To ensure a fair comparison, both the SVM and Ensemble models used the same 80/20 split. 

A more thorough approach would be to use cross-validation, where the data is split into multiple training and validation sets, called folds. This would give an idea of how the model performs with random variation in the training set. However, the models below use a standard train-test split, so the model results could vary based on the training set.

![Split](/images/dataprep/split.png)

[Image source](https://learningds.org/ch/16/ms_cv.html)

> [!IMPORTANT]
> Train-test splitting was performed after pre-modeling transformations were applied for each model, but prior to model training. See below for train-test snippets for each model.

{{% details title="Train-Test Snippets" closed="true" %}}

Training data (before TF-IDF conversion):
![Train](/images/svm/train.png)
Testing data (before TF-IDF conversion):
![Test](/images/svm/test.png)
Example of training data after TF-IDF conversion (very sparse matrix):
![Sparse](/images/svm/sparse.png)

{{% /details %}}



## Support Vector Machine Models

Four SVMs were created:
1. Initial experiments were conducted using Scikit-learn's SVC with linear, RBF, and polynomial kernels on a sample of the training set (these models used a randomly sampled subset of the training samples, specifically 10,000 of out ~40,000 samples, because they were quite slow to run)
2. LinearSVC was applied to the full training set, because Scikit-learn documentation says it is optimized for large datasets

> [!NOTE]
> Source code for the four SVMs can be found here:\
> [github.com/michael-van-vuuren/csci5612-workspace/models/supervised/svm-and-boosting.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/d577d96020be7ac2e551e71ffbf006b0dd59b00c/models/supervised/svm-and-boosting.ipynb)

Every model was ran with multiple regularization parameters (C). In every case, a regularization parameter of 10 was the best. This higher regularization parameter means that the SVMs allow for less missclassifications during training at the cost of smaller margins around its hyperplanes. The reason this parameter was not pushed up even more, say to 100, is because this would likely lead to overfitted models that are extremely sensitive to noise in the training set. 

An initial attempt was made to project the original dataset onto two dimensions using PCA and fit an SVM to visualize the decision boundaries. However, the first two principal components did not capture enough variance, resulting in visualizations where the points were grouped together and uninformative. To address this, three simulated examples were created to illustrate how decision boundaries might appear with different SVM kernels in a high-dimensional space.

The approach for plotting decision boundaries was based on:
https://scikit-learn.org/stable/auto_examples/svm/plot_svm_kernels.html

### SVC via Linear Kernel

SVC uses one-vs-one (OvO) for multiclass classification, which, for each class pair, trains a binary classifier that predicts if a sample is in one class or the other. This is true across all possible kernel selections.

#### Results
|    **Approach**   |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|-------------------|------------|-------------|----------|------------|
| Via Linear Kernel (C = 10.0) |   0.62   |   0.62    |  0.62  |   0.62   |

![Metrics](/images/svm/svclinear-metrics.png)

For every metric, the linear kernel SVM (C = 10) scored 0.62. It was the weakest out of all the models. It struggled especially with `Pop` and `Rap`. The linear hyperplanes formed by the linear kernel SVM seemed to struggle with separating the samples with these labels, because their lyrics were shared across many genres. This suggests that genre classification from lyrics is not easy to separate with just a straight line, and non-linear models might be able to find better patterns.

{{< tabs items="C = 10.0, C = 1.0, C = 0.1" >}}
{{< tab >}}
![ConfusionMatrix](/images/svm/svclinear-conf3.png)
{{< /tab >}}
{{< tab >}}
![ConfusionMatrix](/images/svm/svclinear-conf1.png)
{{< /tab >}}
{{< tab >}}
![ConfusionMatrix](/images/svm/svclinear-conf2.png)
{{< /tab >}}
{{< /tabs >}}

#### Example decision boundary

This uses simulated 2D data points to show the linear decision boundaries formed when SVM is used with a linear kernel. Keep in mind that Scikit-learn's SVC uses a one-vs-one multiclass classification strategy, which is why some of the colored regions have very complicated shapes (like `Rap`).

![DecisionBoundaryExample](/images/svm/linear-svm-ex.png)

### SVC via RBF Kernel

#### Results
|    **Approach**   |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|-------------------|------------|-------------|----------|------------|
| Via RBF Kernel (C = 10.0) |   0.69   |   0.70    |  0.69  |   0.69   |

![Metrics](/images/svm/svcrbf-metrics.png)

The RBF kernel SVM (C = 10) had the best performance overall, with an F1 Score of 0.69. It handled all genres decently, especially `Metal`, `Reggae`, and `Electronic`. It still struggled with `Pop` and `Rap`, however it did perform a little bit better than the linear kernel SVM. This shows that non-linear relationships between lyrics and genre exist, and the RBF kernel was able to capture them better.

{{< tabs items="C = 10.0, C = 1.0, C = 0.1" >}}
{{< tab >}}
![ConfusionMatrix](/images/svm/svcrbf-conf3.png)
{{< /tab >}}
{{< tab >}}
![ConfusionMatrix](/images/svm/svcrbf-conf1.png)
{{< /tab >}}
{{< tab >}}
![ConfusionMatrix](/images/svm/svcrbf-conf2.png)
{{< /tab >}}
{{< /tabs >}}

#### Example decision boundary

This uses simulated 2D data points to show the non-linear decision boundaries formed when SVM is used with an RBF kernel. Notice that the boundaries are rounded. This is why RBF kernel SVMs perform well on complicated datasets. 

![DecisionBoundaryExample](/images/svm/rbf-svm-ex.png)

### SVC via Polynomial Kernel

#### Results
|    **Approach**   |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|-------------------|------------|-------------|----------|------------|
| Via Polynomial Kernel (deg = 2, C = 10.0) |   0.67   |   0.68    |  0.67  |   0.67   |

![Metrics](/images/svm/svcpoly-metrics.png)

The polynomial kernel SVM (C = 10) performed better than the linear kernel SVM, but slightly worse than the RBF kernel SVM, with an F1 Score of 0.67. Its performance suggests that it was able to capture some of the non-linear patterns, but not as well as the RBF kernel. This makes sense because the polynomial kernel with a degree of two is less flexible than the RBF kernel.

{{< tabs items="C = 10.0, C = 1.0, C = 0.1" >}}
{{< tab >}}
![ConfusionMatrix](/images/svm/svcpoly-conf3.png)
{{< /tab >}}
{{< tab >}}
![ConfusionMatrix](/images/svm/svcpoly-conf1.png)
{{< /tab >}}
{{< tab >}}
![ConfusionMatrix](/images/svm/svcpoly-conf2.png)
{{< /tab >}}
{{< /tabs >}}

#### Example decision boundary

This uses simulated 2D data points to show the non-linear decision boundaries formed when SVM is used with a polynomial kernel of degree 2. Notice that the boundaries are still rounded, but less than the RBF SVM. 

![DecisionBoundaryExample](/images/svm/poly-svm-ex.png)

### LinearSVC

LinearSVC uses one-vs-rest (OvR) for multiclass classification, which, for each class, trains a binary classifier that predicts if a sample is in the class or not. This is faster than the OvO strategy used by SVC, but it can sometimes cause worse classification performance.

#### Results
|    **Approach**   |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|-------------------|------------|-------------|----------|------------|
| Via LinearSVC (C = 10.0) |   0.66   |   0.66    |  0.66  |   0.66   |

![Metrics](/images/svm/linearsvc-metrics.png)

The LinearSVC SVM (C = 10) had an F1 Score of 0.66, which was slightly better than the earlier SVM with a linear kernel, which had an F1 Score of 0.62. The training was a lot faster than the first linear kernel SVM, which makes sense because it uses a one-vs-rest multiclass classification strategy instead of one-vs-one. This means it has to train a lot less binary classifiers. LinearSVC is also specifically optimized for larger datasets.

{{< tabs items="C = 10.0, C = 1.0, C = 0.1" >}}
{{< tab >}}
![ConfusionMatrix](/images/svm/linearsvc-conf3.png)
{{< /tab >}}
{{< tab >}}
![ConfusionMatrix](/images/svm/linearsvc-conf1.png)
{{< /tab >}}
{{< tab >}}
![ConfusionMatrix](/images/svm/linearsvc-conf2.png)
{{< /tab >}}
{{< /tabs >}}

#### Interpretation

The LinearSVC weights show which words push the model toward or away from each genre, making the model easy to interpret. 

![Interpretation](/images/svm/interpretation.png)

> [!NOTE]
> There are some non-English words that should have been filtered out during preprocessing, but language detectors are not perfect. The results are still quite good.

The patterns make sense:
- **Country** avoids aggressive words like *idiot*, and favors words like *honki* (stemmed version of honky). It seems that some Spanish songs were tagged with country.
- **Metal** includes dark, intense words like *damnat* (stemmed version of damnation).
- **Rap** strongly weighs terms like *pimp* and *brooklyn*.
- **Pop** seems to be dominated by Spanish songs, with words like *loca* and *bella*.
- **Electronic** favors words like *virus*, and avoid words like *farm* and *jean*.
- **Jazz** has some French words like *trist* (stemmed version of triste, meaning sad in French), as well as some sophisticated words like *momento*.
- **Reggae** shows its Caribbean influence with the terms *inna* and *alleluia*.

This confirms the model is actually picking up meaningful genre information from lyrics.

![34](/images/intro-eda/34.png)



## Conclusions

Support Vector Machines were decent at predict a song's genre based on its lyrics, especially with non-linear kernels. The RBF kernel performed the best, compared to the linear and polynomial kernels, which suggests that the feature space requires non-linear hyperplanes. For practical use, LinearSVC trained faster, while still obtaining decent results, which would be useful if the model's training set was regularly updated, or updated in real time. These lyric to genre models can be used in the future to tag songs based on their lyrics, which could improve other classification models. In the next section, ensemble learning is used to capture the patterns in the lyrics even better than these SVMs were able to. 
