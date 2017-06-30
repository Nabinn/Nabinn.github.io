---
layout: post
title: Classifier to estimate the surviving chance of Titanic passengers
image: /img/hello_world.jpeg
tags: [machine learing, classification]
---

## Overview of data
On April 15, 1912, the Titanic sank after colliding with an iceberg, killing 1502 out of 2224 passengers and crew. Although there was some element of luck involved in surviving the sinking, some groups of people were more likely to survive than others, such as women, children, and the upper-class. In this tutorial, we carry an analysis to find out who these people are.

The dataset can be downloaded from kaggle:

<a href="https://www.kaggle.com/c/titanic/data">click here to download data</a>

The data has been split into two groups:
training set (train.csv)
test set (test.csv)
The training set should be used to build your machine learning models. For the training set, we provide the outcome (also known as the “ground truth”) for each passenger. Your model will be based on “features” like passengers’ gender and class. You can also use feature engineering to create new features.

The test set should be used to see how well your model performs on unseen data. For the test set, we do not provide the ground truth for each passenger. It is your job to predict these outcomes. For each passenger in the test set, use the model you trained to predict whether or not they survived the sinking of the Titanic.



## Dataset
Let's take a look at the datase. For each passenger, the following information are provided:
```
VARIABLE        DESCRIPTIONS:
PassengerId     Id of passenger
Survived        Survived
                (0 = No; 1 = Yes)
Pclass          Passenger Class
                (1 = 1st; 2 = 2nd; 3 = 3rd)
Name            Name
Sex             Sex
Age             Age
SibSp           Number of Siblings/Spouses Aboard
Parch           Number of Parents/Children Aboard
Ticket          Ticket Number
Fare            Passenger Fare
Cabin           Cabin number
Embarked        Port of Embarkation
                ( C = Cherbourg, Q = Queenstown, S = Southampton )
```

Here are some samples extracted from the dataset:

<div style="overflow-x: auto">
| survived | pclass | name | sex | age | sibsp | parch | ticket | fare |
| -------- | ------ | ---- | --- | --- | ----- | ----- | ------ | ---- |
|1|1|Aubart, Mme. Leontine Pauline|female|24|0|0|PC 17477|69.3000|
|0|2|Bowenur, Mr. Solomon|male|42|0|0|211535|13.0000|
|1|3|Baclini, Miss. Marie Catherine|female|5|2|1|2666|19.2583|
|0|3|Youseff, Mr. Gerious|male|45.5|0|0|2628|7.2250|
</div>
There are 2 classes in our task 'not survived' (class 0) and 'survived' (class 1), and the passengers data have 8 features.
