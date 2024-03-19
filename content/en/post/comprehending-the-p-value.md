---
title: Comprehending the P Value
date: 2022-04-12 17:58:00 -0300
categories: [Blog]
tags: [statistic, p value]
showtoc: true
summary: P-value definition, how to use it, and interpreting the measurement
aliases:
- 2022-04-12-comprehending-the-p-value
cover:
    image: "https://ik.imagekit.io/devmedeiros/guy-working_2J4TgmjhL.webp?tr=w-700"
    alt: men writing on a notebook next to a laptop
    caption: Image of StockSnap by Pixabay
---

## What is the p-value?

In statistics, we have hypothesis tests, which are made to decide whether to reject or *not* reject the null hypothesis. Some examples of hypothesis testing are Neyman-Pearson, Shapiro-Wilk, Student's T-test, and others.

All hypothesis tests have a test statistic specific to them. And this statistic is used to evaluate the test, but this task can be tiring even with the use of computers because most software does not return the comparison statistic as an output, only the sample statistic. In this case, the p-value comes to facilitate this comparison, as it is already a representation of this test statistic. It represents the probability of obtaining a test statistic equal to or more extreme than the one calculated in your sample, considering the null hypothesis to be true.

This makes it easier because by knowing the confidence level that you want to test your hypothesis, you just need to compare if the p-value is smaller or greater than your confidence level, while if you were to use the test statistic it would still be necessary to calculate the statistic for each different confidence level than you would compare. So suppose you want to compare 1%, 5%, and 10%, you would have to calculate three different test statistics to compare your sample statistic.

## How to use it?

It is very common when we are learning statistics on our own, to read that if the p-value is less than 5% we reject the hypothesis or that if it is higher we "accept" the hypothesis.

The value with which we compare the p-value must be defined together with people from the business area you are working with, it is very common in some sectors to use a very small p-value such as 1% or even 0.1% and in others use values ​​greater than 5%.

Note that a p-value of 0.02 would be rejected if we consider `α = 1%`, but not if `α = 5%`. When in doubt about which one to use, it is first recommended to make this decision before taking the test. Second, keep in mind that an `α = 1%` will have a confidence of `99% (1-α)`, whereas if it were 5% it would be ~~only~~ 95%. It might seem like it's best to take the value that gives you the most "confidence", but a very small p-value can lead to more rejections of your hypothesis.

The truth is that hypothesis tests only give us the rejection information, when a null hypothesis is not rejected, it means that no evidence was found that contradicts what it claims, it does not mean that we have proved it to be correct. So you have to be very careful when using the p-value.

## Interpreting the measure

A very common way of interpreting the measure is _"The null hypothesis is rejected, with α% confidence"_, in the case of rejection (where the `p-value > α`) and the case of *no* rejection (`p-value < α`) _"Not enough evidence was found to reject the null hypothesis, with α% confidence"_.
