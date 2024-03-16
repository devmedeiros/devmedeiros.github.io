---
title: Guia do Pacote de Python seaborn
date: 2021-11-07 15:14:00 -0300
categories: [Blog]
tags: [python, seaborn, visualização de dados, pandas]
summary: Um guia simples de como fazer gráficos básicos usando o pacote seaborn do Python
cover:
    image: "https://ik.imagekit.io/devmedeiros/python_seaborn/title-axis-outside-legend__zUIAf_2427.jpg?tr=w-700"
    alt: "a dispersion plot using seaborn"
    hidden: true
---

## Introdução

Eu estou aprendendo visualização de dados no Python e eu me vejo como alguém que aprende fazendo, por isso eu vou fazer alguns gráficos simples usando o pacote `seaborn` que poderão ser utilizados como referência sempre que precisar refrescar a memória.

Primeiramente é necessários que os pacotes estejam propriamente importados, após isso eu carrego o banco de dados _iris_.

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

url = "https://git.io/JXciW"

iris = pd.read_csv(url)
```

Caso não esteja familiarizado com o banco de dados _iris_, veja as cinco primeiras linhas dele a seguir:

|sepal_length | sepal_width | petal_length | petal_width | species |
|----|-----|-----|-----|--------|
|5.1 | 3.5 | 1.4 | 0.2 | setosa |
|4.9 | 3.0 | 1.4 | 0.2 | setosa |
|4.7 | 3.2 | 1.3 | 0.2 | setosa |
|4.6 | 3.1 | 1.5 | 0.2 | setosa |
|5.0 | 3.6 | 1.4 | 0.2 | setosa |

## Gráfico de barras

Criar um simples gráfico de barras.

```python
sns.barplot(x="species", y="petal_width", data=iris)
```

![seaborn barplot species x petal_width](https://ik.imagekit.io/devmedeiros/python_seaborn/barplot_EGCCNkum4y.jpg?updatedAt=1636308224965)

Fazendo um gráfico de barras horizontais.

```python
sns.barplot(x="petal_width", y="species", data=iris)
```

![seaborn barplot horizontal species x petal_width](https://ik.imagekit.io/devmedeiros/python_seaborn/horizontal-barplot_pcHXoAQWTH.jpg?updatedAt=1636308226028)

Ordem das barras personalizada.

```python
sns.barplot(
    x="species",
    y="petal_width",
    data=iris,
    order=["virginica", "setosa", "versicolor"])
```

![seaborn barplot custom bar order](https://ik.imagekit.io/devmedeiros/python_seaborn/barplot-custom-order_hUl5vUQOi.jpg?updatedAt=1636308225481)

Acrescentar limites para as barras de erro.

```python
sns.barplot(x="species", y="petal_width", data=iris, capsize=.2)
```

![seaborn barplot caps error](https://ik.imagekit.io/devmedeiros/python_seaborn/barplot-cap-error-bar_xD7fHewAZ.jpg?updatedAt=1636308225172)

Gráfico de barra sem barras de erro.

```python
sns.barplot(x="species", y="petal_width", data=iris, ci=None)
```

![barplot no error bar](https://ik.imagekit.io/devmedeiros/python_seaborn/barplot-no-cap-error_5sf2jPDpBag.jpg?updatedAt=1636308225717)

## Gráfico de dispersão

Um gráfico de dispersão simples.

```python
sns.scatterplot(x="sepal_width", y="petal_width", data=iris)
```

![seaborn scatterplot](https://ik.imagekit.io/devmedeiros/python_seaborn/scatterplot_nj8frw1JV.jpg?updatedAt=1636308224619)

Acrescentando grupos no gráfico de dispersão.

```python
sns.scatterplot(x="sepal_width", y="petal_width", data=iris, hue="species")
```

![seaborn scatterplot grouped](https://ik.imagekit.io/devmedeiros/python_seaborn/scatterplot-grouped_JHOKt9xydY.jpg?updatedAt=1636308224802)

Acrescentando grupos e escalando os pontos de um gráfico de dispersão.

```python
sns.scatterplot(
    x="sepal_width",
    y="petal_width",
    data=iris,
    hue="sepal_length",
    size="sepal_length")
```

![seaborn scatterplot grouped size](https://ik.imagekit.io/devmedeiros/python_seaborn/scatterplot-grouped-size_mZJt-TjEv.jpg?updatedAt=1636308224806)

## Legenda e Eixos

Para mover a legenda do gráfico para fora da área de plotagem, você pode utilizar `bbox_to_anchor = (1,1), loc=2`. O gráfico a seguir possui um titulo personalizado, um novo título para o eixo x e pro eixo y.

```python
sns.scatterplot(x="sepal_width", y="petal_width", data=iris, hue="species")
plt.legend(
    title="Species",
    bbox_to_anchor = (1,1),
    loc=2)
plt.xlabel("Sepal Width")
plt.ylabel("Petal Width")
plt.title("Sepal Width x Petal Width")
```

![seaborn scatterplot outside legend with custom title and axis labels](https://ik.imagekit.io/devmedeiros/python_seaborn/title-axis-outside-legend__zUIAf_2427.jpg?updatedAt=1636308224813)
