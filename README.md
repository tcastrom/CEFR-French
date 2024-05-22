
## Overview

This Kaggle contest involves a dataset of 4,200 sentences with their respective difficulty labels. Additionally, we were provided with 1,200 unlabeled sentences for the contest submission.

### Data and Features

We enhanced our dataset using the `textstat` package to generate additional features. Specifically, we utilized the VIF (Variance Inflation Factor) to select the three least correlated measures due to the high correlation among indicators in `textstat`. The selected features are:
- Flesch Reading Ease Index
- Liau Index
- Polysyllable Count

Furthermore, we incorporated features from the Open Lexicon online library, such as:
- Frequency of word appearance in films and books
- Number of letters and syllables

These additional features were averaged per sentence to improve our model, as they provide quantitative data not captured by IT-DF or CamemBERT tokenization.

### Initial Models

We started with basic classification models, including logistic regression and KNN. Initially, we used only the sentences tokenized using TF-IDF, which measures word importance within a document and adjusts for general frequency. However, these models performed poorly:
- **Best accuracy:** 48% on 480 randomly chosen labeled sentences
- **Kaggle contest accuracy:** 45%

The poor performance is attributed to the models relying solely on word frequency, making it difficult to distinguish between similar difficulty levels. Specifically, the A2 class had less than 36% accuracy due to nuances between A2, A1, and B1 levels.

### Improved Models

To address these shortcomings, we integrated readability metrics and quantitative word information. While models using only these metrics were competitive with TF-IDF-based models, the best accuracy achieved was 43%.

Combining both approaches yielded better results:
- **Combined model accuracy:** 50% on the 480 testing sentences

### CamemBERT Implementation

Our initial models struggled to encapsulate word meanings and sentence structures. To overcome this, we used text embeddings with CamemBERT, a model trained on 138 GB of data, significantly more than our initial models.

CamemBERT allows tokenization of data using its special tokenizer. Utilizing `CamembertForSequenceClassification`, we trained a model to classify the difficulty of French sentences. Using Optuna for hyperparameter tuning, we achieved:
- **58% accuracy** on the Kaggle contest

To further improve, we created a custom model incorporating both sentences and readability metrics. Using Wandb for hyperparameter optimization, we achieved:
- **62% accuracy**

The final model, available [here](#), utilizes this approach. Recreating the model from scratch may require a paid Colab Pro account and additional credits due to the computation time (around 8 hours on a GPU A100).

---

For a detailed breakdown of all models and results, please refer to the tables below. All models and their respective results can be found [here](#).
