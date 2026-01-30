---
title: "Learning Machine Learning and Kaggle's Playground Series"
date: 2026-01-29
tags:
---

<h3>Overview</h3>

For this project, I participated in a Kaggle competition with the goal of learning the basics of **machine learning**.  
The task was to build a model that could **predict student test scores** based on a given dataset.

Because of this, I shared ideas, discussed approaches with my peers, learned machine learning and Kaggle terminology (e.g. overfitting, CV/LB score, encoding), and used AI tools such as ChatGPT to create and enhance a predictive model. More importantly, I learned the process of managing data, training models, and gradually improving their accuracy.

Kaggle Competition Results on Public Leaderboard (as of now):
- Kaggle Username: lilrayray
- Place: 916/3929
- Best Score: 8.69287

---

<h3>Strategy & Approach</h3>

My general approach looked like this:

1. **Explored the dataset**
   - Created graphs to show correlations
   - Checked data types
   - Identified important features

2. **Preprocessed the data**
   - Handled missing values
   - Encoded categorical variables
   - Scaled numerical features when necessary
     
3. **Engineered new features**
   - Created interaction features
       - Example: Sleep Score, which is hours of sleep multiplied by 1, 2, or 3 depending on sleep quality (higher quality = higher multiplier)
   - Created square features

4. **Model selection**
   - Started with a simple notebook using LightGBM
       - Found that creating a prediction then adding it back into the features to create another prediction worked well
   - Experimented with a more advanced model
       - Tried to use a neural network and failed miserably
   - Opted for a simpler but effective approach by using multiple models
       - Inspired by a notebook found online, I trained LightGBM, XGBoost, CatBoost, and used Ridge Stacking to blend the models' predictions together (this worked the best)
         
5. **Evaluation**
   - Avoided overfitting by validating results
   - Submitted to Kaggle to check leaderboard score

---

<h3>What I Learned</h3>

- The importance of **Exploratory Data Analysis**
    - I never realized how important it is to find correlations (graphing also makes this proccess a whole lot easier)
- The importance of **data cleaning and preprocessing**
    - Not cleaning data might lead to overfitting or inaccurate predictions
    - Preprocessing needs to be done so that the models can read the data
- The basics on how some models work
- How Kaggle competitions are structured and evaluated

---

<h3>Challenges & Troubles</h3>

Some of the main challenges I ran into:

- Understanding how to deal with an unproccessed dataset
- Finding ways to improve my model (half the time my score would be worse)
- Understanding a lot of the code and terminology shown in the Kaggle discussions and the code page.

---

<h3>Results</h3>

While my model wasn’t perfect, it achieved a reasonable prediction accuracy and helped me understand the workflow of a machine learning project.

More importantly, I enjoyed having discussions with my peers in class and learning the process together.

---

<h3>Notebooks & Code</h3>

Exploratory Data Analysis
[01_eda.ipynb (GitHub)](../assets/01_eda.ipynb)

Feature Engineering
[02_feature_engineering.ipynb (GitHub)](../assets/02_feature_engineering.ipynb)

Final Model (Ridge + CatBoost + LightGBM + XGBoost)
[05_ridge_catlightxg_model.ipynb (GitHub)](../assets/05_ridge_catlightxg_model.ipynb)

---

<h3>Next Steps</h3>

- Experiment with additional models
- Explore neural networks
- Participate in more Kaggle competitions

