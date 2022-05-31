---
title: Data Science Challenge - Churn Rate
date: 2022-05-30 16:49:00 -0300
categories: [Projects]
tags: [storytelling, python, análise de dados, aprendizado de máquina, rede neural]
showtoc: true
---

![heart with an A inside and you can read 'Alura Voz telecommunication company'](https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/aluravoz.png#center)

Eu fui desafiada a tomar o papel da nova cientista de dados na Alura Voz. Essa empresa fictícia é do ramo de telecomunicação e precisa reduzir sua taxa de evasão de clientes.

Esse desafio é dividido em quatro semanas. Para a primeira semana o objetivo é tratar o banco de dados proveniente de uma API. Em seguida, precisamos identificar clientes que são mais propensos a deixar a empresa, usando exploração e análise de dados. E então, na terceira semana, nós usamos modelos de _machine learning_ para prever a taxa de evasão da Alura Voz. A última semana é para expor o que fizemos durante o desafio e construir nosso portfolio. Caso esteja interessado em ver o código, ele está disponível no meu [repositório](https://github.com/devmedeiros/Challenge-Data-Science) do GitHub.

# Primeira Semana

## Lendo o Banco de Dados

O banco de dados foi disponibilizado no formato JSON e num primeiro momento aparenta ser um _data frame_ normal.

|  | customerID | Churn | customer | phone | internet | account |
|---|---|---|---|---|---|---|
| 0 | 0002-ORFBO | No | {'gender': 'Female', 'SeniorCitizen': 0, 'Part... | {'PhoneService': 'Yes', 'MultipleLines': 'No'} | {'InternetService': 'DSL', 'OnlineSecurity': '... | {'Contract': 'One year', 'PaperlessBilling': '... |
| 1 | 0003-MKNFE | No | {'gender': 'Male', 'SeniorCitizen': 0, 'Partne... | {'PhoneService': 'Yes', 'MultipleLines': 'Yes'} | {'InternetService': 'DSL', 'OnlineSecurity': '... | {'Contract': 'Month-to-month', 'PaperlessBilli... |
| 2 | 0004-TLHLJ | Yes | {'gender': 'Male', 'SeniorCitizen': 0, 'Partne... | {'PhoneService': 'Yes', 'MultipleLines': 'No'} | {'InternetService': 'Fiber optic', 'OnlineSecu... | {'Contract': 'Month-to-month', 'PaperlessBilli... |
| 3 | 0011-IGKFF | Yes | {'gender': 'Male', 'SeniorCitizen': 1, 'Partne... | {'PhoneService': 'Yes', 'MultipleLines': 'No'} | {'InternetService': 'Fiber optic', 'OnlineSecu... | {'Contract': 'Month-to-month', 'PaperlessBilli... |
| 4 | 0013-EXCHZ | Yes | {'gender': 'Female', 'SeniorCitizen': 1, 'Part... | {'PhoneService': 'Yes', 'MultipleLines': 'No'} | {'InternetService': 'Fiber optic', 'OnlineSecu... | {'Contract': 'Month-to-month', 'PaperlessBilli... |

Entretanto, como pode ser observado, `customer`, `phone`, `internet`, e `account` são suas próprias tabelas. Então eu normalizei elas separadamente e depois simplesmente concatenei todas essas tabelas em uma.

## Dados Faltantes

A primeira vez que eu procurei por dados faltantes nessa base nenhum foi encontrado, mas a medida que eu explorei os dados eu percebi que havia espaços em branco e vazios não sendo contados como `NaN`. Então eu corrigi isso e descobri que havia 224 dados faltantes para a variável `Churn` e 11 para `Charges.Total`.

Eu decidi desconsiderar os dados faltantes da variável `Churn`, pois este será nosso objeto de estudo e não há sentido em estudar algo que não existe. No caso dos dados faltantes de `Charges.Total`, eu imagino que representa um cliente que não pagou nada ainda, pois todos eles possuem 0 meses de contrato, ou seja, eles acabaram de se tornar clientes, então eu simplesmente substitui o valor faltante por 0.

## Codificação de Variáveis

A variável `SeniorCitizen` foi a única que veio com `0` e `1` ao invés de `Yes` e `No`. Por hora eu irei trocar esses valores por "yes" e "no", pois isto torna a análise mais simples de ser lida.

`Charges.Monthly` e `Charges.Total` foram renomeadas para perderem o ponto, pois isto atrapalha na hora de lidar com elas no _python_.

# Segunda Semana

## Análise de Dados

No primeiro gráfico podemos ver o quão desbalanceado nosso banco de dados é. Há mais de 5000 clientes que não deixaram a empresa e um pouco menos de 2000 que deixaram.

![bar plot with two bars, the first one is for 'no' and the second is for 'yes', the first bar is over 5000 count and the second one is around 2000](https://github.com/devmedeiros/Challenge-Data-Science/blob/main/2%20-%20Second%20Week/churn_count.jpg?raw=true#center)

Eu experimentei usar técnicas de sobreamostragem (_oversampling_) para lidar com esse deslanceamento, mas isto fez com que os modelos de aprendizado de máquina tivessem uma performance pior. E subamostragem (_undersampling_) não é uma opção com um banco de dados desse tamanho, então eu decidi deixar do jeito que está, e quando for hora de separar os dados de treino e teste eu irei estratificar o banco de acordo com a variável `Churn`.

Eu também gerei 16 gráficos para todas as variáveis discretas, eu optei por usar um grid de 6x3 para facilitar a leitura. O objetivo é ver se havia algum comportamento que fazia alguns cliente mais propensos a deixar a empresa. É claro que todas, exceto por `gender`, parecem ter algum papel em determinar se um cliente vai ou não deixar a empresa. Mais especificamente forma de pagamento, contratos, _backup online_, suporte técnico, e serviço de internet.

![there are 16 bar plots in the image showing how each feature is correlated with the churn rate](https://github.com/devmedeiros/Challenge-Data-Science/blob/main/2%20-%20Second%20Week/categorical_churn.jpg?raw=true#center)

No gráfico de `tenure`, eu decidi fazer gráficos de distribuição dos meses de contrato do cliente, um gráfico para os cliente que não evadiram e um para os que evadiram. Podemos ver que clientes que evadiram o fizeram no início do seu tempo na empresa.

![there are two plots side-by-side, in the first one the title is 'Churn = No' the data is along the tenure axis and is in a U shape. the second plot has the title 'Churn = Yes' and starts high and drops fast along the tenure line](https://github.com/devmedeiros/Challenge-Data-Science/blob/main/2%20-%20Second%20Week/tenure_churn.jpg?raw=true#center)

A cobrança mensal média para os cliente que não evadiram é de 61,27 unidades monetária, enquanto que clientes que evadiram pagam 74,44. Isso provavelmente é por conta do tipo de contrato que esses tipo de clintes preferem, mas de qualquer forma é senso comum que preços altos afastam clientes.

## O Perfil de Evasão

![person jumping through the window](https://64.media.tumblr.com/tumblr_lojvnhHFH91qlh1s6o1_400.gifv#center)

Considerando tudo que eu pude observar através de gráficos e medidas, eu fiz um perfil de clientes que são mais propensos a evadir a empresa.

- Clientes novos são mais propensos a evadir do que clientes antigos.

- Clientes com poucos serviços e produtos tendem a deixar a empresa. Se eles não estão presos a um contrato mais longo eles aparentam ser mais propensos a abandonar a empresa.

- Sobre os meios de pagamentos, cliente que evadem possuem uma preferência **forte** por cheques eletrônicos e usualmente gastam 13,17 unidades monetárias a mais que a média de clientes que não deixaram a empresa.

# Terceira Semana

## Preparando o Banco de Dados

Damos início fazendo variáveis _dummies_, de forma que teremos n-1 _dummies_ para n variáveis. Então fazemos uma matriz de correlação para avaliar a correlação das nossas variáveis.

![correlation matrix with all the features](https://github.com/devmedeiros/Challenge-Data-Science/blob/main/3%20-%20Third%20Week/corr_matrix.png?raw=true#center)

Podemos ver que a variável `InternetService_No` possui correlações altas com diversas outras variáveis, isso se dá porque as outras variáveis depende do cliente ter ou não acesso a internet. Então irei tirar essas variáveis dependentes do modelo. A mesma coisa ocorre com `PhoneService_Yes`.

`tenure` e `ChargesTotal` também possuem uma alta correlação, mas eu testei rodar os modelos sem uma das duas ou ambas e os modelos tiveram uma performance pior e levaram mais tempo para covergirem, então eu decidi manter elas no modelo, e elas são relevantes para o problema.

Após retirar essas variáveis eu termino de preparar o banco de dados com uma normalização das variáveis numéricas, `ChargesTotal` e `tenure`.

## Banco de Dados de Teste e Treino

Eu dividi o banco de dados em treino e teste, 20% para teste e o resto para treino. Eu estratifiquei os dados de acordo com a variável `Churn` e embaralhei os dados antes de separar. A mesma divisão de dados é usada em todos os modelos.

## Modelo Base

O modelo base foi feito através de um classificador _dummy_, basicamente ele diz que todos os clientes se comportam da mesma forma. Neste caso o modelo chutou que nenhum cliente iria deixar a empresa. Usando essa abordagem o modelo base obteve uma acurácia de `0,73456`.

## Modelo 1 - Florestas Aleatórias

Eu inicio usando uma busca no grid com validação cruzada (_grid search with cross-validation_) para encontrar os melhores parâmetros dentro de uma seleção de opções. O melhor modelo encontrado pela busca foi:

```python
RandomForestClassifier(max_depth=15, max_leaf_nodes=75, random_state=22)
```

Após ajustar o modelo a medida de acurácia foi de `0,78282`.

## Modelo 2 - Classificação de Vetores de Suporte Linear

Neste modelo eu usei os parâmetros padrões.

```python
LinearSVC(random_state=22)
```

Após o ajuste, a medida de acurácia foi de `0,78992`.

## Modelo 3 - Rede Neural Multicamada Perceptron

Aqui eu fixei o solucionador LBFGS, pois de acordo com a documentação do `sklearn` ele tem uma performance melhor em banco de dados pequenos [^1], e também fiz uma busca no grid com validação cruzada para encontrar o melhor tamanho da camada oculta. O melhor modelo foi:

```python
MLPClassifier(hidden_layer_sizes=(1,), max_iter=9999, random_state=22, solver='lbfgs')
```

A acurácia foi de `0,79489`.

## Conclusão

Após rodar os três modelos, todos usando o mesmo `random_state`. Eu encontrei as seguintes medidas de acurácia e melhorias no desempenho (comparado com o modelo base):

| **Modelo** | **Medida de Acc** | **Melhoria** |
|------------|:-----------------:|:------------:|
| Modelo Base|      0,73456      |       -      |
| Modelo 1   |      0,78282      |     6,57%    |
| Modelo 2   |      0,78992      |     7,54%    |
| Modelo 3   |      0,79489      |     8,21%    |

 O modelo com a maior acurácia é a rede neural com multicamada _perceptron_ com uma melhoria de 8,21%, comparado com o modelo base. No geral eu acho que esta foi uma melhoria boa considerando o tempo e recursos disponíveis.

 No fim, eu gostei desse desafio, pois é incomum eu praticar aprendizado de máquina, mas graças ao desafio eu tive a oportunidade de fazer um pequeno projeto nessa área que é tão importante e relevante. Essa foi a minha primeira vez trabalhando com redes neurais e ajuste de hiperparâmetros, e tenho certeza que na próxima vez terei resultados ainda melhores.

[^1]: [sklearn documentation](https://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html)
