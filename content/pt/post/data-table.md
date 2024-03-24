---
title: Uma Visão Geral Sobre o Pacote data.table do R
date: 2021-10-27 09:41:00 -0300
categories: [Blog]
keywords: [R, data.table, manipulação de dados, transformação de dados]
tags: [R, Transformação de Dados]
summary: Um dos pacotes de manipulação de dados mais rápidos do R
aliases:
- 2021-10-27-data-table
cover:
    image: "https://ik.imagekit.io/devmedeiros/code_gASTmaW7t.webp?tr=w-700"
    alt: "janela com código data.table"
---

O pacote `data.table` é um dos pacotes de manipulação de dados mais rápido, atualmente ele é mais rápido até que o `pandas` e `dplyr` [^1]. A sintaxe de um `data.table` é `dt[i, j, by]`, em que:

- i é utilizado para amostrar linhas
- j é utilizado para amostrar colunas
- by é utilizado para amostrar grupos, igual ao **GROUP BY** do SQL

Você pode ler em _voz alta_ como[^2]:

>Pegue `dt`, amostra/reordene as linhas usando `i`, então calcule `j`, agrupando por `by`.

Um `data.table` também é um `data.frame` e todas as manipulações de dados básicas que você pode usar em um `data.frame` se aplica a um `data.table`. Como `ncol()`, `nrow()`, `names()`, `summary()`. Mas ele não para por aí, por exemplo `data.table` possui uma variável especial `.N` que é um integral que contains os números das linhas no grupo. Se você usar `dt[.N]` você verá a última linha do seu `data.table`.

Outra coisa interessante do `data.table` é que se você quiser filtrar/amostrar uma coluna, você não precisa utilizar `df$x[df$x == 1]` você pode simplesmente usar `dt[x == 1]` o que torna o seu código mais limpo e fácil de ler.

Você também tem a possibilidade de utilizar operadores especiais como: `%like%`, `%in%` e `%between%`. Estes operadores funcionam como operadores de SQL, **LIKE**, **IN**, e **BETWEEN**, respectivamente.

Se você está familiarizado com SQL tem uma coisa que esse pacote oferece que irá chamar a sua atenção. No inglês se chama _chaining_ (acorrentar), que permite a você a fazer uma sequência de operações em um `data.table`, você apenas precisa utilizar `dt[][]` e _acorrentar_ múltiplas operações com "[]".

Mas este não são todos os operadores, com `:=` você pode alterar dados sem fazer uma nova cópia na memória.

Caso queira começar a usar o pacote eu sugiro que use a [cheatsheet](https://raw.githubusercontent.com/rstudio/cheatsheets/master/datatable.pdf) (folha de referência/colinha). É extramente útil caso já tenha um conhecimento básico sobre `data.frame`s.

[^1]: https://h2oai.github.io/db-benchmark/
[^2]: https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html
