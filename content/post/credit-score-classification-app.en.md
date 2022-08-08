---
title: Credit Score Classification App
date: 2022-08-08 17:17:00 -0300
categories: [Projects]
tags: [python, streamlit, pickle, machine learning, credit score, app, data visualization, data science, random forest]
toc: true
summary: Using Streamlit to make a web app that classifies your credit score using Python
---

# Project Overview

This project showcase a data science life cycle, where I clean and prepare the dataset, use feature engineering, machine learning, deploy, and data visualization.

![data science cycle of this project in a diagram](https://ik.imagekit.io/devmedeiros/data-science-cycle_QZwyHaXsP.png?ik-sdk-version=javascript-1.4.3&updatedAt=1659975338736#center)

The dataset comes from [kaggle](https://www.kaggle.com/datasets/parisrohan/credit-score-classification?select=train.csv), it has a lot of information about a person's credit and bank details, but it also has a lot of typos, missing data, and censored data. This dataset needed cleaning and also needed some feature engineering, I needed to mutate some features, so they could be read by the model. Thus when presented with categorical data I needed to identify if it was ordinal or nominal, if it was an ordinal variable then it would be mapped to sequential numbers otherwise I'd make a dummy. For _yes_ and _no_ variables I choose to make just one dummy, but for types of loans I made one dummy for each loan type and if someone didn't have a loan they simply get 0 on all loan type features. I talk about the process of cleaning and feature engineering on this dataset [here](/post/data-cleaning-credit-score/).

Then I needed a machine learning model that I could predict a person's credit score based on some features. To decide which features I was going to use I based my decision on what is commonly used among real companies, and I also choose variables that I thought made sense. I ended up with the following features:

- Age
- Annual income
- Number of bank accounts
- Number of credit cards
- Number of delayed payments
- Credit card utilization ratio
- Total EMI paid monthly
- Credit history age in months
- Loans
- Missed any payment in the last 12 months
- Paid minimum amount on at least one credit card

With the features ready, I moved on to making the model, I decided to use a simple Random Forest, for now, I do intend to work on making this model better, but in this first instance, I wanted to focus on making the streamlit app.

After I finished the model I serialized it and the scaler using the `pickle` package. To deploy the model and build a visualization I used [streamlit](https://streamlit.io/).

![a gif showing how the streamlit credit score app works](https://user-images.githubusercontent.com/33239902/183321842-be97fb04-f00b-4b62-8e6e-2b53d25335a0.gif)

In this app, you can fill out a form or just select one of the three default profiles given to see how the model evaluates each person's credit score. It also presents how certain the model was by displaying a pie graph with the probability (in percentage) of each credit score group the answers fit. It also shows how much each feature counts towards your credit score, according to this model. You can see the app live [here](https://devmedeiros-credit-score-classification-appstreamlit-app-fcakrl.streamlitapp.com/).

---

All of the code is available at my GitHub [repository](https://github.com/devmedeiros/credit-score-classification-app). Besides the code, there you'll find the documentation, the original and treated data (all the stages of treatment), all the requirements for building this project, and how to run it locally.