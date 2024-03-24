---
title: Simulação de 21
date: 2021-10-24 20:43:00 -0300
categories: [Blog]
keywords: [R, simulação, shiny, ggplot]
tags: [R, Shiny]
summary: Uma simulação de 21 usando R e Shiny, você pode rodar um jogo com p jogadores, d baralhos, n vezes
aliases:
- 2021-10-24-blackjack-simulation
cover:
    image: "https://ik.imagekit.io/devmedeiros/blackjack__J-I9kYyJ.webp?tr=w-700"
    alt: "um ás e um valete sobre uma mesa verde com peças de poquêr"
    caption: "Imagem de Michael Brasuela por Pixabay"
---

O meu objetivo com este projeto é de simular o ambiente de um jogo de 21, também conhecido como _blackjack_. Assim, eu decidi fazer diversas funções para emular o comportamento do _dealer_, de um jogador iniciante, um jogador cauteloso e um estrategista. Com esse conjunto de funções você pode rodar um jogo com **p** jogadores, **d** baralhos e quaisquer combinações de arquétipos de jogadores. Além disso, também pode rodar o jogo **n** vezes.

Eu também fiz uma [aplicação shiny](https://jaqueline-medeiros.shinyapps.io/appa/) para demonstrar como a simulação funciona. No aplicativo, você é limitado no número de jogadores, mas caso queira rodar o código com mais jogadores eu sugiro que olhe o [respositório](https://github.com/devmedeiros/blackjack-simulation) do GitHub. Nele você encontra as regras consideradas para a simulação e o código completo.

Caso seja familiar com a linguagem de programação R, você  também pode rodar o aplicativo localmente, basta carregar a biblioteca do shiny `library(shiny)` e rodar o código `runGitHub("blackjack-simulation", "devmedeiros", ref = "main")`.

O app é composto de uma barra lateral com um espaço para escolher os arquétipos, o número de baralhos a ser usados e quantas rodadas você quer simular. Dependendo de quantas rodadas você escolher a simulação pode ficar mais lenta, pois a simulação roda de acordo com as suas escolhas todas vez que clica no botão **RUN SIMULATION RUN**.

Na aba _plot_, nota-se a evolução da taxa de perda através das rodadas.

**1** quer dizer que o jogador perdeu aquela rodada e um **0** quer dizer que ele ganhou.

![Aplicação do shiny, aba plot](https://ik.imagekit.io/devmedeiros/plot_cm7Dhm0u6a.png?updatedAt=1635119435941)

A aba _game setup_ mostra todas as cartas distribuídas na simulação, cada carta foi entregue da esquerda para a direita e uma célula em branco quer dizer que o jogador não pediu pra receber outra carta (_hit_, bate).

![Aplicação do shiny, aba game setup](https://ik.imagekit.io/devmedeiros/game-setup_-FspHIe5w.png?updatedAt=1635119436114)

Por fim, a aba _lose rate_ mostra os mesmo dados da aba _plot_, mas em formato de tabela. Isso é útil quando se quer analisar como uma estratégia foi melhor do que outra.

![Aplicação do shiny, aba lose rate](https://ik.imagekit.io/devmedeiros/lose_rate_jPTu-cXHuN.png?updatedAt=1635119436123)
