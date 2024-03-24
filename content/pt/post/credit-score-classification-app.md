---
title: Aplicativo de Classificação de Score de Crédito
date: 2022-08-08 17:17:00 -0300
categories: [Projects]
keywords: [python, streamlit, pickle, aprendizado de máquina, score, aplicativo, visualização de dados, ciência de dados, random forest]
tags: [Python, Streamlit, Aprendizado de Máquina, Visualização de Dados, Ciência de Dados]
showtoc: true
summary: Usando o Streamlit para fazer um aplicativo da web que classifica sua pontuação de crédito usando Python
cover:
    image: "https://user-images.githubusercontent.com/33239902/183321842-be97fb04-f00b-4b62-8e6e-2b53d25335a0.gif"
    alt: "um gif mostrando como o app streamlit de classificação de crédito"
    hidden: true
---

## Visão Geral do Projeto

Este projeto mostra um ciclo de vida de ciência de dados, onde eu limpo e preparo o banco de dados, _feature engineering_, aprendizado de máquina, _deploy_ e visualização de dados.

![ciclo de ciência de dados deste projeto em um diagrama](https://ik.imagekit.io/devmedeiros/data-science-cycle_QZwyHaXsP.png?ik-sdk-version=javascript-1.4.3&updatedAt=1659975338736#center)

O conjunto de dados vem do [kaggle](https://www.kaggle.com/datasets/parisrohan/credit-score-classification?select=train.csv), e possui muitas informações sobre o crédito e os dados bancários de uma pessoa, mas ele também tem muitos erros de digitação, dados ausentes e dados censurados. Este conjunto de dados precisava ser limpo e também precisava de _feature engineering_, eu precisava alterar algumas _features_, para que pudessem ser lidos pelo modelo. Assim, quando apresentada com dados categóricos, eu precisava identificar se era ordinal ou nominal, se fosse uma variável ordinal, seria mapeada para números sequenciais, caso contrário, eu faria uma _dummy_. Para as variáveis ​​_sim_ e _não_ eu escolhi fazer apenas uma _dummy_, mas para os tipos de empréstimos fiz uma _dummy_ para cada tipo de empréstimo e se alguém não tiver um empréstimo, simplesmente obtém 0 em todos as _features_ de tipo de empréstimo. Eu falo sobre o processo de limpeza e _feature engeeniring_ neste conjunto de dados [aqui](/pt/post/data-cleaning-credit-score/).

Então eu precisava de um modelo de ML que pudesse prever o score de crédito de uma pessoa com base em algumas _features_. Para decidir quais _features_ eu iria usar eu analisei quais variáveis são usadas em empresas reais, e eu também escolhi variáveis que eu achei que faziam sentido. Eu acabei escolhendo as seguintes variáveis:

- Idade
- Renda anual
- Quantidade de contas bancárias
- Quantidade de cartões de crédito
- Quantidade de pagamentos atrasados
- Proporção de uso do cartão de crédito
- Quantidade de pagamento de empréstimos
- Idade do histórico de crédito em meses
- Empréstimos
- Perdeu algum pagamento nos últimos 12 meses
- Pagou o valor mínimo em pelo menos um cartão de crédito

Com as _features_ prontas, eu comecei a trabalhar no modelo, por enquanto eu decidi usar um modelo simples de Floresta Aleatória (Random Forest), eu pretendo melhorar esse modelo, mas nesse primeiro momento eu foquei eu fazer o app do streamlit.

Depois que terminei o modelo eu serializei ele e o _scaler_ usando o pacote `pickle`. Para fazer o _deploy_ do modelo e construir a visualização, usei o [streamlit](https://streamlit.io/).

![um gif mostrando como funciona o aplicativo de score de crédito streamlit](https://user-images.githubusercontent.com/33239902/183321842-be97fb04-f00b-4b62-8e6e-2b53d25335a0.gif)

Neste aplicativo, você pode preencher um formulário ou apenas selecionar um dos três perfis padrão fornecidos para ver como o modelo avalia a pontuação de crédito de cada pessoa. Ele também apresenta "a certeza" do modelo exibindo um gráfico de pizza com a probabilidade (em porcentagem) de cada grupo de score de crédito em que as respostas se encaixam. Ele também mostra o quanto cada recurso conta para sua pontuação de crédito, de acordo com esse modelo. Você pode ver o aplicativo ao vivo [aqui](https://devmedeiros-credit-score-classification-appstreamlit-app-fcakrl.streamlitapp.com/).

---

Todo o código está disponível no meu [repositório](https://github.com/devmedeiros/credit-score-classification-app) do GitHub. Além do código, lá você encontra a documentação, os dados originais e tratados (todas as etapas do tratamento), todos os requisitos para a construção deste projeto e como executá-lo localmente.