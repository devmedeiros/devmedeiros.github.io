---
title: EDA Top 50 Livros Bestsellers da Amazon
date: 2021-12-28 14:55:00 -0300
categories: [Blog]
tags: [python, visualização de dados, análise de dados, EDA]
summary: Análise Exploratória de Dados dos 50 livros mais bem vendidos da Amazon de 2009 - 2019
cover:
    image: "https://ik.imagekit.io/devmedeiros/books_n0VtB39Hz.webp?tr=w-700"
    alt: image of an multiple books, most closed and some open
    caption: Imagem de [Abhi Sharma](https://www.flickr.com/photos/abee5/8314929977), [CC BY 2.0](https://creativecommons.org/licenses/by/2.0/)
---

Recentemente eu terminei um curso na Alura chamado _Python para Data Science_ e eu quero colocar o que eu aprendi em prática, para isso eu vou fazer uma análise descritiva nesse banco de dados [**Amazon Top 50 Bestselling Books 2009 - 2019**](https://www.kaggle.com/sootersaalu/amazon-top-50-bestselling-books-2009-2019). Nele há 550 livros e eles foram categorizados como **fiction** (ficção) e **non-fiction** (não ficção) pelo Goodreads. Todo o código pode ser visto [aqui](https://gist.github.com/devmedeiros/12813bebd78f7662966096e963ed0aa9).

Eu comecei olhando as cinco primeiras observações do banco de dados.

| Name                                              | Author                   | User Rating | Reviews | Price | Year | Genre       |
|---------------------------------------------------|--------------------------|-------------|---------|-------|------|-------------|
| 10-Day Green Smoothie Cleanse                     | JJ Smith                 | 4.7         | 17350   | 8     | 2016 | Non Fiction |
| 11/22/63: A Novel                                 | Stephen King             | 4.6         | 2052    | 22    | 2011 | Fiction     |
| 12 Rules for Life: An Antidote to Chaos           | Jordan B. Peterson       | 4.7         | 18979   | 15    | 2018 | Non Fiction |
| 1984 (Signet Classics)                            | George Orwell            | 4.7         | 21424   | 6     | 2017 | Fiction     |
| 5,000 Awesome Facts (About Everything!) (Natio... | National Geographic Kids | 4.8         | 7665    | 12    | 2019 | Non Fiction |

Aqui é possível ver que os dados tem o **Year** (ano) em que o livro estava no _top 50 de mais vendidos_, seu **Price** (preço), a média dos **User Rating** (avaliação dos usuários), total de **Reviews** (avaliações), **Author** (autor), **Name** (nome do livro) e por fim, **Genre** (gênero).

Não há valores nulos no banco de dados. E dos 550 livros há 248 autores diferentes, então vamos ver quais autores possuem mais livros no top 50 dos mais vendidos neste período.

| Autor                             | Número de livros |
|------------------------------------|-----------------|
| Jeff Kinney                        | 12              |
| Gary Chapman                       | 11              |
| Rick Riordan                       | 11              |
| Suzanne Collins                    | 11              |
| American Psychological Association | 10              |
| Dr. Seuss                          | 9               |
| Gallup                             | 9               |
| Rob Elliott                        | 8               |
| Stephen R. Covey                   | 7               |
| Stephenie Meyer                    | 7               |
| Dav Pilkey	                       | 7               |
| Bill O'Reilly                      | 7               |
| Eric Carle	                       | 7               |

O autor com mais livros no top 50 foi Jeff Kinney, empatado em segundo, com 11 livros, foi Gary Chapman, Rick Riordan, e Suzanne Collins. Empatado em 9º, está Stephen R. Covey, Stephenie Meyer, Dav Pilkey, Bill O'Reilly, e Eric Carle, com 7 livros cada.

![Gráfico de viola da avaliação dos usuários](https://ik.imagekit.io/devmedeiros/violing_ur_vmTFo02uK.jpg?updatedAt=1640708039606)

Com o gráfico de violino podemos ver como está concentrado a avaliação dos usuários e como os dados são compostos de livros _bestsellers_ faz sentido que a avaliação dos usuários está em sua maioria concentrada em torno de 4.5 e 4.75.

![Boxplot da quantidade de reviews por ano](https://ik.imagekit.io/devmedeiros/boxplot_year_reviews_Pa1YGMhj2z1.jpg?updatedAt=1640708039777)

Esse boxplot da quantidade de avaliações por ano mostra que a variabilidade aumentou através dos anos, tendo o seu pico em 2014 e gradualmente estabilizando. Podemos ver também que nos primeiros anos, 2010 e 2011, havia mais _outliers_ nos dados.

| Gênero       | Avaliação do Usuário | Preço |
|-------------|-------------|-------|
| Ficção     | 4.65        | 10.85 |
| Não Ficção | 4.60        | 14.84 |

A avaliação média do usuário por gênero parece ser semelhante, com apenas 0.05 de diferença, mas o preço já apresenta uma diferença maior, 10.85 para ficção e 14.84 para não ficção. Para termos certezas de que essas diferenças são estatisticamente significantes, eu vou utilizar o teste de Mann-Whitney.

A hipótese nula do teste de Mann-Whitney é de que as amostras possuem a mesma distribuição, e em ambos os casos, nós rejeitamos a hipótese nula com 95% de confiança. O p-valor para os dados do preço foi de 8.34e-08 e o p-valor para a avaliação do usuário foi de 1.495e-07.

Para mostrar visualmente quão diferente as suas distribuição são, podemos olhar para os seguintes gráficos.

![Distribuição do preço dos livros por gênero](https://ik.imagekit.io/devmedeiros/hist_price_qxT6fxEGQ.jpg?updatedAt=1640708039771)

A distribuição para os preços de livros de ficção é fortemente inclinados para a esquerda e consistentemente diminuem a medida que o preço aumenta. Enquanto que os livros de ficção começam altos e se tornam ainda mais altos, com 120 e quase 140 ocorrências nas duas primeiras categorias, então ele rapidamente diminui.

![Distribuição da avaliação dos usuários por gênero](https://ik.imagekit.io/devmedeiros/hist_ur_6YxOQ_Huz.jpg?updatedAt=1640708040024)

A distribuição para a avaliação do usuário do gênero de ficção lentamente aumenta, tendo seu pico próximo de 4.8. E a distribuição para o gênero de não ficção tem seu pico logo após 4.6.
