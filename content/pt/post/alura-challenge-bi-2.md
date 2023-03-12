---
title: Alura Challenge BI 2
date: 2022-06-30 19:23:00 -0300
categories: [Blog]
tags: [visualização de dados, dashboard, powerbi, SQL, business intelligence]
summary: Alura ofereceu um desafio de quatro semanas Challenge BI, onde os participantes precisavam fazer 3 dashboards
cover:
    image: "https://user-images.githubusercontent.com/33239902/176906617-80e0a1c3-3b3a-4f26-9ee1-2747ba1e00e1.gif#vitrinedev"
    alt: "a gif showing how alura food report works"
    hidden: true
---

O Alura Challenge BI 2 foi um evento que durou quatro semanas, no qual nós fomos desafiados a fazer um painél de _Business Intelligence_ com base nos requisitos de três empresas fictícias. Durante o evento eu fiz esses três painés usando o Power BI, você pode ver também os arquivos fontes dos painéis no meu [repositório](https://github.com/devmedeiros/Alura-Challenge-BI-2).

{{< linebreak >}}

{{< tabs tabTotal="3">}}
{{< tab tabName="Alura Films" >}}

<p>O objetivo deste painel é ajudar a encontrar a melhor seleção para um filme que será produzido.</p>

<iframe title="Relatório Alura Films" width="100%" height="450" src="https://app.powerbi.com/view?r=eyJrIjoiZTllNjE2ZTQtNjdkMy00OGI4LTllNTAtM2RhNGE2YWU4YmZlIiwidCI6IjI2ZjA4NzIyLTFjOWUtNGVkZS1iN2VkLThhMmI3N2ZmM2Q5YyJ9&pageName=ReportSection11c983afd307f2fa2f9a" frameborder="0" allowFullScreen="true"></iframe>

<p>Com isso em mente, eu explorei na primeira aba um pequeno resumo sobre os filmes no banco de dados, com ela você pode ter uma ideia geral dos nossos dados. Na segunda aba é apresentado a nota no IMDB e o Meta Score dos filmes. Eu também criei uma medida que mostra o quanto essas duas avaliações concordam entre si. Uma medida de 100 quer dizer que <strong>não concordam nem um pouco</strong> e 0 quer dizer <strong>concordam completamente</strong>, a medida de concordância foi de 9,48.</p>
<p>Nossa terceira aba apresenta informação sobre as estrelas dos filmes, os 10 atores com a maior rentabilidade, e os 10 atores com a maior contagem de filmes.</p>
<p>A quarta aba apresenta a distribuição da renda bruta pela quantidade de gêneros diferentes que um filme tem. Ela também mostra quantos filmes tem um certo gênero, como por exemplo, <strong>Drama</strong> é a escolha mais popular para gênero de filme, com 72% dos filmes do banco de dados. Por fim, essa aba também mostra a renda bruta média por gênero.</p>
<p>A quinta e última aba mostra um pouco de informação sobre a classificação indicativa brasileira e mostra o Meta Score médio para filmes com certificação. Ela também mostra a renda bruta, em média, para cada classificação indicativa.</p>

{{< /tab >}}
{{< tab tabName="Alura Food" >}}

<p>A <strong>Alura Food</strong> está interessada em expandir seus negócios para o mercado indiano. Para isso, a empresa pediu para calcular medidas que os ajudem a tomarem uma melhor decisão.</p>

<iframe title="Relatório Alura Food" width="100%" height="450" src="https://app.powerbi.com/view?r=eyJrIjoiNGEzMTQzMGYtNzFhNC00YjcyLThiMDMtMGE2YTMzMDRhODFiIiwidCI6IjI2ZjA4NzIyLTFjOWUtNGVkZS1iN2VkLThhMmI3N2ZmM2Q5YyJ9&pageName=ReportSection" frameborder="0" allowFullScreen="true"></iframe>

<p>Primeiramente, eu fiz uma junção dos bancos de dados e limpei eles através do Power BI, traduzi alguns dos textos de inglês para português através da Planilhas Google, e converti o preço das refeições da sua moeda original para o real (moeda brasileira). Por fim, eu usei o Figma para fazer o background, incluindo imagens e títulos.</p>
<p>Neste projeto, eu optei por usar apenas uma página para mostrar toda a informação pedida, pois eu acredito que isso facilita a análise dos dados.</p>
<p>A maioria dos restaurantes neste mercado indiano não oferecem entrega online, com apenas 19,21% deles tendo este serviço. A avaliação média dos restaurantes é de 3,72, de um total de 5, essa avaliação também é apresentada em formato textual, com um <strong>Muito Bom</strong> sendo uma nota acima de 4.</p>
<p>O preço médio de uma refeição por pessoa é de R$ 39,48 (em torno de USD 7,65). E há 9577 restaurantes diferentes em todo o banco de dados, dos quais 3968, especializam na culinária do norte indiano. Nova Delhi é, de longe, o local mais popular para se abrir um restaurante, com 5473 deles, a segunda cidade é Gurgaon, com 1118 restaurantes.</p>

{{< /tab >}}
{{< tab tabName="Alura Skimó">}}

<p>A <strong>Alura Skimo</strong> está interessada em analisar seus dados de vendas, para ajudar com isso eu fiz este <i>dashboard</i>.</p>

<iframe title="Relatório Alura Food" width="100%" height="450" src="https://app.powerbi.com/view?r=eyJrIjoiNTllYjJmODItYzQxNC00ZmEzLTk5ZGMtZDgzNzY2NDM2ZGMxIiwidCI6IjI2ZjA4NzIyLTFjOWUtNGVkZS1iN2VkLThhMmI3N2ZmM2Q5YyJ9" frameborder="0" allowFullScreen="true"></iframe>

<p>Ele é composto de três páginas. A primeira apresenta um resumo com todas as principais medidas que encontramos no painel, filtrado para mostrar o mês mais recente. Nesta primeira página, você pode parar o cursor do mouse sobre uma medida e isso fará com que apareça um pequeno gráfico com a série histórica com uma linha de tendência.</p>
<p>Seguindo para a próxima página, nela você pode ver toda a informação sobre os produtos vendidos pela <strong>Alura Skimo</strong>. Você pode filtrar os dados por sabor de sorvete, tipo de embalagem, categoria, e custo do produto. Ela mostra a informação básica sobre os produtos, junto de um ranque dos produtos que são os mais bem vendidos e dois gráficos que mostram as vendas por sabor e vendas por categoria.</p>
<p>Na última página, você pode encontrar informações sobre os vendedores da nossa empresa. Ele mostra quando o funcionário entrou para a empresa, a comissão deles, o faturamento e quantas vendas cada um fez, tudo isso no último ano (2018).</p>
<p>Esse painel foi complexo comparado aos outros dois, pois nosso banco de dados veio de arquivos SQL. Primeiro eu tive que criar o banco de dados e carregar cada arquivo SQL para ele, então eu só tive que carregar o banco para o Power BI. Por fim, toda a limpeza, tratamento e processamento de dados foi feito no próprio Power BI.</p>

{{< /tab >}}
{{< /tabs >}}