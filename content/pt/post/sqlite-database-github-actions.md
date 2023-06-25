---
title: "Como Usar o Banco SQLite com o Github Actions"
date: 2023-05-31 21:54:00 -0300
lastmod: 2023-06-25 15:43:00 -0300
categories: [Blog]
tags: [github, gitlab, github actions, ci/cd, sqlite, database, banco de dados, scrapping, python, tutorial, SQL, git]
showtoc: true
summary: Descubra uma alternativa de graça a computação em nuvem para data scraping com GitHub Actions!
cover:
    image: "https://i.imgur.com/GoVtXjz.jpg"
    alt: "um desenho no horizonte de uma rede de eletricidade simbolizando a conexão do sqlite e github actions"
    caption: "Foto de kiwi thompson na Unsplash"
---

Recentemente eu venho trabalhando com um projeto de raspagem de dados (web scrapping) que trabalha com uma pequena quantidade de dados, tão pequena que eu poderia usar tranquilamente os recursos gratuitos dos serviços de cloud disponíveis, mas eu não gosto de correr o risco de ser cobrada por um projeto pessoal. Para resolver isso eu estive procurando uma alternativa gratuita em que eu também possa compartilhar os dados e que rode com o Github Actions.

Caso queira checar o repositório que contém o código discutido neste post, siga este [link](https://github.com/devmedeiros/template-sqlite-actions).

> Eu vou ilustrar como integrar o banco de dados SQLite com o Github Actions usando Python, mas caso você saiba como modificar um arquivo usando outra linguagem de programação esse post ainda é relevante para você.

## Escrevendo seu Gerador de Dados/Scrapper

Primeiro, seu projeto precisa estar em um repositório, no meu caso estou usando o Github. Eu escrevi um código Python que faz uma raspagem de dados numa página web e salva esses dados num banco SQLite, nesse exemplo vou ilustrar isso com um código mais simples.

```python
# Importe as bibliotecas
import pandas as pd
import random
from sqlalchemy import create_engine
from datetime import datetime

# Crie os dados aleatórios
people = ['Ana', 'Bob', 'Charles', 'Daiana']
values = [random.random(), random.random(), random.random(), random.random()]

# Crie uma conexão com o banco de dados
engine = create_engine('sqlite:///data.db')

# Crie o data frame
data = pd.DataFrame({'people': people, 'values': values, 'load_date': datetime.now()})

# Salve o data frame
data.to_sql('data', if_exists='append', con=engine, index=False)
```

## Configurando o Workflow

Se você rodar o código acima diversas vezes numa máquina local ele irá funcionar, mas você irá notar que no Github ele não irá persistir as mudanças, isso é porque é necessário fazer um commit. Para fazer isso você precisará criar um workflow, no seu repositório crie um arquivo yaml na pasta `.github/workflow`. Esse arquivo será o seu workflow, você pode escolher qualquer nome que quiser.

```yaml
# dê um nome para o workflow
name: random data

# defina a frequência que ele irá rodar
on:
  schedule:
    - cron: "0 * * * *" # a cada hora
  workflow_dispatch:

env:
  ACTIONS_ALLOW_UNSECURE_COMMANDS: true

# crie as tarefas/jobs
jobs:
  generate-latest:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4

        with:
          python-version: '3.10' # fixa o python para a versão 3.10

      - name: Install requirements
        run: pip3 install -r requirements.txt # configura o ambiente

      - name: Run random data
        run: python main.py

      - name: Commit changes
        run: |
          git config --global user.name "github-actions"
          git config --global user.email "action@github.com"

          git add -A
          git commit -m "add more data"
      
      - name: Push changes
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
```

Não se esqueça de habilitar as permissões do seu workflow, no seu repositório navegue até `Settings > Actions > General`, e selecione **Read and write permissions**.

![worflow permissions do site oficial do github](https://i.imgur.com/pOym60i.png#center)

## Conclusão

Essa pode ser uma boa alternativa gratuita caso você queira ser capaz de compartilhar os dados que você está coletando ou gerando. Mas não se esqueça de ficar atento nas limitações do Github quando usando a versão free. Veja os limites de uso atuais no [site oficial](https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration#usage-limits) deles.

---

Caso queira ver uma aplicação real do que foi discutido aqui, você pode checar esse [respositório](https://github.com/devmedeiros/nota-fiscal-goiana). Onde eu implementei um scrapper mensal que coleta os dados e os disponibiliza num banco SQLite para todos.