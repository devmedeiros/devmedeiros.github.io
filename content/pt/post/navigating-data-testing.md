---
title: "Navegando Data Testing"
date: 2023-11-20 18:45:00 -0300
categories: [Blog]
tags: [teste de dados, data testing, database, banco de dados, great expectations, assertr, engenheiro analítico, analytics engineer, engenheiro de dados, data engineering, SQL]
showtoc: true
summary: O que é Data Testing e porque você deveria se importar com isso?
cover:
    image: "https://ik.imagekit.io/devmedeiros/navigating-data-testing_Ka1fdcGdv.webp?tr=w-700"
    alt: "um barco a vela navegando com um por do sol ao fundo"
    caption: "Foto de [Bobby Stevenson](https://unsplash.com/pt-br/@bobbystevenson?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) na [Unsplash](https://unsplash.com/pt-br/fotografias/veleiro-branco-no-mar-durante-o-por-do-sol-3AEJ6imbQTo?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)"
  
---

## Introdução à Data Testing

Data testing (teste de dados) vem de teste de software da ciência da computação, quando é testado várias partes do código para determinar se ele está apto ou não para ser usado. Nesse mesmo sentido, quando estiver testando seus dados você quer avaliar se os dados são **acurados**, **completos** e **consistentes**, o quw significa que, respectivamente, você quer garantir que está extraindo e enviando o que você está esperando, garantir que não está perdendo dados através de transformações ou alterações no formato dos dados e garantir que quando estiver transportando seus dados do ponto A ou ponto B, eles sejam os mesmos e possuam a mesma informação.

Uma forma de fazer isso é através de estruturas básicas de banco de dados, definindo tipos e restrições à tabela. Mas se isso não for suficiente, você pode utilizar regras mais complexas usando outras ferramentas como [**Great Expectations**](https://greatexpectations.io/), e [**assertr**](https://docs.ropensci.org/assertr/).

## Estrutura Comum de Banco de Dados

Quando estiver criando ou alterando uma tabela em um banco de dados, você precisa especificar o tipo de dados da coluna e pode determinar diversas restrições. Esses tipos de dados determinam como os valores serão apresentados. Então imagine que você tem uma tabela com informações sobre os funcionários, como seus nomes, salários, e datas de nascimento.

| Nome | Salário   | Data de Nascimento |
|------|-----------|--------------------|
| Ana  | 3000.00   | 1988-02-02         |
| Bob  | 4000.00   | 1970-04-12         |
| Carl | 2000.00   | 1999-07-05         |

A coluna de salário seria do tipo numérico com dois dígitos, pois é assim que formatamos moedas. A data de nascimento seria do tipo "date" e o nome "string". Esses tipos de dados são uma forma de restrição, caso você crie uma tabela com esses tipos de dados, não poderá inserir uma nova linha com tipos de dados diferentes.

Mas você também pode acrescentar mais restrições, você poderia definir uma coluna para ser composta apenas de valores únicos ou que não aceita valores nulos/vazios.

Definir tipos de dados e restrições é vital para manter a integridade dos dados, pois sempre que alguém ou algum processo automático tentar adicionar ou alterar um pedaço de informação na tabela que possui as restrições, um erro surgirá. Esses erros ajudam o usuário a evitar erros de digitação ou inserção de opções inválidas que pode quebrar outras aplicações.

Imagine que você possui um sistema que manda um presente de aniversário para um empregado automaticamente. Se esse empregado não tiver uma data de nascimento registrada ele nunca receberá seu presente de aniversário. Então, quando tornamos impossível de deixar a data de nascimento vazia ajudamos usuários a prestar atenção e a usar o banco de dados de forma correta, mantendo tudo funcionando como deveria.

### Limitações

Quando geralmente pensamos em testes de unidades de dados, ou em testar seus dados para ter certeza de que nada está fora do comum, é fácil imaginar que confiar apenas nas restrições de dados é uma boa ideia, pois abrange a maioria dos casos de uso que normalmente usamos. Mas se pararmos para pensar nisso, podemos encontrar mais usos que nem sabíamos que precisamos.

Em nossa tabela de exemplo de funcionário, definir o salário como um tipo de dados numérico com decimais geralmente evita muitos erros potenciais, mas não impede que as pessoas tenham um salário negativo ou até mesmo um valor absurdo de salário. Claro que você pode definir o tamanho de um número, mas ele define apenas os dígitos; portanto, se seu salário máximo for 10.000, você terá que defini-lo como 9.999 ou 99.999.

Claro que ajuda, mas não resolve totalmente o problema.

Outro exemplo de limitação seria regras complexas, como por exemplo, se os empregados possuem níveis de senioridade como I, II, e III, e esses níveis acompanham faixas de salário, você não tem como ter uma regra alternativa que diz _"usuários da faixa I tem um salário entre 1000 e 2000"_.

## Além de Restrições

Se precisar definir regras de validação de dados mais personalizadas e/ou específicas de negócio, você pode usar [**Great Expectations**](https://greatexpectations.io/) e [**assertr**](https://docs.ropensci.org/assertr/). Ambos são ótimos para definir regras e parâmetros de validação para testar seus dados.

Você pode escrever testes definindo valores mínimos e máximos, verificar valores exclusivos, verificar se os valores estão dentro de um determinado conjunto e muito mais opções.

Em conclusão, explorar data testing envolve um equilíbrio cuidadoso entre confiar em estruturas de banco de dados convencionais e usar ferramentas especializadas como **Great Expectations** e **assertr**. Embora as restrições desempenhem um papel crucial na manutenção da integridade dos dados, o reconhecimento das suas limitações destaca a necessidade de regras de validação mais diferenciadas e específicas de negócio. Ao adotar essas ferramentas, os engenheiros e analistas de dados podem garantir não apenas a precisão e integridade dos seus dados, mas também navegar pelas complexidades dos cenários do mundo real.