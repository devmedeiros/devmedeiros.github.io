---
title: Compreendendo o P-Valor
date: 2022-04-12 17:58:00 -0300
categories: [Blog]
tags: [estatística, p-valor]
showtoc: true
summary: A definição do p-valor, como usar ele, e como interpretar a medida
---

## O que é o p-valor?

Em estatística temos os testes de hipóteses, que são feitos para tomar uma decisão, de rejeitar ou *não* rejeitar a hipótese nula. Alguns exemplos de teste de hipótese são: Neyman-Pearson, Shapiro-Wilk, T de Student, entre outros.

Todos testes de hipóteses possuem uma estatística de teste específico dele. E essa estatística é utilizada para avaliar o teste, mas essa tarefa pode ser cansativa até com o uso de computadores, pois a maioria dos _softwares_ não devolvem como resposta a estatística de comparação, apenas a estatística amostral. Nesse caso o p-valor vem para facilitar essa comparação, pois ele já é uma representação dessa estatística de teste. Ele representa a probabilidade de se obter uma estatística de teste igual ou mais extrema que a calculada na sua amostra, considerando a hipótese nula como verdadeira.

Isso facilita, pois sabendo o nível de confiança que você quer testar a sua hipótese basta comparar se o p-valor é menor ou maior que o seu nível de confiança, enquanto que se fosse usar a estatística de teste ainda seria necessário calcular a estatística para cada nível de confiança diferente que você fosse comparar. Então suponha que queira comparar 1%, 5% e 10%, você teria que calcular três estatísticas de teste diferentes para comparar a sua estatística amostral.

## Como usar ele?

É muito comum quando estamos aprendendo estatística por conta própria lermos que se o p-valor for menor que 5% rejeitamos a hipótese ou que se for maior "aceitamos" a hipótese.

O valor com o qual comparamos o p-valor deve ser definido juntamente com pessoas da área de negócio do que você está trabalhando, é muito comum em alguns setores utilizarem um p-valor muito pequeno como 1% ou até 0,1% e em outros usarem valores maiores de que 5%.

Veja que um p-valor de 0,02 seria rejeitado se considerarmos `α = 1%`, mas não seria se `α = 5%`. Na dúvida sobre qual usar, primeiramente é recomendado tomar essa decisão antes de fazer o teste. Segundo, tenha em mente que um `α = 1%` vai ter uma confiança de `99% (1-α)`, enquanto que se fosse 5% seria ~~apenas~~ 95%. Pode parecer que é melhor pegar o valor que lhe dá mais "confiança", mas um p-valor muito pequeno pode levar a mais rejeições da sua hipótese.

A verdade é que os testes de hipóteses só nos dão a informação da rejeição, quando uma hipótese nula não é rejeitada isso quer dizer que não foram encontradas evidências que contradizem o que ela afirma, isso não quer dizer que provamos que ela está correta. Então é preciso tomar muito cuidado ao utilizar o p-valor.

## Interpretando a medida

Uma forma de interpretar a medida muito comum é _"Rejeita-se a hipótese nula, com α% de confiança"_, no caso de rejeição (em que o `p-valor > α`) e no caso de *não* rejeição (`p-valor < α`) _"Não foram encontradas evidências suficientes para rejeitar a hipótese nula, com α% de confiança"_.
