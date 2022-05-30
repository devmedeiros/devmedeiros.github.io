---
title: Data Science Challenge - Churn Rate
date: 2022-05-30 16:49:00 -0300
categories: [Projects]
tags: [storytelling, python, data analysis, machine learning]
showtoc: true
---

![heart with an A inside and you can read 'Alura Voz telecommunication company'](https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/aluravoz.png#center)

I was challenged to take the role of a new data scientist hired at Alura Voz. This made-up company is a telecommunication company and it needs to reduce the Churn Rate.

The challenge is divided into four weeks. For the first week, the goal was to clean the dataset provided by an API. Next, we need to identify clients who are more likely to leave the company, using data exploration and analysis. Then, in the third week, we made machine learning models to predict the churn rate for Alura Voz. The last week is to show off what we made during the challenge and build our portfolio. In case you are interested in seeing the code for the challenge just head over to my GitHub [repository](https://github.com/devmedeiros/Challenge-Data-Science).

# First Week

## Reading the dataset

The dataset is available in a JSON file, at first glance it looked like a normal data frame.

|  | customerID | Churn | customer | phone | internet | account |
|---|---|---|---|---|---|---|
| 0 | 0002-ORFBO | No | {'gender': 'Female', 'SeniorCitizen': 0, 'Part... | {'PhoneService': 'Yes', 'MultipleLines': 'No'} | {'InternetService': 'DSL', 'OnlineSecurity': '... | {'Contract': 'One year', 'PaperlessBilling': '... |
| 1 | 0003-MKNFE | No | {'gender': 'Male', 'SeniorCitizen': 0, 'Partne... | {'PhoneService': 'Yes', 'MultipleLines': 'Yes'} | {'InternetService': 'DSL', 'OnlineSecurity': '... | {'Contract': 'Month-to-month', 'PaperlessBilli... |
| 2 | 0004-TLHLJ | Yes | {'gender': 'Male', 'SeniorCitizen': 0, 'Partne... | {'PhoneService': 'Yes', 'MultipleLines': 'No'} | {'InternetService': 'Fiber optic', 'OnlineSecu... | {'Contract': 'Month-to-month', 'PaperlessBilli... |
| 3 | 0011-IGKFF | Yes | {'gender': 'Male', 'SeniorCitizen': 1, 'Partne... | {'PhoneService': 'Yes', 'MultipleLines': 'No'} | {'InternetService': 'Fiber optic', 'OnlineSecu... | {'Contract': 'Month-to-month', 'PaperlessBilli... |
| 4 | 0013-EXCHZ | Yes | {'gender': 'Female', 'SeniorCitizen': 1, 'Part... | {'PhoneService': 'Yes', 'MultipleLines': 'No'} | {'InternetService': 'Fiber optic', 'OnlineSecu... | {'Contract': 'Month-to-month', 'PaperlessBilli... |

But, as we can see, `customer`, `phone`, `internet`, and `account` are their own separate table. So I had to normalize them separately and then I just concatenated all these tables into one.

## Missing data

The first time I looked for missing data in this dataset I notice that apparently, that wasn't anything missing, but later on, I noticed that there was empty space and just space not being counted as `NaN`. So I corrected this, and now the dataset had 224 missing values for `Churn` and 11 missing for `Charges.Total`.

I decided to drop the missing `Churn` because this is going to be the object of our study and there isn't a point in studying something that doesn't exist. For the missing `Charges.Total`, I think it represents a customer that hasn't paid anything yet, because all of them had a tenure of 0, meaning that they had just become a client, so I just replaced the missing value for 0.

## Feature Encoding

The feature `SeniorCitizen` was the only one that came with `0` and `1` instead of `Yes` and `No`. For now, I'm changing it to yes and no, because it'll make the analysis simpler to read.

`Charges.Monthly` and `Charges.Total` were renamed to lose the dot because the dot gets in the way when calling the feature in python.

# Second Week

## Data Analysis

In the first plot, we can see how much unbalanced our data set is. There're over 5000 clients that didn't leave the company and a little less than 2000 that left.

![bar plot with two bars, the first one is for 'no' and the second is for 'yes', the first bar is over 5000 count and the second one is around 2000](https://github.com/devmedeiros/Challenge-Data-Science/blob/main/2%20-%20Second%20Week/churn_count.jpg?raw=true#center)

I experimented with oversampling the dataset to handle this imbalance, but it made the machine learning models worse. And undersampling isn't an option with this dataset size, so I just decided to leave it the way it is, and when it's time to split the training and test set I'll stratify the dataset by the `Churn` feature.

I also generated 16 plots for all the discrete data, I opted to make the grid 6x3 to make it better to read. I wanted to see if there was any behavior that made some clients more likely to leave the company. Is clear that all, except for `gender`, seems to play a role in determining if a client will leave the company or not. More specifically payment methods, contracts, online backup, tech support, and internet service.

![there are 16 bar plots in the image showing how each feature is correlated with the churn rate](https://github.com/devmedeiros/Challenge-Data-Science/blob/main/2%20-%20Second%20Week/categorical_churn.jpg?raw=true#center)

In the `tenure` plot, I decided to make a distribution plot for the tenure, one plot for clients that didn't churn and another for the clients that did churn. We can see that clients that left the company tend to do so at the beginning of their time in the company.

![there are two plots side-by-side, in the first one the title is 'Churn = No' the data is along the tenure axis and is in a U shape. the second plot has the title 'Churn = Yes' and starts high and drops fast along the tenure line](https://github.com/devmedeiros/Challenge-Data-Science/blob/main/2%20-%20Second%20Week/tenure_churn.jpg?raw=true#center)

The average monthly charge for clients that didn't churn is 61.27 monetary units, while clients that churn were paying 74.44. This is probably because of the type of contract they prefer, but either way is known that higher prices drive the customers away.

## The Churn Profile

![person jumping through the window](https://64.media.tumblr.com/tumblr_lojvnhHFH91qlh1s6o1_400.gifv#center)

Considering everything that I could see through plots and measures. I came up with a profile for clients that are more likely to churn.

- New clients are more likely to churn than older clients.

- Customers that use fewer services and products tend to leave the company. Also, when they aren't tied down to a longer contract they seem to be more likely to quit.

- Regarding the payment method, clients that churn have a **strong** preference for electronic checks and usually are spending 13.17 monetary units more than the average client that didn't leave.

# Third Week

## Preparing the dataset

We start by making dummies variables dropping the first, so we would have n-1 dummies for n categories. Then we move on to look at features correlation.

![correlation matrix with all the features](https://github.com/devmedeiros/Challenge-Data-Science/blob/main/3%20-%20Third%20Week/corr_matrix.png?raw=true#center)

We can see that the `InternetService_No` feature has a lot of strong correlations with many other features, this is because these other features depend on the client having internet service. So I'll drop all features that are dependent on this one. The same thing happens with `PhoneService_Yes`.

`tenure` and `ChargesTotal` also have a strong correlation, so I tried running the models without one of them and both, and it had a worse performance and took a long time to converge, so I decided to keep them as they are relevant as well.

After dropping the features I finish preparing the dataset by normalizing the numeric data, `ChargesTotal` and `tenure`.

## Test and training dataset

I split the dataset into training and testing sets, 20% for testing and the rest for training. I stratified the data by the `Churn` feature and I shuffle the dataset before splitting. The same split is used by all the models.

## Baseline

I made the baseline model using a dummy classifier that guessed that every client behaved the same. The dummy always guessed that no client will leave the company. By using this approach we got a baseline accuracy score of `0.73456`.

## Model 1 - Random Forest

I start by using a grid search with cross-validation to find the best parameters within a given pool of options. The best model the grid search found was:

```python
RandomForestClassifier(max_depth=15, max_leaf_nodes=75, random_state=22)
```

After fitting this model, the accuracy score was `0.78282`.

## Model 2 - Linear SVC

For this model, I just used the default parameters.

```python
LinearSVC(random_state=22)
```

After fitting, it got an accuracy score of `0.78992`.

## Model 3 - Multi-layer Perceptron

Here I fixed the solver to LBFGS, because according to the documentation it has a better performance in smaller datasets [^1], and used grid search with cross-validation to find a hidden layer size that would be the best. The best model was:

```python
MLPClassifier(hidden_layer_sizes=(1,), max_iter=9999, random_state=22, solver='lbfgs')
```

After fitting its accuracy score was `0.79489`.

## Conclusion

After running the three models, all of them used the same `random_state`. I got the following accuracy scores and improvements (compared to the baseline model):

| **Model** | **Acc Score** | **Improvement** |
|-----------|:-------------:|:---------------:|
| Baseline  |    0.73456    |        -        |
| Model 1   |    0.78282    |      6.57%      |
| Model 2   |    0.78992    |      7.54%      |
| Model 3   |    0.79489    |      8.21%      |

 The model with the highest accuracy is a neural network with a multi-layer perceptron with an 8.21% accuracy increase, compared to the baseline. Overall I think this is a good improvement given the amount of time and resources available.

 In the end, I liked this challenge, because I don't usually like to practice machine learning, but thanks to the challenge I got the chance to make a small project in this area that is so important and relevant. This was my first time working with neural networks and tunning hyper-parameters, and I'm sure the next time I'll get even better results.

[^1]: [sklearn documentation](https://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html)
