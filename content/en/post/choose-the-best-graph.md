---
title: Making Better Graphs
date: 2022-11-15 08:01:00 -0300
categories: [Blog]
tags: [data visualization, business intelligence, storytelling, review, ethic]
summary: Choosing the best graph or plot can be a hard task, but it doesn't need to be.
cover:
    image: "https://ik.imagekit.io/devmedeiros/graph-choosing_T2RxECo-q.webp"
    alt: "a donut graph showing 'yes' as a majority while it represents 10%, and 'no' as a minority while it represents 90%"
    caption: "piechart from WTF Visualizations"
    hidden: true
---

Learning how to make better visualizations come with study and practice, today I want to talk about sources you can use to guide you into making better graphs and other data visualizations in general.

## From Data to Viz

![infograph showing different possibilities for choosing a plot](https://i.imgur.com/416qeWj.png)

[From Data to Viz](https://www.data-to-viz.com/) brings a lot of tools to help navigate the data visualization world. In the website `Explore` tab you are faced with an interactive infographic that helps you pick a plot, you can choose between: `numeric`, `categoric`, `numeric&categoric`, `maps`, `network`, and `time series`. But it doesn't only show plots that would be good for your type of data, it also shows a description explaining the plot and common mistakes people make when using that specific plot.

At the end of each branch from the infographic, there is also a `story` link, where you can read about an implementation of that type of plot using real-world data.

And in the case, you are just starting and don't know many terms or are unsure if your data contains `too many` or `few` points when you rest your mouse over the options nodes you'll be able to see a description with an example table representing what that means. 

By heading to the `Caveats` tab you're able to see a collection of caveats, you can filter them by:

 - **Improvement:** all caveats that will suggest some sort of improvement on plots.

 - **Misleading:** some plots may be misleading by omitting some information, may it be sample size or how proportionally different two categories are. By reading this you'll be able to spot bad practices that lead to confusion or manipulation.

This website also provides links to **Python** and **R** galleries where you can get code snippets to reproduce these plots.

If you prefer to learn by looking at what not to do, you should check out [WTF Visualizations](https://viz.wtf/). It showcases bad visualizations that don't make sense or are somehow misleading.

There you'll find several pie plots that don't add up to 100%, area graphs where the numbers don't match the area, and just straight-up manipulation like the example below.

{{< figure src="https://64.media.tumblr.com/d8bcf748d564125312aeba4001b96df5/tumblr_qzg4n3nfJY1sgh0voo1_640.jpg" caption="piechart from [WTF Visualizations](https://viz.wtf/)" align="center" alt="a donut graph showing 'yes' as a majority while it represents 10%, and 'no' as a minority while it represents 90%">}}