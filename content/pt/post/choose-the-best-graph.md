---
title: Fazendo Gráficos Melhores
date: 2022-11-15 08:01:00 -0300
categories: [Blog]
tags: [data visualization, business intelligence, storytelling, review, ethic]
summary: Escolher o melhor gráfico pode ser uma tarefa difícil, mas não precisa ser.
cover:
    image: https://i.imgur.com/eggG31k.jpg
    alt: "um gráfico de rosca mostrando sim como a maioria enquanto ele representa 10%, e não como a minoria quando ele representa 90%"
    caption: "piechart de WTF Visualizations"
    hidden: true
---

Aprendendo a fazer melhores visualizações vem com estudo e prática, hoje quero falar sobre fontes que você pode usar para orientá-lo a fazer gráficos melhores e outras visualizações de dados em geral.

## From Data to Viz

![infográfico mostrando diferentes possibilidades de escolha de um terreno](https://i.imgur.com/416qeWj.png)

[From Data to Viz](https://www.data-to-viz.com/) traz muitas ferramentas para ajudar a navegar no mundo da visualização de dados. Na guia `Explore` você se depara com um infográfico interativo que ajuda a escolher um enredo, você pode escolher entre: `numérico`, `categórico`, `numérico&categórico`, `mapas`, `rede` e `série temporal`. Mas ele não mostra apenas gráficos que seriam bons para o seu tipo de dados, mas também uma descrição explicando o gráfico e os erros comuns que as pessoas cometem ao usar esse gráfico específico.

No final de cada ramo do infográfico, há também um link `story`, onde você pode ler sobre uma implementação desse tipo de plot usando dados reais.

Caso você esteja apenas começando e não conhece muitos termos ou não tem certeza se seus dados contêm `muitos` ou `poucos` pontos quando você passa o mouse sobre os nós de opções, você poderá ver uma descrição com uma tabela de exemplo representando o que isso significa.

Ao ir para a guia `Caveats` você pode ver uma coleção de advertências e pode filtrá-las por:

- **Improvement:** todas as ressalvas que irão sugerir algum tipo de melhoria nas parcelas.

 - **Misleading:** alguns gráficos podem ser enganosos ao omitir algumas informações, seja o tamanho da amostra ou a diferença proporcional entre duas categorias. Ao ler isso, você poderá identificar práticas ruins que levam à confusão ou manipulação.

Este site também fornece links para galerias **Python** e **R**, onde você pode obter snippets de código para reproduzir esses gráficos.

Se você preferir aprender observando o que não fazer, confira [WTF Visualizations](https://viz.wtf/). Ele mostra visualizações ruins que não fazem sentido ou são de alguma forma enganosas.

Lá você encontrará vários gráficos de pizza que não somam 100%, gráficos de área onde os números não correspondem à área e apenas manipulação direta como no exemplo abaixo.

{{< figure src="https://64.media.tumblr.com/d8bcf748d564125312aeba4001b96df5/tumblr_qzg4n3nfJY1sgh0voo1_640.jpg" caption="gráfico de pizza de [WTF Visualizations](https://viz.wtf/)" align="center" alt="um gráfico de rosca mostrando 'sim' como maioria enquanto representa 10% e 'não' como minoria enquanto representa 90%">}}