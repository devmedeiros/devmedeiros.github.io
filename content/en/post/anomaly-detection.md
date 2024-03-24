---
title: What is Anomaly Detection?
date: 2023-07-09 16:32:00 -0300
categories: [Blog]
keywords: [anomaly detection, fraud detection, outliers, class imbalance, data analysis, machine learning]
tags: [Data Analysis, Machine Learning]
showtoc: false
summary: How can your credit card issuer know if a purchase is made by you or a cloned version of your credit card?
cover:
    image: "https://ik.imagekit.io/devmedeiros/outlier_kHqTJ7_Iv.webp?tr=w-700"
    alt: "five diferent apples on a white table"
    caption: "Photo by Isabella Fischer on Unsplash"
---

Have you ever had a purchase blocked by your credit card issuer without you taking any action? If this happened to you, your credit card issuer probably had detected that your purchase didn't match your _normal spending habit_, maybe you were at a different location or spend a lot of money suddenly. Anyway, the credit card company was using **anomaly detection** techniques to prevent fraudulent activities.

## What is Anomaly Detection?

> _Anomaly detection_ refers to the problem of finding patterns in data that do not conform to expected behavior. [^1]

In statistics there is a famous distribution that is called "The Normal Distribution", this name is not in vain. A lot of random things are normally distributed, for instance, if you select 100 random people and measure their height, and made a distribution plot, you'd end up with something looking like a symmetrical bell curve.

Even though abnormal results are possible, it is expected that most people or things will fall within a normal distribution. When something is divergent from this normality it is said to be an outlier or an anomaly.

When dealing with something like credit card fraud you can create an expectation of a person expanding habits, so if your daily routine consists of waking up and driving to your workplace and buying a coffee, then lunch in New York. Your credit card company would find it weird if you suddenly made a physical purchase in Rio de Janeiro.

In simple terms, that is what anomaly detection is trying to do, understand how something works so it can identify if it starts acting weird.

## Anomaly Detection x Class Imbalance in Datasets

If you are aware of what class imbalance is, you may be wondering _"can't I use anomaly detection to solve this class imbalance problem?"_ - sometimes you can, but it's not appropriate.

Class imbalance occurs when one class has significantly more samples than the other class(es). This can lead to biased models that favor the majority class and perform poorly on minority classes. While anomaly detection focuses on identifying rare or abnormal data points that deviate significantly from the normal patterns in the dataset. 

Even though it has some overlap on what is trying to do, unless you have a class imbalance problem that the minority class can be considered an abnormal behavior, you won't be able to use anomaly detection.

---

I talked about class imbalance before on a churn rate problem in a fictitious telecommunication company, if you are interested you can check it [here](/post/2022-05-30-churn-rate-challenge/).

[^1]: Chandola, V., Banerjee, A., and Kumar, V. 2009. Anomaly detection: A survey. ACM Comput. Surv. 41, 3, Article 15 (July 2009), 58 pages. DOI = [10.1145/1541880.1541882](http://doi.acm.org/10.1145/1541880.1541882)