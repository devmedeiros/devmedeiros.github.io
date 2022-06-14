---
title: Feature Selection
date: 2022-06-14 19:23:00 -0300
categories: [Blog]
tags: [feature selection, scikit-learn]
showtoc: true
---

# What is feature selection?

Feature selection is the process of finding pertinent features and eliminating unnecessary, redundant, or noisy data. It can be useful to help the training process be faster and it can also avoid overfitting. According to Munson and Caruana, the goal of feature selection is to identify the feature set that best balances the risks of having too few features with the risks of having too many features. It can also help improve the prediction accuracy and the interpretation of models.[^1] [^2]

I chose three methods for feature selection to show, but these are not all available methods, in the `scikit-learn` package that are 17 algorithms implemented at the time of writing. [^3]

# Correlation among features

Correlation among features can bring unnecessary noise to your model. Because a high correlation means that when one feature increases the other increases as well (or decreases if is negative). So if two features have a high correlation they aren’t bringing any new information to our model, in this case, is best to choose one of them to keep and drop the other.

Look at the following correlation matrix, for example:

![small correlation plot with weight, height and BMI](https://ik.imagekit.io/devmedeiros/out_w4T4rFn5h.png?ik-sdk-version=javascript-1.4.3&updatedAt=1655244061311#center)

We can see that height and weight are heavily correlated with BMI, we don’t need to keep these three features, we can drop height and weight and keep just the BMI, or depending on what you want with the model you just drop the BMI.

# Low Variance

Removing features with low variance is a simple method of feature selection. You can set a minimal variance a feature needs to determine if it continues in the model or not. The reasoning behind this is that a low variance means it has a small effect on the final model.

Although there isn't concrete information on what is a "low variance", the `scikit-learn` default option for this method is 0, but you can test other values for your dataset and see what works best for you.

# Recursive feature elimination

The purpose of recursive feature elimination (RFE) is to pick features by iteratively considering smaller and smaller sets of features given an external estimator that assigns weights to features. The importance of each feature is first determined through any particular attribute, and the estimator is then trained on the initial set of features. The least crucial features are then removed from the present list of features. On the pruned set, that process is continued recursively until the target number of features to select is eventually attained. [^4]


[^1]: [MUNSON, M. Arthur; CARUANA, Rich. On feature selection, bias-variance, and bagging. In: **Joint European Conference on Machine Learning and Knowledge Discovery in Databases**. Springer, Berlin, Heidelberg, 2009. p. 144-159.](https://link.springer.com/chapter/10.1007/978-3-642-04174-7_10)

[^2]: [KUMAR, Vipin; MINZ, Sonajharia. Feature selection: a literature review. **SmartCR**, v. 4, n. 3, p. 211-229, 2014.](https://faculty.cc.gatech.edu/~hic/CS7616/Papers/Kumar-Minz-2014.pdf)

[^3]: [API Reference — scikit-learn 1.1.1 documentation](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.feature_selection)

[^4]: [1.13. Feature selection — scikit-learn 1.1.1 documentation](https://scikit-learn.org/stable/modules/feature_selection.html)