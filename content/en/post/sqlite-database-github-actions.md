---
title: "How to Use SQLite Database with Github Actions"
date: 2023-05-31 21:54:00 -0300
lastmod: 2024-03-11 22:29:00 -0300
categories: [Blog]
tags: [github, gitlab, github actions, ci/cd, sqlite, database, scraping, python, tutorial, SQL, git]
showtoc: true
summary: Discover a free alternative to cloud engines for data scraping on GitHub Actions!
cover:
    image: "https://ik.imagekit.io/devmedeiros/integration_hnYRmMYaY.webp"
    alt: "a skyline of a powerline symbolizing the seamly connection between github actions and sqlite"
    caption: "Photo by kiwi thompson on Unsplash"
---

Recently I’ve been working with a data scraping project that works with a small amount of data, small enough that free resources/tier from the most popular cloud engines are enough to allocate my data, but I don’t like having the risk of being billed over this personal project. To solve this I’ve been looking for a free alternative that I can share and that runs automatically with Github Actions.

If you want to check out the repo that contains the code discussed in this post, follow this [link](https://github.com/devmedeiros/template-sqlite-actions).

> I'll illustrate how to integrate SQLite Databases with Github Actions using Python, but if you know how to modify a file using another programming language this post is still relevant to you.

## Writing your Data Generator/Scrapper

First, your project needs to be on a repository, in my case, I’m using Github. I wrote a Python code that scrapes a webpage and saves the data to a SQLite database, on this example I’ll illustrate this with a much simpler code.

```python
# Import libraries
import pandas as pd
import random
from sqlalchemy import create_engine
from datetime import datetime

# Create random data
people = ['Ana', 'Bob', 'Charles', 'Daiana']
values = [random.random(), random.random(), random.random(), random.random()]

# Create db connection
engine = create_engine('sqlite:///data.db')

# Create the dataframe
data = pd.DataFrame({'people': people, 'values': values, 'load_date': datetime.now()})

# Save the data frame
data.to_sql('data', if_exists='append', con=engine, index=False)
```

## Setting the Workflow

If you run the above code multiple times on a local machine it’ll work, but you’ll notice that on Github it’ll not persist the changes, that is because you need to commit the changes. To do this you’ll need to create a workflow, on your repo create a yaml file on `.github/workflow`. This file is going to be your workflow, you can choose any name you want.

```yaml
# name your workflow
name: random data

# definy the frequency it'll run
on:
  schedule:
    - cron: "0 * * * *" # hourly
  workflow_dispatch:

env:
  ACTIONS_ALLOW_UNSECURE_COMMANDS: true

# create the jobs
jobs:
  generate-latest:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10' # setting python version to 3.10

      - name: Install requirements
        run: pip3 install -r requirements.txt # setting the environment

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

Don't forget to enable workflow permissions, on your repo go to `Settings > Actions > General`, and select **Read and write permissions**.

![workflow permissions from github official website](https://i.imgur.com/pOym60i.png#center)

## Conclusion

This can be a good free alternative in case you want to be able to share the data you are scraping or generating. But you still need to keep an eye on Github's limitations when using the free version. See the current usage limits on their [official website](https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration#usage-limits).

---

If you would like to see a real-like application of this you can go to [this repo](https://github.com/devmedeiros/nota-fiscal-goiana). Where I've implemented a monthly scrapper that saves the data to an SQLite database that is available to everyone.