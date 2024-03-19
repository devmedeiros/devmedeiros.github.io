---
title: Sentiment Analysis of Fake News vs Real News
date: 2021-10-12 19:47:00 -0300
categories: [Blog]
tags: [data science, R, sentiment analysis, NLP, ggplot, storytelling]
summary: Sentiment Analysis comparing Fake News and Real News using R
aliases:
- 2021-10-12-fakenews-sentiment
cover:
    image: "https://ik.imagekit.io/devmedeiros/newspaper_9yA4QRDzj.webp?tr=w-700"
    alt: "a newspaper"
    caption: "Image of Krzysztof Pluta by Pixabay"
---

I want to tackle sentiment analysis using R in a simple way, just to get me started. With this in mind we begin loading all the packages we'll be using.

```r
library(readr)
library(dplyr)
library(tidytext)
library(tokenizers)
library(stopwords)
library(ggplot2)
```

Then we need to load our dataset. This data comes from Kaggle [_Fake and real news dataset_](https://www.kaggle.com/clmentbisaillon/fake-and-real-news-dataset).

```r
Fake <- read_csv('~/fakenews/Fake.csv')
True <- read.csv('~/fakenews/True.csv')
```

I want to merge the two datasets, but first we need to create a new column that will tell me where the data came from.

```r
Fake$news <- 'fake'
True$news <- 'real'

data <- rbind(Fake,True)
```

Now we can start the data cleaning. In this first moment, we'll do a simple tokenization on the **title** and **text** variables. Then we'll be removing the stopwords according to the _snowball_ source from the _stopwords_ package.

```r
title <- tibble(news = data$news, text = data$title)

corpus <- tibble(news = data$news, corpus = data$text)

tidy_title <- title %>%
  unnest_tokens(word, text, token = 'words') %>%
  filter(!(word %in% stopwords(source = 'snowball')))

tidy_corpus <- corpus %>%
  unnest_tokens(word, corpus, token = 'words') %>%
  filter(!(word %in% stopwords(source = "snowball")))
```

With the tidy data we can select the ten most frequent words from which title news' group.

```r
p0 <- tidy_title %>%
  group_by(news, word) %>%
  summarise(n = n()) %>%
  arrange(desc(n)) %>%
  slice(1:10)
```

Fake news titles mention _video_ and _trump_ by a large margin, 8477 and 7874 respectively. On the real news titles, _trump_ is also one of the most mentioned, coming on first with 4883 appearances, followed by _u.s_, 4187, and _says_ with 2981.

![10 most frequent words by fake news titles and real news](https://ik.imagekit.io/devmedeiros/10_popular_titles_H0dG3ljPN.png?updatedAt=1634083210303 "Top 10 words by fake news and real news")

Now we prepare the data to the sentiment analysis. I'm interested in classifing the data into sentiments of joy, anger, fear or surprise, for example. So I'll be using the `nrc` dataset from [Saif Mohammad and Peter Turney](http://saifmohammad.com/WebPages/NRC-Emotion-Lexicon.htm).

```r
p1 <- tidy_title %>%
  inner_join(get_sentiments('nrc')) %>%
  group_by(sentiment, news) %>%
  summarise(n = n()) %>%
  mutate(prop = n/sum(n))
```

![sentiment from news titles](https://ik.imagekit.io/devmedeiros/title_sentiment_lkDe-y_p97.png?updatedAt=1634083978487 "Sentiment from news titles")

**Disgust** seems to be the most common sentiment around fake news titles while **trust** is the lowest, even though it still is more than 50%. Overall fake news titles seems to have more "sentiment" than real news in this particular dataset. Even positive sentiments like **joy** and **surprise**.

```r
p2 <- tidy_corpus %>%
inner_join(get_sentiments('nrc')) %>%
group_by(sentiment, news) %>%
summarise(n = n()) %>%
mutate(prop = n/sum(n))
```

For the news' corpus we can see the same sentiments are prevalent, but the proportion is lower compared to the title. A fake news article loses **trust** when the reader takes more time to read it. It also becames less **negative** and shows less **fear**.

![sentiment from news text](https://ik.imagekit.io/devmedeiros/corpus_sentiment_302EKlTtO.png?updatedAt=1634083978312 "Sentiment from news corpus text")

An improvement we could do here is to use our own stopwords and change the way we made the tokens. We had instances were _trump_ and _trump's_ didn't correspond to the same thing and if we had used this data to train a model this could become problematic.
