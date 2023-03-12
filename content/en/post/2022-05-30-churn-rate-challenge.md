---
title: Data Science Challenge - Churn Rate
date: 2022-05-30 16:49:00 -0300
lastmod: 2022-06-09 18:08:00 -0300
categories: [Blog]
tags: [storytelling, python, data analysis, machine learning, neural network]
showtoc: true
summary: Alura hosted a four-week Data Science Challenge using an imbalanced dataset of Churn Rate of a company Alura Voz
cover:
    image: "https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/aluravoz.png"
    alt: "heart with an A inside and you can read 'Alura Voz telecommunication company'"
---

I was challenged to take the role of a new data scientist hired at Alura Voz. This made-up company is a telecommunication company and it needs to reduce the Churn Rate.

The challenge is divided into four weeks. For the first week, the goal was to clean the dataset provided by an API. Next, we need to identify clients who are more likely to leave the company, using data exploration and analysis. Then, in the third week, we made machine learning models to predict the churn rate for Alura Voz. The last week is to show off what we made during the challenge and build our portfolio. In case you are interested in seeing the code for the challenge just head over to my GitHub [repository](https://github.com/devmedeiros/Challenge-Data-Science).

## First Week

### Reading the dataset

The dataset is available in a JSON file, at first glance it looked like a normal data frame.

![table head with the first five rows](https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/1%20-%20Data%20Cleaning/table_head.png#center)

But, as we can see, `customer`, `phone`, `internet`, and `account` are their own separate table. So I had to normalize them separately and then I just concatenated all these tables into one.

### Missing data

The first time I looked for missing data in this dataset I notice that apparently, that wasn't anything missing, but later on, I noticed that there was empty space and just space not being counted as `NaN`. So I corrected this, and now the dataset had 224 missing values for `Churn` and 11 missing for `Charges.Total`.

I decided to drop the missing `Churn` because this is going to be the object of our study and there isn't a point in studying something that doesn't exist. For the missing `Charges.Total`, I think it represents a customer that hasn't paid anything yet, because all of them had a tenure of 0, meaning that they had just become a client, so I just replaced the missing value for 0.

### Feature Encoding

The feature `SeniorCitizen` was the only one that came with `0` and `1` instead of `Yes` and `No`. For now, I'm changing it to yes and no, because it'll make the analysis simpler to read.

`Charges.Monthly` and `Charges.Total` were renamed to lose the dot because the dot gets in the way when calling the feature in python.

## Second Week

### Data Analysis

In the first plot, we can see how much unbalanced our data set is. There're over 5000 clients that didn't leave the company and a little less than 2000 that left.

![bar plot with two bars, the first one is for 'no' and the second is for 'yes', the first bar is over 5000 count and the second one is around 2000](https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/2%20-%20Data%20Analysis/churn.jpg#center)

I experimented with oversampling the dataset to handle this imbalance, but it made the machine learning models worse. And undersampling isn't an option with this dataset size, so I just decided to leave it the way it is, and when it's time to split the training and test set I'll stratify the dataset by the `Churn` feature.

I also generated 16 plots for all the discrete data, to see all the plots check this [notebook](https://github.com/devmedeiros/Challenge-Data-Science/blob/main/2%20-%20Data%20Analysis/data_analysis.ipynb). I wanted to see if there was any behavior that made some clients more likely to leave the company. Is clear that all, except for `gender`, seems to play a role in determining if a client will leave the company or not. More specifically payment methods, contracts, online backup, tech support, and internet service.

In the `tenure` plot, I decided to make a distribution plot for the tenure, one plot for clients that didn't churn and another for the clients that did churn. We can see that clients that left the company tend to do so at the beginning of their time in the company.

![there are two plots side-by-side, in the first one the title is 'Churn = No' the data is along the tenure axis and is in a U shape. the second plot has the title 'Churn = Yes' and starts high and drops fast along the tenure line](https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/2%20-%20Data%20Analysis/tenure.jpg#center)

The average monthly charge for clients that didn't churn is 61.27 monetary units, while clients that churn were paying 74.44. This is probably because of the type of contract they prefer, but either way is known that higher prices drive the customers away.

### The Churn Profile

![person jumping through the window](https://64.media.tumblr.com/tumblr_lojvnhHFH91qlh1s6o1_400.gifv#center)

Considering everything that I could see through plots and measures. I came up with a profile for clients that are more likely to churn.

- New clients are more likely to churn than older clients.

- Customers that use fewer services and products tend to leave the company. Also, when they aren't tied down to a longer contract they seem to be more likely to quit.

- Regarding the payment method, clients that churn have a **strong** preference for electronic checks and usually are spending 13.17 monetary units more than the average client that didn't leave.

## Third Week

### Preparing the dataset

We start by making dummies variables dropping the first, so we would have n-1 dummies for n categories. Then we move on to look at features correlation.

![correlation matrix with all the features](https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/3%20-%20Model%20Selection/corr_matrix.jpg#center)

We can see that the `InternetService_No` feature has a lot of strong correlations with many other features, this is because these other features depend on the client having internet service. So I'll drop all features that are dependent on this one. The same thing happens with `PhoneService_Yes`.

`tenure` and `ChargesTotal` also have a strong correlation, so I tried running the models without one of them and both, and it had a worse performance and took a long time to converge, so I decided to keep them as they are relevant as well.

After dropping the features I finish preparing the dataset by normalizing the numeric data, `ChargesTotal` and `tenure`.

### Test and training dataset

I split the dataset into training and testing sets, 20% for testing and the rest for training. I stratified the data by the `Churn` feature and I shuffle the dataset before splitting. The same split is used by all the models. After splitting the dataset I decided to oversample the **train** data using SMOTE[^1] because the dataset is imbalanced. The reason that I only used this technique on the training set is that I don't want to have a biased result, oversampling all the datasets would mean that I'd be testing my models on the same data that I trained, and that's not the goal here.

### Model Evaluation

I'll use a dummy classifier to have a baseline model for the accuracy score, and I'll also use the metrics: `precision`, `recall` and `f1 score`[^2]. Although the dummy model won't have values for this metrics, I'll keep it for comparison on how much the models improved.

### Baseline

I made the baseline model using a dummy classifier that guessed that every client behaved the same. It is always guessed that no client will leave the company. By using this approach we got a baseline accuracy score of `0.73456`.

All models moving forward will have the same random state.

### Model 1 - Random Forest

I start by using a grid search with cross-validation to find the best parameters within a given pool of options using the `recall` as the strategy to evaluate the performance. The best model was:

```python
RandomForestClassifier(criterion='entropy', max_depth=5, max_leaf_nodes=70, random_state=22)
```

After fitting this model, the evaluating metrics were:
- Accuracy Score: 0.72534 
- Precision Score: 0.48922 
- Recall Score: 0.78877 
- F1 Score: 0.60389

### Model 2 - Linear SVC

For this model, I just used the default parameters and set the ceiling for the maximum of iterations to `900000`.

```python
LinearSVC(max_iter=900000, random_state=22)
```

After fitting this model, the evaluating metrics were:
- Accuracy Score: 0.71966 
- Precision Score: 0.48217 
- Recall Score: 0.75936 
- F1 Score: 0.58982

### Model 3 - Multi-layer Perceptron

Here I fixed the solver to LBFGS, because according to the documentation it has a better performance in smaller datasets[^3], and used grid search with cross-validation to find a hidden layer size that would be the best. The best model was:

```python
MLPClassifier(hidden_layer_sizes=(1,), max_iter=9999, random_state=22, solver='lbfgs')
```

After fitting this model, the evaluating metrics were:
- Accuracy Score: 0.72818 
- Precision Score: 0.49133 
- Recall Score: 0.68182 
- F1 Score: 0.57111

### Conclusion

After running the three models, all of them used the same random_state. I got the following accuracy scores and improvements (compared to the baseline model):

![results table](https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/3%20-%20Model%20Selection/results_table.png#center)

In the end, the Random Forest had the best metrics overall. This model can _recall_ a great portion of clients that churn correctly, still is not perfect but is certainly a starting point. The _accuracy_ score is not as high as I'd like, but in this particular problem, the goal is to keep clients from leaving the company and is better to use resources to keep a client that will not leave than to do nothing.

In the end, I liked this challenge, because I don't usually practice machine learning, but thanks to the challenge I got the chance to make a small project in this area that is so relevant and important. This was my first time working with neural networks and tunning hyper-parameters, and I'm sure the next time I'll get even better results.

[^1]: [imbalanced-learn documentation](https://imbalanced-learn.org/stable/references/generated/imblearn.over_sampling.SMOTE.html)
[^2]: [Accuracy, Precision, Recall or F1? - Koo Ping Shung](https://towardsdatascience.com/accuracy-precision-recall-or-f1-331fb37c5cb9)
[^3]: [scikit-learn documentation](https://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html)