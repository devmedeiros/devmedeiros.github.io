---
title: UX/UI for Business Intelligence Dashboards
date: 2022-04-29 11:14:00 -0300
categories: [Blog]
tags: [UX, dashboard, powerbi, data visualization, business intelligence, storytelling, figma]
showtoc: true
---

# What is UX/UI?

UX is the acronym for User Experience, it is a recent concept about making design decisions while thinking about the user experience. The UX designer needs to be concerned about whether their product is easy to use and intuitive, making changes to it whenever necessary to suit the user's needs.

UI stands for User Interface. It is everything involved in the interaction of the user and the product. The UI designer is responsible for developing interfaces, not only limited to the visual aspects but also ensuring that the interfaces are functional and usable and generally contribute to a good user experience.

# How to Improve the Experience of BI Dashboards?

Many people see BI dashboards as web pages. This comes with some expectations. For example, most sites that have some navigation system use a top menu with buttons, a side menu (most common in Brazil being on the left, but in some countries, it is on the right), or a hamburger menu (the one we click on it and the options appear).

{{< figure src="https://ik.imagekit.io/devmedeiros/site_layout_C0BWt0-Rh.png?ik-sdk-version=javascript-1.4.3&updatedAt=1651180441785" caption="Jaqueline Medeiros - All rights reserved" alt="Image with three website layouts, the first with a top navigation bar, the second with a left-side menu and the third with a sandwich menu">}}

With this, a majority of people who use the dashboards expect to find navigation buttons and data segmentation (filter) in these places, in addition to other information such as logo and title.

## Data Segmentation

Also commonly called a data filter, it is a fundamental part of several dashboards, its positioning needs to be defined carefully, because if it is in a place that the user does not expect, it can prevent your dashboard from being used efficiently, in addition to maintaining a visual standard for everyone its filters help people more easily recognize what is and isn't a filter.

{{< figure src="https://ik.imagekit.io/devmedeiros/base_7_vbzsb9L.png?ik-sdk-version=javascript-1.4.3&updatedAt=1651183332760" alt="Image with six filters, distributed in two columns, each one with three. One filter is for a city, the other is for a restaurant and the last one is for reservations? The only difference between them is the design, on the left they are consistent and on the right, each one has a unique design.">}}

You can and should use and experiment with different themes in your projects. What matters, when making it easier for users, is consistency, pick a model for your filters with the desired colors and all the graphic specifications that are of interest to you and use it in all filters, as this will help people quickly find it and recognize that it's a filter.

![Three filters with, city, restaurant and accept reservation? Where the city shows an eraser next to the title and the text Clear selections](https://ik.imagekit.io/devmedeiros/borracha_Xwi_TYTHB.png?ik-sdk-version=javascript-1.4.3&updatedAt=1651183773246#center )

It's common for people when they are reading something on the computer to position the mouse pointer where they are reading. For Power BI, this makes it easier for the user to find the clear data segmentation button because when hovering the mouse over the filter name, a rubber appears on the left side where the filter name is. If you use Power BI, you probably already knew that, but we can improve this by using something that the user already knows, which is the **rubber**, and create a button using it as an icon and make this button clear all filters at the same time. If you want to know how to make this button [here](https://community.powerbi.com/t5/Desktop/Clear-All-Slicers-by-one-button-in-power-bi-desktop/m-p/494518), in the Power BI forum, explains how.

This feature needs may be unnoticed at first. Regularly dashboard users don't realize which filters they have used or they use so many that they just want to be able to clear the selection faster and more efficiently, so this button makes the process easier.

## Page Navigation

Power BI's native page navigation is not intuitive for most people and you may want to direct the navigation in a specific flow that facilitates understanding and contributes to the intended storytelling. In this case, we have the option to hide all the report tabs, except for the opening/home page. But what's the best way to direct the user to the other pages? Suppose your report is simple, you have an overview tab and another tab with a breakdown, a simple button would solve your problem, but if your report is extensive it may be unfeasible to put a button for each tab on all pages.

{{< figure src="https://ik.imagekit.io/devmedeiros/infografico_kQJe_yhQD.png?ik-sdk-version=javascript-1.4.3&updatedAt=1651241421864" caption="Jaqueline Medeiros - All rights reserved" alt= "A circle with four rectangles around it, a line connects the rectangles to the circle" width="100%">}}

In this case, it might be interesting to consider having a home page that leads to all the other pages and placing a `home` or `back` button on them.

# How to Improve Dashboards Interface?

## Prototyping Programs

Using a specific program to prototype your dashboard allows for greater artistic freedom compared to what Business Intelligence applications typically provide. Figma is a great tool for this, you can create advanced backgrounds and prototypes with amazing quality to use in your BI dashboards.

Here's an example of a dashboard I made a few months ago:

{{< figure src="https://raw.githubusercontent.com/devmedeiros/Alura-Challenge-BI-2/main/Alura%20Food/alura%20food%20resume.png" title="Panel with data" alt="In the picture you can read the text: Overview of Indian Restaurants Market Dev_Medeiros City Restaurant Accepts Reservations? 19.21 % Restaurants with Online Delivery Average Rating 3.72 R$ 39.48 Average Price per Person 9577 Total Restaurants 5 Most Popular Cities Restaurants 5 Most Popular Cuisine">}}

The background of this panel was done completely in Figma, even some of the titles of the BI visuals.

{{< figure src="https://raw.githubusercontent.com/devmedeiros/Alura-Challenge-BI-2/main/Alura%20Food/Alura%20Food.png" title="Background made in Figma" alt=" In the image you can read the text: Overview of Indian Restaurants Market Dev_Medeiros City Restaurant Accepts Reservations? % of Restaurants with Online Delivery Average Rating Average Price per Person Total Restaurants 5 Cities with the most Restaurants 5 Most Popular Cuisine">}}

You can check out more Power BI Dashboards I made for Alura Challenge: [Alura Films](/post/2022-02-17-alura-films-powerbi), [Alura Skimo](/post/2022-03-08-alura-skimo-powerbi), and [Alura Food](/post/2022-02-26-alura-food-powerbi).

## Figures and Icons

{{< figure src="https://img.freepik.com/vetores-gratis/employees-dando-as-maos-e-ajudando-os-colegas-a-cliir-as-escadas_74855-5236.jpg?w =1380&t=st=1651239796~exp=1651240396~hmac=1b2ab902322bbf8ce26e3bc83f602e69075fcc2e18f0afdc91a328c7942ba746" caption="Vector created by pch.vector - br.freepik.com" align="center">}}

Figures and icons when used correctly help make the panel stand out, and make it more eye-catching and beautiful. There are several ways to get images, if you or your team can't create them yourself there is the option of using online platforms that provide vectorized images. On these platforms, there are free options, which require some type of attribution, and premium (paid) options, which often do not need to attribute the author and have a higher quality.

{{< figure src="https://img.icons8.com/dusk/344/cash-register.png" caption="ICONS8 icon" alt="Cash register icon" align="center">}}
