---
title: "[BTS] Nota Fiscal Goiana"
date: 2024-03-24 11:31:00 -0300
categories: [Blog]
keywords: [python, streamlit, scraping, data science, data engineering, etl, nota fiscal goiana, power bi]
tags: [Python, Streamlit, Scraping, Data Science, Data Engineering, Power BI]
summary: A behind-the-scenes decision on the decision-making process of the project Nota Fiscal Goiana.
cover:
    image: "https://ik.imagekit.io/devmedeiros/bts-nota-fiscal-goiana_uNTM0DrqZ.webp"
    alt: "a view from above of a table with many laptops, there are logos for the nota fiscal goiana, streamlit and sqlite"
showtoc: true
---

## Introduction

The Nota Fiscal Goiana is an economic program implemented by the state of Goiás, Brazil. This program is designed to boost the collection of the Tax on Circulation of Goods and Services (ICMS) by encouraging people to ask for receipts when they buy goods and services within the state. Additionally, the program includes monthly raffles that distribute cash prizes to participating citizens.

As I regularly checked the Nota Fiscal Goiana portal and realized the website's simplicity, I decided to write a scraper to collect the raffle results. Initially, I aimed to simplify the process of checking if I had won, but as I delved deeper, I realized the potential to analyze the dataset more comprehensively.

## Early Design and Idea Phase

The original idea was to scrape raffle results, store them in a database, and give people the option to access the data directly from the database or just view it through a good-looking Power BI dashboard.

This led me to start writing the code for scraping the website and parsing the tables. However, since there were approximately 50 published results not available on the website, I had to resort to checking the Diário Oficial do Estado de Goiás, where the official government documents of Goiás are published in Brazil.

### Challenges Encountered

- Not all results were completely published on the Diário Oficial, leading to some results being unavailable.

- The project doesn't have a budget, so automation can't require VM and online databases. Which Power BI requires both.

### Solutions

The results that I could find published on the Diário Oficial were saved in the database along with results available on the website.

Initially, I was using free options of online databases, and the backup of the old data was on an SQLite available on the project repository. However, I realized that when running the scraper I could save the data on the SQLite and commit the changes, when I did the information persisted on the SQLite file and I could just read it.

To help automate the dashboard I resorted to using Streamlit instead of Power BI. Because Streamlit allows me to read the SQLite file directly from the GitHub repository and every time someone opens the Streamlit dashboard it reads the current data, so I don't need to set up refreshes.

## Current State of the Project

Currently, the project extracts monthly the raffle results from the Nota Fiscal Goiana and the state ICMS collection and saves the results in the SQLite file, using GitHub Actions. The Streamlit dashboard reads the SQLite file and displays the information on the Streamlit Community Cloud.

### Lessons Learned

While the Power BI dashboard was useful, I faced challenges with refreshing it easily. Initially using an SQLite database, I couldn't connect to the file on the repository, necessitating manual updates on my local machine. To overcome this, I explored alternatives like MySQL and PostgreSQL, enabling automated monthly scraping with GitHub Actions and updating the dashboard through the Power BI website.

The SQLite database is really helpful and depending on the size and scope of your project, you should look at it as a possible option to store your data.

### Going forward

I intend to maintain the project, making future patches to the scraping script if the website changes. And improving the code as my knowledge matures.

---

## Explore Further

- **GitHub Repository:** Access the project's source files and codes on our [GitHub Repository](https://github.com/devmedeiros/nota-fiscal-goiana/tree/main). Currently, everything is in Portuguese, but I plan to translate it in the future.

- **Streamlit Dashboard:** Explore the live Streamlit dashboard [here](https://nota-fiscal-goiana.streamlit.app/) to interact with the project's data visualization directly.

- **Academic Article:** If you're interested in a detailed academic analysis of this project, check out our article [here](http://periodicos.unifacef.com.br/resiget/article/view/2700/1876). Please note that the article is in Portuguese.