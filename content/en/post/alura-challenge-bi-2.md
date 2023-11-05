---
title: Alura Challenge BI 2
date: 2022-06-30 19:23:00 -0300
lastmod: 2023-11-05 19:35:00 -0300
categories: [Blog]
tags: [data visualization, dashboard, powerbi, SQL, business intelligence]
summary: Alura hosted a four-week Challenge BI, where participants needed to make 3 dashboards
showtoc: true
cover:
    image: "https://user-images.githubusercontent.com/33239902/176906617-80e0a1c3-3b3a-4f26-9ee1-2747ba1e00e1.gif#vitrinedev"
    alt: "a gif showing how alura food report works"
    hidden: true
---

The Alura Challenge BI 2 was a four-week event where we're challenged to make _Business Intelligence_ dashboards according to the request of three made-up companies. During the event I made this three dashboard using Power BI, you can look at the source file of all dashboards at my [repo](https://github.com/devmedeiros/Alura-Challenge-BI-2).

## Alura Films

The objective of this dashboard is to help find the best selection for an upcoming movie.

{{< powerbi "Relatório Alura Films" "https://app.powerbi.com/view?r=eyJrIjoiZTllNjE2ZTQtNjdkMy00OGI4LTllNTAtM2RhNGE2YWU4YmZlIiwidCI6IjI2ZjA4NzIyLTFjOWUtNGVkZS1iN2VkLThhMmI3N2ZmM2Q5YyJ9" >}}

With this in mind, I explored in the first page a little summary about the movies in the database, with this you can get a general view of our data. The second page is displayed the IMDB and Meta Score of the movies. I also created a measure to show how much both measurements agree. A score of 100 means **doesn't agree at all** and 0 means **completely agree**, the agreeable score was 9.48.

Our third page shows information about the movie stars, the top 10 actors with the highest movie gross revenue, and the top 10 actors with the biggest movie count.

The fourth page presents the distribution of gross revenue by how many different genres a movie has. It also shows how many movies have a given genre, for example, **Drama** is the most popular choice for movie genre, with 72% of movies in the dataset. At last, this page also shows the mean gross revenue by genre.

The fifth and last page shows a bit of information about the Brazillian Parental Guide and shows the average Meta Score for movies by the parental guide certification. It also shows how much gross revenue, on average, each parental guide certification earned.

## Alura Food

**Alura Food** is interested in expanding its business by entering the Indian market. To do so, the company asked to calculate measures to help them make a better decision.

{{< powerbi "Relatório Alura Food" "https://app.powerbi.com/view?r=eyJrIjoiNGEzMTQzMGYtNzFhNC00YjcyLThiMDMtMGE2YTMzMDRhODFiIiwidCI6IjI2ZjA4NzIyLTFjOWUtNGVkZS1iN2VkLThhMmI3N2ZmM2Q5YyJ9" >}}

First of all, I merged the datasets and cleaned the data through Power BI, translated a few texts from English to Portuguese through Google Sheets, and converted the meals price from their respective currency value to BRL (Brazillian Currency). Finally, I used Figma to make the background, including images and titles.

In this project, I opted to used a single page to display all the requested information as I believe it makes it easier to analyze the data.

Most restaurants in this Indian market don't offer online delivery, with just 19.21% having it. The average rating for the restaurants is 3.72 out of a maximum of 5, the rating is also presented as text, with a _Muito Bom_(Very Good) being a score over 4.

The average price of a meal per person is around R$ 39.48 (around USD 7.65). And there are 9577 different restaurants in the dataset, of which 3968, specialize in North Indian cuisine. New Delhi is, by far, the most popular choice to open a restaurant with 5473, the second closest city is Gurgaon, with 1118 restaurants.

## Alura Skimó

**Alura Skimo** is interested in analyzing their sales data, to help with this I made this dashboard.

{{< powerbi "Relatório Alura Skimo" "https://app.powerbi.com/view?r=eyJrIjoiNTllYjJmODItYzQxNC00ZmEzLTk5ZGMtZDgzNzY2NDM2ZGMxIiwidCI6IjI2ZjA4NzIyLTFjOWUtNGVkZS1iN2VkLThhMmI3N2ZmM2Q5YyJ9" >}}

It’s composed of three pages. The first one presents a summary of all the main measures you’ll find in the dashboard, filtered for the most recent month. On this first page, you can also rest your mouse cursor over a measure and it’ll show a small plot with the historical series with a tendency line.

Moving on to the next page, you can see all the information about the products sold by **Alura Skimo**. You can filter the data by ice cream flavor, type of package, category, and product cost. It displayed the basic information about the products, along with a rank of the best-selling products and a couple of plots showing the sales by flavor and sales by category.

On the last page, you can find information about the salesperson from our company. It shows when they joined the company, how much is their cut, how much revenue they got and how many sales each one made, everything in the last year (2018).

This dashboard is complex next to the other two because our dataset came from SQL files. First, I had to create the database and load each SQL file into it, then all I had to do was load it on Power BI. At last, I did all the data cleaning and processing on it.