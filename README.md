![2](https://github.com/tcastrom/CEFR-French/assets/140407074/d9536274-54cf-4c3b-81cb-357cd579e2ce)


## Overview

This Kaggle contest involves a dataset of 4,200 sentences with their respective difficulty labels. Additionally, we were provided with 1,200 unlabeled sentences for the contest submission.

### Model's Presentation
## Video Presentation
[![Watch the video](https://img.youtube.com/vi/f1B7x2-sDzg/0.jpg)](https://youtu.be/f1B7x2-sDzg)

### Data and Features

We enhanced our dataset using the `textstat` package to generate additional features. Specifically, we utilized the VIF (Variance Inflation Factor) to select the three least correlated measures due to the high correlation among indicators in `textstat`. The selected features are:
- Flesch Reading Ease Index
- Liau Index
- Polysyllable Count

Furthermore, we incorporated features from the[Open Lexicon](http://openlexicon.fr/datasets-info/Lexique382/README-Lexique.html) online library, such as:
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

The final model, available [here](https://github.com/tcastrom/CEFR-French/tree/main/Models/CamemBERT/Models%20with%20readability%20metrics/saved%20model) , utilizes this approach. Recreating the model from scratch may require a paid Colab Pro account and additional credits due to the computation time (around 8 hours on a GPU A100).

---

For a detailed breakdown of all models and results, please refer to the tables in the model folder [here](https://github.com/tcastrom/CEFR-French/tree/main/Models). All models and their respective results can be found [here](https://github.com/tcastrom/CEFR-French/tree/main/Models).


# An interesting application of our model: EPFL to Paris Journey Game

## Overview

The **EPFL to Paris Journey** is an interactive game designed to help international students at EPFL improve their French language skills. The objective of the game is to write complex sentences in French within a set time limit to "travel" from EPFL in Lausanne to the Eiffel Tower in Paris. The game makes learning French fun and engaging, motivating students to put forth their best effort.

## Video Presentation
[![Watch the video](https://img.youtube.com/vi/GRkDAG-6Ers/0.jpg)](https://youtu.be/GRkDAG-6Ers))

### CEFR Levels

The **Common European Framework of Reference for Languages (CEFR)** is an international standard for describing language proficiency. It consists of six levels:
- **A1 (Beginner)**
- **A2 (Elementary)**
- **B1 (Intermediate)**
- **B2 (Upper Intermediate)**
- **C1 (Advanced)**
- **C2 (Proficient)**

Each level is designed to assess a learner's ability to perform various linguistic tasks. The levels are widely recognized and provide a clear benchmark for language proficiency. For more details on how CEFR levels are used in our models, refer to the [Initial Models](#initial-models) and [Improved Models](#improved-models) sections.

## How the Game Works

### Gameplay

1. **Starting the Game**: The game begins when the player presses the "Start the game!" button. They then have **90 seconds** to write as many complex French sentences as possible.
2. **Writing Sentences**: Players type their sentences into the provided text input field.
3. **Submitting Sentences**: After typing a sentence, players press the "Submit Sentence" button. The game then analyzes the sentence's complexity and assigns it a CEFR level using techniques similar to those described in the [CamemBERT Implementation](#camembert-implementation) section.
4. **Scoring**: Points are awarded based on the CEFR level of the sentence:
   - **A1**: 5 points
   - **A2**: 10 points
   - **B1**: 20 points
   - **B2**: 30 points
   - **C1**: 40 points
   - **C2**: 50 points

### Progress Tracking

- **Distance Calculation**: The total distance from EPFL to the Eiffel Tower is **517 kilometers**. Each point earned corresponds to 1 kilometer traveled.
- **Encouragement Messages**: The game provides motivational messages based on the player's progress:
  - Less than 100 kilometers: "**Keep going! You can do it!**"
  - 100 to 250 kilometers: "**Great job! You're halfway there!**"
  - 250 to 400 kilometers: "**You're making great progress! Almost there!**"
  - More than 400 kilometers: "**Just a little more! The Eiffel Tower is in sight!**"

## Why the Game is Interesting

### Educational Value

The game leverages the CEFR framework to provide a structured and standardized approach to language learning. By focusing on sentence complexity, it encourages players to think critically about their language use, promoting deeper learning. This aligns with our goal of improving model accuracy by integrating readability metrics and quantitative word information as discussed in the [Improved Models](#improved-models) section.

### Engagement

Gamifying the learning process keeps players motivated and engaged. The clear objectives and rewards system help maintain interest and encourage consistent practice, which is crucial for language acquisition. This approach is similar to how we enhanced our dataset using the `textstat` package and features from the [Open Lexicon](#data-and-features).

### Target Audience

This game is ideal for:
- **International Students**: Specifically designed for non-native French speakers studying at EPFL.
- **French Learners**: Anyone looking to improve their French proficiency through an interactive and enjoyable method.
- **Language Instructors**: Teachers can use the game as a supplementary tool to engage students and track their progress.

## Review and Learn

In addition to the gameplay, the "Review and Learn" page provides valuable learning opportunities:
- **Sentence History**: Players can review all the sentences they've submitted along with the points and CEFR levels assigned to each.
- **Common Phrases**: The page also includes the ten most frequently used sentences in Paris, helping players learn practical phrases they can use in real-life situations.

## How to Play

1. Navigate to the "Game" page.
2. Press the "Start the game!" button.
3. Write and submit as many complex French sentences as you can within 90 seconds.
4. Track your progress and aim to reach the Eiffel Tower by scoring 517 points.
5. Review your submitted sentences and learn from the "Review and Learn" page.


