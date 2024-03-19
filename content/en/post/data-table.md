---
title: An Overview on data.table R Package
date: 2021-10-27 09:41:00 -0300
categories: [Blog]
tags: [R, data.table, data manipulation, data transformation]
summary: One of the fastest packages for data manipulation on R
aliases:
- 2021-10-27-data-table
cover:
    image: "https://ik.imagekit.io/devmedeiros/code_gASTmaW7t.webp?tr=w-700"
    alt: "window with data.table code"
---

The `data.table` package is one of the fastest packages for data manipulation, currently, it is even faster than `pandas` and `dplyr` [^1]. `data.table` syntax is `dt[i, j, by]`, where:

- i is used to subset rows
- j is used to subset columns
- by is used to subset groups, like **GROUP BY** from SQL

You can read it _out loud_  as[^2]:

>Take `dt`, subset/reorder rows using `i`, then calculate `j`, grouped by `by`.

A `data.table` is also a `data.frame` and all of the basic data manipulations you can use in `data.frame`s applies to `data.table`. Like `ncol()`, `nrow()`, `names()`, `summary()`. But it has more possibilities, for instance in `data.table` there is a special variable `.N` which is an integer that contains the row number in the group. If you use `dt[.N]` you'll get the last row of your `data.table`.

Another cool feature of `data.table` is that if you want filter/subset a column you don't need to use `df$x[df$x == 1]` you can simple use `dt[x == 1]` which make your code much more readable and clean.

You also get to use special operators: `%like%`, `%in%` and `%between%`. These operators work like SQL operators, **LIKE**, **IN**, and **BETWEEN**, respectively.

If you are familiar with SQL there is this one thing the package offers that will catch your eye. It's called _chaining_, which allows you to perform a sequence of operations in a `data.table`, you just need to use `dt[][]` and chain multiple operations "[]".

But that's not all with the operator `:=` you can alter data without making a new copy in the memory.

If you want to start using the package I suggest you use the [cheatsheet](https://raw.githubusercontent.com/rstudio/cheatsheets/master/datatable.pdf). It's really useful if you already have a basic knowledge about `data.frame`s.

[^1]: https://h2oai.github.io/db-benchmark/
[^2]: https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html
