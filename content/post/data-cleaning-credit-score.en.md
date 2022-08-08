---
title: Cleaning Credit Score Classification Dataset
date: 2022-07-24 13:06:00 -0300
categories: [Blog]
tags: [data cleaning, python, pandas, tutorial, kaggle, numpy, credit score, classification]
showtoc: true
summary: How to come up with ways to clean a dataset using Python
---

![a man wearing gloves cleaning a table with a cloth](https://ik.imagekit.io/devmedeiros/cleaning-table_NhcXa2k4f.jpg)
[_Profession photo created by freepik - freepik.com_](https://www.freepik.com/photos/profession)

> **Disclaimer:** I'll be talking about how to come up with the python code, if you want to read the actual code please go to this [repo](https://github.com/devmedeiros/credit-score-classification-app/tree/main/notebooks).

# Meet the Credit Score Classification Dataset

The dataset that we'll clean comes from [kaggle](https://www.kaggle.com/datasets/parisrohan/credit-score-classification?select=train.csv), which is the `train.csv` dataset, but this could be used for the `test.csv` as well.

There are 28 columns and 100k rows in this dataset. I compiled a feature description table that you can see below.

| Feature | Description |
|---|----|
|`ID` | Represents a unique identification of an entry |
|`Customer_ID` | Represents a unique identification of a person |
|`Month` | Represents the month of the year |
|`Name` | Represents the name of a person |
|`Age` | Represents the age of the person |
|`SSN` | Represents the social security number of a person |
|`Occupation` | Represents the occupation of the person |
|`Annual_Income` | Represents the annual income of the person |
|`Monthly_Inhand_Salary` | Represents the monthly base salary of a person |
|`Num_Bank_Accounts` | Represents the number of bank accounts a person holds |
|`Num_Credit_Card` | Represents the number of other credit cards held by a person |
|`Interest_Rate` | Represents the interest rate on a credit card |
|`Num_of_Loan` | Represents the number of loans taken from the bank |
|`Type_of_Loan` | Represents the types of loan taken by a person |
|`Delay_from_due_date` | Represents the average number of days delayed from the payment date |
|`Num_of_Delayed_Payment` | Represents the average number of payments delayed by a person |
|`Changed_Credit_Limit` | Represents the percentage change in credit card limit |
|`Num_Credit_Inquiries` | Represents the number of credit card inquiries |
|`Credit_Mix` | Represents the classification of the mix of credits |
|`Outstanding_Debt` | Represents the remaining debt to be paid (in USD) |
|`Credit_Utilization_Ratio` | Represents the utilization ratio of credit cards |
|`Credit_History_Age` | Represents the age of credit history of the person |
|`Payment_of_Min_Amount` | Represents whether only the minimum amount was paid by the person |
|`Total_EMI_per_month` | Represents the monthly EMI payments (in USD) |
|`Amount_invested_monthly` | Represents the monthly amount invested by the customer (in USD) |
|`Payment_Behaviour` | Represents the payment behavior of the customer (in USD) |
|`Monthly_Balance` | Represents the monthly balance amount of the customer (in USD)
|`Credit_Score` | Represents the bracket of credit score (Poor, Standard, Good)

Even though we have 100k rows, within these rows that are only 12,500 different customers, each customer appears 8 times (from January to August). So basically we can select a particular customer and look at their information and easily find incorrect data and be able to adjust it.

# Cleaning Typos and Outliers

In this dataset that is a lot of typos or just straight-up nonsense. You'll find some values to be: `_`, `!@9#%8`, `__10000__`, `NM` or `_______`. I believe these typos are in the dataset to represent the improbability that you may find when dealing with real-world data and most of them mean that this is a null value.

For a moment I thought `__10000__` would just be a typo, but there is no amount invested monthly that is over 200 dollars.

```txt
__10000__             4305
0.0                    169
80.41529543900253        1
36.66235139442514        1
89.7384893604547         1
                      ... 
36.541908593249026       1
93.45116318631192        1
140.80972223052834       1
38.73937670100975        1
167.1638651610451        1
Name: Amount_invested_monthly, Length: 91049, dtype: int64
```

Following this logic, I looked for nonsense in the data frame and I started to replace them with numpy `nan`'s. I also looked for outliers by looking at the distribution of values, if there was a value that only appeared once and was isolated I substitute it for a null value. I based this decision not only on this but also when I looked for customers that had this outlier and I observed all the data from this particular customer, I'd see weird things like:

![a table displaying information on a given customer and showing an outlier on its annual income](https://raw.githubusercontent.com/devmedeiros/credit-score-classification-app/main/reports/figures/annual_income.png#center)

By looking at this customer is clear that he didn't make this much money annually only one month of the year.

When you finish this search for typos and outliers don't forget to assign the correct data type to your features. Some features like `age` started with string characters among the age values and because of this, it's uploaded as an object instead of int or float.

# Filling Null Values

After dealing with all the outliers and typos, we ended up with a lot of null values, as you can see:

```python
df.isna().sum()[df.isna().sum() > 0]
```
```txt
Age                         2776
Occupation                  7062
Annual_Income                993
Monthly_Inhand_Salary      15002
Num_Credit_Card             2271
Interest_Rate               2034
Num_of_Loan                 4348
Type_of_Loan               11408
Num_of_Delayed_Payment      7002
Changed_Credit_Limit        2091
Num_Credit_Inquiries        1965
Credit_Mix                 20195
Credit_History_Age          9030
Payment_of_Min_Amount      12007
Amount_invested_monthly     8784
Payment_Behaviour           7600
Monthly_Balance             1200
dtype: int64
```

Instead of just dropping all these null values I first try to fill them using the information I already have. Remember that I said that a customer has historical data for 8 months? We can just use this historical data to fill the null values using an aggregation measurement of our choice filtering for the customer, this will be more accurate than just calculating the mean value of the database.

I decided to use the average values for the following columns:

```python
mean_columns = [
    'Num_of_Delayed_Payment', 'Changed_Credit_Limit', 'Num_Credit_Inquiries',
    'Amount_invested_monthly', 'Monthly_Balance', 'Num_of_Loan',
    'Num_Credit_Card', 'Interest_Rate', 'Annual_Income', 'Monthly_Inhand_Salary'
    ]
```

And the last non-empty value for these:

```python
last_columns = ['Age', 'Occupation', 'Type_of_Loan', 'Credit_Mix']
```

The reason for not using the mean for all my values is that I didn't want to have someone be 20.5 years old and `Occupation`, `Type_of_Loan`, and `Credit_Mix` are discrete data.

# Feature Engineering

With the clean data, we can proceed to feature engineering. In this case, we first want to change the `Type_of_Loan`, because that are some occurrences that it has all the loans in one value, as you can see:

```txt
Not Specified, Mortgage Loan, Auto Loan, and Payday Loan                                                                                         8
Payday Loan, Mortgage Loan, Debt Consolidation Loan, and Student Loan                                                                            8
Debt Consolidation Loan, Auto Loan, Personal Loan, Debt Consolidation Loan, Student Loan, and Credit-Builder Loan                                8
Student Loan, Auto Loan, Student Loan, Credit-Builder Loan, Home Equity Loan, Debt Consolidation Loan, and Debt Consolidation Loan               8
Personal Loan, Auto Loan, Mortgage Loan, Student Loan, and Student Loan                                                                          8
Name: Type_of_Loan, Length: 5380, dtype: int64
```

So I'll save all the different loan types in one vector, by splitting the loans every time there is a `,` or `, and`.

```python
loan_types = []
for index in df.index:
    temp = df.Type_of_Loan[index].replace('and ', '').split(', ')
    for i in temp: #loan in temp array
        if i not in loan_types: #if loan is not in loan_types
            loan_types.append(i) #add it
```

Now we can create dummy variables of these `loan_types`, so a customer will receive the number 1 if they have this loan or a 0 if they don't.

```python
for loan in loan_types:
    df[loan] = 0 #create the loan column in the df with 0
    for index in df.index:
        temp = df.Type_of_Loan[index].replace('and ', '').split(', ')
        if loan in temp:
            df.loc[index, loan] = 1
```

Now I want to keep working on this dataset to make it ready for training a machine learning model. For this reason, I need to transform my discrete data into numeric.

The feature `Credit_History_Age` has the values as strings "22 Years and 5 Months", this pattern repeats itself, so we can take advantage of this and select the year multiplied by 12 and sum the month, resulting in a new feature with the credit history age in months. When we are done with this, there are still going to be null values, to fill them I choose to interpolate the values, this works great when the missing value is in February up until July because it interpolates with the customer's credit history age, but it becomes a bad guessed when the missing value is in January or August. 

The months' names are going to be replaced by their number counterpart, so January is 1, February is 2, and so on. `credit_mix` and `credit_score` have 3 sequential categories, I choose to go with -1, 0, and 1, but you can use 1, 2, 3 and it'll produce the same result.

Don't forget to check the GitHub [Repository](https://github.com/devmedeiros/credit-score-classification-app/tree/main/) for the complete code mentioned here and the cleaned dataset.