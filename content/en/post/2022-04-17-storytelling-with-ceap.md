---
title: Storytelling with CEAP
date: 2022-04-17 10:08:00 -0300
categories: [Blog]
tags: [storytelling, data science, data visualization, data analysis]
showtoc: true
summary: The storytelling of how much each deputy/senator spends in Brazil
cover:
    image: https://i.imgur.com/u57LTSR.jpg
    alt: brazil nation congress
    caption: Image of Renato Laky by Pixabay
---

## The Dataset

CEAP stands for _Cota para o Exercício da Atividade Parlamentar_, in English, Exercise of Parliamentary Activity from Brazil. The quota is a monthly amount that can be used by the deputy/senator to defray typical expenses of the exercise of the parliamentary mandate.

The monthly amounts don't accumulate over the months. Another important thing is Senators in Brazil have an 8-year mandate, and there are elections every 4 years.

The main dataset is provided by the Brazilian government through the transparency portal.[^1] I gathered additional data to link the quota for each senator and federal unit.[^2][^3]

## Data Analysis

### Total Amount Refunded by Year

![Bar plot with blue bars and green bars indicating election years, shows the total amount refunded by senators in Brazil](https://raw.githubusercontent.com/devmedeiros/7DaysOfCode/main/img/refunded_year.jpg#center)

On the image above we can see how much was refunded by year. The year 2022 is the smallest because is not over yet. The data was collected on April 16, 2022.

There seems to be a trend to spend slightly less money on election years.

The first three years were the three lowest spending years, **11.52M**, **11.61M**, and **10.64M** from 2008 to 2010, respectively. This was probably because in the following years the expenses that could be refunded increased in 2011.

### Top Highest and Lowest Average Refunded Percentages by Senators

These percentages represent the quota percentage a given senator refunded on average.

![Horizontal bar plot with the 10 highest refundees, on average, senators](https://raw.githubusercontent.com/devmedeiros/7DaysOfCode/main/img/senator_refund_highest_mean.jpg#center)

In the image above, it's seen highest refunded senators note that they’re all closed together, the highest refundee, on average, is João Capiberibe, using **99.78%** of his quota. On the lowest side (image below), it's presented the lowest use of the refund quota, on average. Nailde Panta, is the lowest refundee, on average, using just **4.1%** of her quota.

![Horizontal bar plot with the 10 lowest refundees, on average, senators](https://raw.githubusercontent.com/devmedeiros/7DaysOfCode/main/img/senator_refund_lowest_mean.jpg#center)

### Expense Types Over the Years

Here we can see that in the years 2008 to 2010 there wasn’t any refund by transport and private security services.

![Bar plot of expenses type refunded by the years 2008 to 2022](https://raw.githubusercontent.com/devmedeiros/7DaysOfCode/main/img/expense_type_year.jpg#center)

In 2008, the main expenses were publication of parliamentary activity (over **R$ 7,000.00**) and locomotion/accommodation (almost **R$ 6,500.00**). In the subsequent years, those expenses dropped in half, for the publications and to less than **R$ 1,000.00** for locomotion.

In 2013 and 2014 the expense of private security was the highest ever with over **R$ 4,000.00**. Probably due to protests in those years.

## To learn more

To learn how The Chamber of Deputies from Brazil works click [here](https://www2.camara.leg.br/english).

This is presentation is for learning purposes, from a challenge proposed by Alura #7DaysOfCode. In case you want to know what else can be done with the data, check the [Serenata de Amor](https://serenata.ai/en/). It's a Brazilian project that uses AI to track refunded requests by deputies.

See my code at my GitHub [repo](https://github.com/devmedeiros/7DaysOfCode), there you can also find a slide presentation, learn about how the data was cleaned, and know more about the next task from the challenge.

### References

[^1]: https://www12.senado.leg.br/transparencia/dados-abertos-transparencia/dados-abertos-ceaps
[^2]: https://pt.wikipedia.org/wiki/Lista_de_senadores_do_Brasil#Nova_Rep%C3%BAblica_(1987%E2%80%932023)
[^3]: https://www2.camara.leg.br/legin/int/atomes/2009/atodamesa-43-21-maio-2009-588364-publicacaooriginal-112820-cd-mesa.html
