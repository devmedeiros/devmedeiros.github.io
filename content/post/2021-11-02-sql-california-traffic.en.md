---
title: Have COVID Impacted California Traffic Collisions?
date: 2021-11-02 17:14:00 -0300
categories: [Blog]
tags: [SQL, R, data analysis]
---

**Tools used:** SQL, R

**Category:** Data Analysis

---
<!--more-->

## Introduction

COVID-19 has changed many things in our day-to-day life, from ordering more delivery, working from home, or even just getting a new pet. And now I want to see if it has impacted traffic crashes in California. The dataset that I’ll be using comes from Kaggle. It covers collisions from January 2001 up to December 2020 from California. [^1] The following are the tables present in the dataset:

- case_id
- collisions
- parties
- victims

The **case_id** contains the _case_id_ and _db_year_. The **collisions** table contains all the information about each collision. While the **parties** table contains information about all the parties involved in the collisions, in this case, parties can be drivers, pedestrians, cyclists, and parked vehicles. The **victims** table contain information about all the victims, it also includes passengers.

To answer if the pandemic has impacted the collisions I need to separate my data. Knowing that the first case of COVID was on January 26, 2020, in California. [^2] I’ll be separating the dataset as before COVID for every crash that happened before the first case and after COVID for every crash on the same day and forward.

To compare _before COVID_ and _after COVID_ I’ll looking at a couple of things, **Proportion of DUIs** and **Fatality of Crashes**.

To do this I’ll be querying the database with SQLite through R. For the complete code go to my GitHub [repo](https://github.com/devmedeiros/california-traffic-collisions).

## Proportion of DUIs

Alcohol use has changed in the US during the pandemic [^3] and with all the lockdowns and working from home we may wonder if this alcohol use has caused more crashes or not. With this in mind we take a look at our data about California.

As we can see from the following table, the percentages before and after COVID are very similar. Only **7.3%** and **8.96%** of violations occurred from DUIs.

|         |Before COVID |    	 | 	After COVID|     |
|---------|------------:|-----:|------------:|----:|	 
|Violation| Qnty        |Perc  |	Qnty	     |Perc |
|DUI      | 653467      |	7.3	 |42169	       |8.96 |
|Other    |	 8300399    |	92.7 |428299       |91.04|

## Fatality of Crashes

Moving on to our next task, to see if crashes after COVID became more or less fatal. The analysis is going to look at how many people died from collisions, how many got some injury, and no injury at all.

Looking at the table below you can see that the percentage of collisions that resulted in someone dying was **0.74%** before the pandemic and is now **1.29%**, is not a big difference, but when you look at _no injury_, you can see that before COVID **45.24%** of collisions didn't result in someone having an injury and after the pandemic, this number drops to **18.57%**. This _could_ indicate some influence of the pandemic, but further research is needed.

|                 |Before COVID |    	  |  After COVID|       |
|-----------------|------------:|------:|------------:|------:|
|Degree of Injury |	Qnty	      | Perc	| Qnty	      | Perc  |
|Death            |	68875       |	0.74	| 4131	      | 1.29  |
|Some injury      |	5033610	    | 54.02	| 257057	    | 80.14 |
|No injury        |	4216085	    | 45.24 |	59576	      | 18.57 |

### References

[^1]: [Alexander Gude and California Highway Patrol, “California Traffic Collision Data from SWITRS.” Kaggle, 2021, doi: 10.34740/KAGGLE/DSV/2569326.](https://www.kaggle.com/alexgude/california-traffic-collision-data-from-switrs)

[^2]: [Timeline of Coronavirus US](https://abc7news.com/timeline-of-coronavirus-us-covid-19-bay-area-sf/6047519/)

[^3]: [Pollard MS, Tucker JS, Green HD. Changes in Adult Alcohol Use and Consequences During the COVID-19 Pandemic in the US. JAMA Netw Open. 2020;3(9):e2022942. doi:10.1001/jamanetworkopen.2020.22942](https://jamanetwork.com/journals/jamanetworkopen/fullarticle/2770975)
