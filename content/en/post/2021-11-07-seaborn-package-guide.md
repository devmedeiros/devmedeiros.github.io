---
title: Python seaborn Package Guide
date: 2021-11-07 15:14:00 -0300
categories: [Blog]
tags: [python, seaborn, data visualization, pandas]
summary: A simple guide on how to make basic plots using the seaborn package from Python
cover:
    image: "https://ik.imagekit.io/devmedeiros/python_seaborn/title-axis-outside-legend__zUIAf_2427.jpg?tr=w-700"
    alt: "a dispersion plot using seaborn"
    hidden: true
---

## Introduction

I'm learning data visualization in Python and I see myself as a 'hands on' learner, so I'll be reproducing some basic plots using `seaborn` package that you can use as a reference everytime you need to fresh up your memory.

At first is required that the packages are properly imported, after that I load the iris dataset.

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

url = "https://git.io/JXciW"

iris = pd.read_csv(url)
```

If you're not familiar with the iris dataset, you can see its first five rows below:

|sepal_length | sepal_width | petal_length | petal_width | species |
|----|-----|-----|-----|--------|
|5.1 | 3.5 | 1.4 | 0.2 | setosa |
|4.9 | 3.0 | 1.4 | 0.2 | setosa |
|4.7 | 3.2 | 1.3 | 0.2 | setosa |
|4.6 | 3.1 | 1.5 | 0.2 | setosa |
|5.0 | 3.6 | 1.4 | 0.2 | setosa |

## Barplots

To create simple barplots.

```python
sns.barplot(x="species", y="petal_width", data=iris)
```

![seaborn barplot species x petal_width](https://ik.imagekit.io/devmedeiros/python_seaborn/barplot_EGCCNkum4y.jpg?updatedAt=1636308224965)

Making a horizontal barplot.

```python
sns.barplot(x="petal_width", y="species", data=iris)
```

![seaborn barplot horizontal species x petal_width](https://ik.imagekit.io/devmedeiros/python_seaborn/horizontal-barplot_pcHXoAQWTH.jpg?updatedAt=1636308226028)

Custom bar order.

```python
sns.barplot(
    x="species",
    y="petal_width",
    data=iris,
    order=["virginica", "setosa", "versicolor"])
```

![seaborn barplot custom bar order](https://ik.imagekit.io/devmedeiros/python_seaborn/barplot-custom-order_hUl5vUQOi.jpg?updatedAt=1636308225481)

Add caps to error bars.

```python
sns.barplot(x="species", y="petal_width", data=iris, capsize=.2)
```

![seaborn barplot caps error](https://ik.imagekit.io/devmedeiros/python_seaborn/barplot-cap-error-bar_xD7fHewAZ.jpg?updatedAt=1636308225172)

Barplot withough error bar.

```python
sns.barplot(x="species", y="petal_width", data=iris, ci=None)
```

![barplot no error bar](https://ik.imagekit.io/devmedeiros/python_seaborn/barplot-no-cap-error_5sf2jPDpBag.jpg?updatedAt=1636308225717)

## Scatterplots

A simple scatterplot.

```python
sns.scatterplot(x="sepal_width", y="petal_width", data=iris)
```

![seaborn scatterplot](https://ik.imagekit.io/devmedeiros/python_seaborn/scatterplot_nj8frw1JV.jpg?updatedAt=1636308224619)

Mapping groups to scatterplot.

```python
sns.scatterplot(x="sepal_width", y="petal_width", data=iris, hue="species")
```

![seaborn scatterplot grouped](https://ik.imagekit.io/devmedeiros/python_seaborn/scatterplot-grouped_JHOKt9xydY.jpg?updatedAt=1636308224802)

Mapping groups and scalling scatterplot.

```python
sns.scatterplot(
    x="sepal_width",
    y="petal_width",
    data=iris,
    hue="sepal_length",
    size="sepal_length")
```

![seaborn scatterplot grouped size](https://ik.imagekit.io/devmedeiros/python_seaborn/scatterplot-grouped-size_mZJt-TjEv.jpg?updatedAt=1636308224806)

## Legend and Axes

To change the plot legend to the outside of the plot area, you can use `bbox_to_anchor = (1,1), loc=2`. The following plot has a custom title, a new x axis label, and a y axis label.

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
