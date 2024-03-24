---
title: Data Science Challenge - Churn Rate
date: 2022-05-30 16:49:00 -0300
lastmod: 2022-06-09 18:08:00 -0300
categories: [Blog]
keywords: [storytelling, python, análise de dados, aprendizado de máquina, rede neural]
tags: [Storytelling, Python, Análise de Dados, Aprendizado de Máquina]
showtoc: true
summary: Alura fez um desafio de quatro semanas "Data Science Challenge" utilizando um banco desbalanceado da taxa de evasão da empresa Alura Voz
aliases:
- 2022-05-30-churn-rate-challenge
cover:
    image: "https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/aluravoz.png"
    alt: "heart with an A inside and you can read 'Alura Voz telecommunication company'"
---

Eu fui desafiada a tomar o papel da nova cientista de dados na Alura Voz. Essa empresa fictícia é do ramo de telecomunicação e precisa reduzir sua taxa de evasão de clientes.

Esse desafio é dividido em quatro semanas. Para a primeira semana o objetivo é tratar o banco de dados proveniente de uma API. Em seguida, precisamos identificar clientes que são mais propensos a deixar a empresa, usando exploração e análise de dados. E então, na terceira semana, nós usamos modelos de _machine learning_ para prever a taxa de evasão da Alura Voz. A última semana é para expor o que fizemos durante o desafio e construir nosso portfolio. Caso esteja interessado em ver o código, ele está disponível no meu [repositório](https://github.com/devmedeiros/Challenge-Data-Science) do GitHub.

## Primeira Semana

### Lendo o Banco de Dados

O banco de dados foi disponibilizado no formato JSON e num primeiro momento aparenta ser um _data frame_ normal.

![table head with the first five rows](https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/1%20-%20Data%20Cleaning/table_head.png#center)

Entretanto, como pode ser observado, `customer`, `phone`, `internet`, e `account` são suas próprias tabelas. Então eu normalizei elas separadamente e depois simplesmente concatenei todas essas tabelas em uma.

### Dados Faltantes

A primeira vez que eu procurei por dados faltantes nessa base nenhum foi encontrado, mas a medida que eu explorei os dados eu percebi que havia espaços em branco e vazios não sendo contados como `NaN`. Então eu corrigi isso e descobri que havia 224 dados faltantes para a variável `Churn` e 11 para `Charges.Total`.

Eu decidi desconsiderar os dados faltantes da variável `Churn`, pois este será nosso objeto de estudo e não há sentido em estudar algo que não existe. No caso dos dados faltantes de `Charges.Total`, eu imagino que representa um cliente que não pagou nada ainda, pois todos eles possuem 0 meses de contrato, ou seja, eles acabaram de se tornar clientes, então eu simplesmente substitui o valor faltante por 0.

### Codificação de Variáveis

A variável `SeniorCitizen` foi a única que veio com `0` e `1` ao invés de `Yes` e `No`. Por hora eu irei trocar esses valores por "yes" e "no", pois isto torna a análise mais simples de ser lida.

`Charges.Monthly` e `Charges.Total` foram renomeadas para perderem o ponto, pois isto atrapalha na hora de lidar com elas no _python_.

## Segunda Semana

### Análise de Dados

No primeiro gráfico podemos ver o quão desbalanceado nosso banco de dados é. Há mais de 5000 clientes que não deixaram a empresa e um pouco menos de 2000 que deixaram.

![bar plot with two bars, the first one is for 'no' and the second is for 'yes', the first bar is over 5000 count and the second one is around 2000](https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/2%20-%20Data%20Analysis/churn.jpg#center)

Eu experimentei usar técnicas de sobreamostragem (_oversampling_) para lidar com esse deslanceamento, mas isto fez com que os modelos de aprendizado de máquina tivessem uma performance pior. E subamostragem (_undersampling_) não é uma opção com um banco de dados desse tamanho, então eu decidi deixar do jeito que está, e quando for hora de separar os dados de treino e teste eu irei estratificar o banco de acordo com a variável `Churn`.

Eu também gerei 16 gráficos para todas as variáveis discretas, para ver todos os gráficos olhe este [notebook](https://github.com/devmedeiros/Challenge-Data-Science/blob/main/2%20-%20Data%20Analysis/data_analysis.ipynb). O objetivo é ver se havia algum comportamento que fazia alguns cliente mais propensos a deixar a empresa. É claro que todas, exceto por `gender`, parecem ter algum papel em determinar se um cliente vai ou não deixar a empresa. Mais especificamente forma de pagamento, contratos, _backup online_, suporte técnico, e serviço de internet.

No gráfico de `tenure`, eu decidi fazer gráficos de distribuição dos meses de contrato do cliente, um gráfico para os cliente que não evadiram e um para os que evadiram. Podemos ver que clientes que evadiram o fizeram no início do seu tempo na empresa.

![there are two plots side-by-side, in the first one the title is 'Churn = No' the data is along the tenure axis and is in a U shape. the second plot has the title 'Churn = Yes' and starts high and drops fast along the tenure line](https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/2%20-%20Data%20Analysis/tenure.jpg#center)

A cobrança mensal média para os cliente que não evadiram é de 61,27 unidades monetária, enquanto que clientes que evadiram pagam 74,44. Isso provavelmente é por conta do tipo de contrato que esses tipo de clintes preferem, mas de qualquer forma é senso comum que preços altos afastam clientes.

### O Perfil de Evasão

![person jumping through the window](https://64.media.tumblr.com/tumblr_lojvnhHFH91qlh1s6o1_400.gifv#center)

Considerando tudo que eu pude observar através de gráficos e medidas, eu fiz um perfil de clientes que são mais propensos a evadir a empresa.

- Clientes novos são mais propensos a evadir do que clientes antigos.

- Clientes com poucos serviços e produtos tendem a deixar a empresa. Se eles não estão presos a um contrato mais longo eles aparentam ser mais propensos a abandonar a empresa.

- Sobre os meios de pagamentos, cliente que evadem possuem uma preferência **forte** por cheques eletrônicos e usualmente gastam 13,17 unidades monetárias a mais que a média de clientes que não deixaram a empresa.

## Terceira Semana

### Preparando o Banco de Dados

Damos início fazendo variáveis _dummies_, de forma que teremos n-1 _dummies_ para n variáveis. Então fazemos uma matriz de correlação para avaliar a correlação das nossas variáveis.

![correlation matrix with all the features](https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/3%20-%20Model%20Selection/corr_matrix.jpg#center)

Podemos ver que a variável `InternetService_No` possui correlações altas com diversas outras variáveis, isso se dá porque as outras variáveis depende do cliente ter ou não acesso a internet. Então irei tirar essas variáveis dependentes do modelo. A mesma coisa ocorre com `PhoneService_Yes`.

`tenure` e `ChargesTotal` também possuem uma alta correlação, mas eu testei rodar os modelos sem uma das duas ou ambas e os modelos tiveram uma performance pior e levaram mais tempo para covergirem, então eu decidi manter elas no modelo, e elas são relevantes para o problema.

Após retirar essas variáveis eu termino de preparar o banco de dados com uma normalização das variáveis numéricas, `ChargesTotal` e `tenure`.

### Banco de Dados de Teste e Treino

Eu dividi o banco de dados em treino e teste, 20% para teste e o resto para treino. Eu estratifiquei os dados de acordo com a variável `Churn` e embaralhei os dados antes de separar. A mesma divisão de dados é usada em todos os modelos. Após separar os dados eu decidi fazer uma sobreamostragem (_oversampling_) dos dados de **teste** usando SMOTE[^1], pois os dados são muito desbalanceados. O motivo de eu usar essa técnica apenas nos dados de teste é que eu não quero ter um resultado viesado, se eu sobreamostrar todo o banco de dados isso quer dizer que eu vou testar meu modelo no mesmo dado que eu o treinei, e este não é meu objetivo.

### Avaliação dos Modelos

Eu vou utilizar um classificador _dummy_ para ter uma base para a medida de acurácia, e eu também vou utilizar as métricas: `precision` (precisão), `recall` (recordação) and `f1 score` (medida f1)[^2]. Apesar de que o modelo _dummy_ não ter valor para essas métricas eu vou manter ele para comparar a melhora dos modelos.

### Modelo Base

O modelo base foi feito através de um classificador _dummy_, basicamente ele diz que todos os clientes se comportam da mesma forma. Neste caso o modelo chutou que nenhum cliente iria deixar a empresa. Usando essa abordagem o modelo base obteve uma acurácia de `0,73456`.

A seguir todos os modelos terão a mesma semente aleatória (_random state_).

### Modelo 1 - Florestas Aleatórias

Eu inicio usando uma busca no grid com validação cruzada (_grid search with cross-validation_) para encontrar os melhores parâmetros dentro de uma seleção de opções utilizando o `recall` como estratégia para avaliar a performance. O melhor modelo encontrado pela busca foi:

```python
RandomForestClassifier(criterion='entropy', max_depth=5, max_leaf_nodes=70, random_state=22)
```

Após ajustar o modelo, as medidas de avaliação foram:
- Medida Accuracy: 0,72534 
- Medida Precision: 0,48922 
- Medida Recall: 0,78877 
- Medida F1: 0,60389

### Modelo 2 - Classificação de Vetores de Suporte Linear

Neste modelo eu usei os parâmetros padrões e aumentei o teto para o máximo de iterações para `900000`.

```python
LinearSVC(max_iter=900000, random_state=22)
```

Após ajustar o modelo, as medidas de avaliação foram:
- Medida Accuracy: 0,71966 
- Medida Precision: 0,48217 
- Medida Recall: 0,75936 
- Medida F1: 0,58982

### Modelo 3 - Rede Neural Multicamada Perceptron

Aqui eu fixei o solucionador LBFGS, pois de acordo com a documentação do `scikit-learn` ele tem uma performance melhor em banco de dados pequenos [^3], e também fiz uma busca no grid com validação cruzada para encontrar o melhor tamanho da camada oculta. O melhor modelo foi:

```python
MLPClassifier(hidden_layer_sizes=(1,), max_iter=9999, random_state=22, solver='lbfgs')
```

Após ajustar o modelo, as medidas de avaliação foram:
- Medida Accuracy: 0,72818 
- Medida Precision: 0,49133 
- Medida Recall: 0,68182 
- Medida F1: 0,57111

### Conclusão

Após rodar os três modelos, todos usando o mesmo `random_state`. Eu encontrei as seguintes medidas de acurácia e melhorias no desempenho (comparado com o modelo base):

![results table](https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/3%20-%20Model%20Selection/results_table.png#center)

No fim, a Floresta Aleatória teve as melhores métricas. Este modelo consegue _recordar_ uma grande parte dos clientes que evadem corretamente, ainda não é perfeito, mas já é um ponto de partida. A medida de _acurácia_ não é tão alta como eu gostaria, mas para este problema em particular o objetivo é impedir os clientes de deixar a empresa e é melhor utilizar recursos para manter um cliente que não vai deixar a empresa do que não fazer nada.

No fim, eu gostei desse desafio, pois é raro eu praticar aprendizado de máquina, mas graças ao desafio eu tive a oportunidade de fazer um pequeno projeto nessa área que é tão importante e relevante. Essa foi a minha primeira vez trabalhando com redes neurais e ajuste de hiperparâmetros, e tenho certeza que na próxima vez terei resultados ainda melhores.

[^1]: [imbalanced-learn documentation](https://imbalanced-learn.org/stable/references/generated/imblearn.over_sampling.SMOTE.html)
[^2]: [Accuracy, Precision, Recall or F1? - Koo Ping Shung](https://towardsdatascience.com/accuracy-precision-recall-or-f1-331fb37c5cb9)
[^3]: [scikit-learn documentation](https://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html)
