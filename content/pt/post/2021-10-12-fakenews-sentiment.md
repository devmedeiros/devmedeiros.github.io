---
title: Análise de Sentimentos de Notícias Falsas vs Notícias Reais
date: 2021-10-12 19:47:00 -0300
categories: [Blog]
tags: [ciência de dados, R, análise de sentimentos, NLP, ggplot]
summary: Análise de Sentimento comparando notícias falsas e notícias reais
cover:
    image: "https://ik.imagekit.io/devmedeiros/newspaper_9yA4QRDzj.webp?tr=w-700"
    alt: "um jornal"
    caption: "Imagem de Krzysztof Pluta por Pixabay"
---

Eu quero fazer uma análise de sentimentos usando o R como uma forma de aprender. Com isso em mente começamos carregando todos os pacotes que iremos utilizar.

```r
library(readr)
library(dplyr)
library(tidytext)
library(tokenizers)
library(stopwords)
library(ggplot2)
```

Então precisamos carregar nosso banco de dados. Esses dados são do  Kaggle [_Fake and real news dataset_](https://www.kaggle.com/clmentbisaillon/fake-and-real-news-dataset).

```r
Fake <- read_csv('~/fakenews/Fake.csv')
True <- read.csv('~/fakenews/True.csv')
```

Eu quero unir ambos os dados, mas antes é preciso criar uma nova columa que irá informar de onde os dados vieram.

```r
Fake$news <- 'fake'
True$news <- 'real'

data <- rbind(Fake,True)
```

Agora podemos iniciar a limpeza dos dados. Neste primeiro momento, fazemos os tokens nas variáveis **title** e **text**. Em seguida iremos remover as _stopwords_ (palavras redundantes) de acordo com a fonte _snowball_ do pacote _stopwords_.

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

Com os dados limpos podemos selecionar as dez palavras mais frequentes dos títulos das noticias, separado por grupo real ou falso.

```r
p0 <- tidy_title %>%
  group_by(news, word) %>%
  summarise(n = n()) %>%
  arrange(desc(n)) %>%
  slice(1:10)
```

Títulos de notícias falsas mencionam muito mais _video_ e _trump_, 8477 e 7874, respectivamente. Já no caso de títulos de notícias reais, _trump_ também é uma das palavras mais mencionadas, aparecendo em primeiro com 4883 aparições, seguido por _u.s_ com 4187 e _says_ com 2981.

![10 most frequent words by fake news titles and real news](https://ik.imagekit.io/devmedeiros/10_popular_titles_H0dG3ljPN.png?updatedAt=1634083210303 "Top 10 words by fake news and real news")

Agora iremos preparar os dados para a análise de sentimento. Eu estou interessada em classificar os dados em sentimentos de alegria, raiva, medo ou surpresa, por exemplo. Então eu iriei utilizar o os dados de `nrc` [Saif Mohammad and Peter Turney](http://saifmohammad.com/WebPages/NRC-Emotion-Lexicon.htm).

```r
p1 <- tidy_title %>%
  inner_join(get_sentiments('nrc')) %>%
  group_by(sentiment, news) %>%
  summarise(n = n()) %>%
  mutate(prop = n/sum(n))
```

![sentiment from news titles](https://ik.imagekit.io/devmedeiros/title_sentiment_lkDe-y_p97.png?updatedAt=1634083978487 "Sentiment from news titles")

**Disgust** (nojo) aparenta ser o sentimento mais comum envolvendo títulos de notícias falsas, enquanto que **trust** (confiança) é o menor, mesmo que ainda seja maior que 50%. Em geral, títulos de notícias falsas aparentam ter mais "sentimento" do que títulos de notícias reais, neste banco de dados. Isso vale até para sentimentos positivos como **joy** (alegria) e **surprise** (surpresa).

```r
p2 <- tidy_corpus %>%
inner_join(get_sentiments('nrc')) %>%
group_by(sentiment, news) %>%
summarise(n = n()) %>%
mutate(prop = n/sum(n))
```

Para o corpo das notícias reais pode-se notas que os mesmos sentimentos são prevalentes, mas a proporção é menor comparado ao título. Um artigo de notícias falsas perde confiança (**trust**) quando o leitor lê o corpo do artigo. Ele também se torna menos negativo (**negative**) e apresenta menos medo (**fear**)

![sentiment from news text](https://ik.imagekit.io/devmedeiros/corpus_sentiment_302EKlTtO.png?updatedAt=1634083978312 "Sentiment from news corpus text")

Uma melhoria que poderia ser feito aqui é tentar construir nosso próprio dicionário de palavras redundantes (_stopwords_) e alterar a forma que a toneização foi feita. Pois houveram momentos em que _trump_ e _trump's_ não corresponderam a mesma coisa e se estes dados tivessem sido usados para treinar um modelo isso poderia se tornar um problema.
