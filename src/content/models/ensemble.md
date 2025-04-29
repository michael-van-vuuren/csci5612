---
title: Ensemble
type: docs
prev: models/svm
next: models/_index
weight: 8
---

## Overall Goal

This section builds on the previous Support Vector Machine (SVM) section, which examined the performance of SVMs classifying songs by genre based on their lyrics. In that section, three kernels: linear, radial basis function (RBF), and polynomial, were tested. Here, the performance of a gradient boosted model that uses an ensemble of decision trees is tested.



## Overview

Boosting is an ensemble machine learning technique that combines weak models in an "ensemble" to produce a strong model. It works by training the weak models in series. Each new weak model is trained on the misclassifications of previous weak models. To make a prediction, the prediction is passed through each weak model, and their predictions are weighted according to their accuracy on the training set.

![Boosting](/images/ensemble/boosting.png)

[Image source](https://corporatefinanceinstitute.com/resources/data-science/boosting/)

Gradient boosting is a type of boosting that uses gradient descent to minimize a loss function. Instead of focusing only on misclassifications (like AdaBoost), each new model is trained to correct the errors (residuals) made by the combined previous models. Gradient boosting is more flexible and often more accurate than other boosting algorithms. Some popular gradient boosting libraries are XGBoost and LightGBM. In this section, LightGBM is used for its fast training speed.

![GradientBoosting](/images/ensemble/gradient-boosting.png)

[Image source](https://www.linkedin.com/pulse/mastering-gradient-boosting-machine-learning-guide-pratik-thorat/)

## Data Preparation

The same data preparation used in the SVM section was applied here: [SVM Data Preparation {{< icon "newtab" >}}](https://michael-van-vuuren.github.io/csci5612/models/svm/#data-preparation). While the overall 80/20 train-test split remained the same for fair comparison, an additional split was made within the training set in this section. This formed a slightly smaller training set and a validation set. The validation set was used by the ensemble model to validate trees during training. Final model performance was still evaluated on the same test set used in the SVM section.

![Split](/images/ensemble/train-test-validate.png)

[Image source](https://medium.com/@rahulchavan4894/understanding-train-test-and-validation-dataset-split-in-simple-quick-terms-5a8630fe58c8)



## Modeling 

Ensemble learning was applied via boosting (used LightGBM, since it was faster than XGBoost). 

> [!NOTE]
> Source code for the boosted model can be found here:\
> [github.com/michael-van-vuuren/csci5612-workspace/models/supervised/svm-and-boosting.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/d577d96020be7ac2e551e71ffbf006b0dd59b00c/models/supervised/svm-and-boosting.ipynb)

A learning rate of 0.05 was a good balance between speed and performance, and model complexity was balanced with performance by experimenting with various `num_leaves` and `n_estimators` parameters. 

The model used the validation set during training and the test set afterward. This model was likely overfitted. Performance would need to be verified using cross-validation (it does perform well on the test set which it did not see at all during training though). 

These pages were used for reference:
1. https://www.kdnuggets.com/2023/07/lgbmclassifier-gettingstarted-guide.html
2. https://lightgbm.readthedocs.io/en/latest/pythonapi/lightgbm.LGBMClassifier.html
3. https://www.kaggle.com/code/marychin/num-leaves-min-data-in-leaf-a-lightgbm-demo

## Results
|    **Approach**   |**Accuracy**|**Precision**|**Recall**|**F1 Score**|
|-------------------|------------|-------------|----------|------------|
| Via LightGBM |   0.88   |   0.88    |  0.88  |   0.88   |

![Metrics](/images/ensemble/lightgbm-metrics.png)

LightGBM performed very well, with an F1 Score of 0.88. Its performance was great across genres, with `Electronic` having an F1 Score of 0.96. `Pop` performed the worst, except still did well, with an F1 Score of 0.79. It also performed better than all of the SVMs. This makes sense because decision trees are non-linear, and the ensemble learning approach allows the model to learn from its mistakes during training.

Although the model had very good classification accuracy, it is likely somewhat overfitted. As mentioned earlier, `num_leaves` and `n_estimators` were tweaked until a balance between performance and complexity was found. The final model has a `num_leaves` parameter of 40, and a `n_estimators` parameter of 500. This means that each decision tree in the ensemble can have at most 40 leaves and that 500 decision trees are trained in sequence, both of which are somewhat high. It is clear from testing that `Electronic` is the model's default prediction, which makes sense because it had the highest F1 Score out of all the classes. To lessen the model's bias toward `Electronic`, it could be penalized in the cost function. 

![ConfusionMatrix](/images/ensemble/lightgbm-conf.png)



## Test on Unseen Samples

The real-world performance of the LightGBM model can be tested by making it predict genre on lyrics it has never seen before. Here, short snippets lyrics are created and fed into the model. First, they are preprocessed in the exact same way the original training samples were. Once they have been vectorized, the LightGBM model accepts them as input, and outputs a genre prediction. It is reassuring to see that the predictions make sense, based on the lyrics. 

{{% details title="Expand to view source code" closed="true" %}}

```python {filename=""}
import re
from nltk.stem import PorterStemmer

# the input needs to be stemmed to match the training data
stemmer = PorterStemmer()
def preprocess(text):
    text = text.lower()
    text = re.sub(r'[^\w\s]', '', text)
    words = text.split()
    stemmed_words = [stemmer.stem(word) for word in words]
    return ' '.join(stemmed_words)

examples = []
# pop
examples.append(['driving through the mountains, playing guitar on a summer night'])
# country
examples.append(['driving my truck through the mountains, playing guitar on a summer night'])
# metal
examples.append(['thunder rains down, blood pours from the sky'])
# rap
examples.append(['hustling through the streets of brooklyn'])
# jazz
examples.append(['train barrels past, autumn leaves, beautiful love mystery'])
# reggae
examples.append(['sunshine smiles, its a beautiful day in jamaica'])
# electronic
examples.append(['dancing in space with aliens'])

for example in examples:
    # preprocess the unseen sample
    unseen_clean = [preprocess(lyric) for lyric in example]

    unseen_bow = count_vectorizer.transform(example)
    unseen_tfidf = tfidf_transformer.transform(unseen_bow)
    unseen_tfidf = normalize(unseen_tfidf)

    # predict
    predicted = model.predict(unseen_tfidf)
    print(f'\nLyrics: {example}')
    print(f'Predicted genre: {predicted[0]}')
```

{{% /details %}}

![UnseenExample](/images/ensemble/unseen.png)

> [!NOTE]
> These examples are somewhat cherry-picked. Often, the model would output `Electronic` as its prediction, even when the lyrics were deliberately created to represent a different genre. This makes sense given the model's bias toward `Electronic`, which could be corrected if desired.

## Conclusions

LightGBM was highly effective at predicting a song’s genre based on its lyrics, outperforming previous SVM approaches. With tuning of parameters like `num_leaves` and `n_estimators`, LightGBM performed well across all genres, especially *Electronic*. However, the model showed some signs of overfitting and bias, and often favored *Electronic* as its default prediction. Despite this, its performance on unseen lyrics was quite good. These results suggest that ensemble methods can capture complex patterns in lyric data more effectively than SVMs. In future work, balancing genre predictions and improving generalization could make these models even more practical for real-world applications like automated genre tagging or music recommendation systems.
