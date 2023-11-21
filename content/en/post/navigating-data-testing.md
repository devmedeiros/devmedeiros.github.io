---
title: "Navigating Data Testing"
date: 2023-11-20 18:45:00 -0300
categories: [Blog]
tags: [data testing, database, great expectations, assertr, analytics engineer, data engineering, SQL]
showtoc: true
summary: What is Data Testing and why you should care about it?
cover:
    image: "https://ik.imagekit.io/devmedeiros/navigating-data-testing_Ka1fdcGdv.webp"
    alt: "a sailboat sailing with a sunset in the background"
    caption: "Photo by [Bobby Stevenson](https://unsplash.com/pt-br/@bobbystevenson?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/pt-br/fotografias/veleiro-branco-no-mar-durante-o-por-do-sol-3AEJ6imbQTo?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)"
  
---

## Introduction to Data Testing

Data testing comes from software testing in computer science, when it is tested various parts of the code to assess if it's fit for use. In this sense, when testing your data you want to evaluate if the data is **accurate**, **completed**, and **consistent**.

So you want to make sure what you are extracting and sending is what you expect, making sure you are not losing data by changing data formats and transforming the data. When moving your data from point A to point B, you want to guarantee that the data is the same and has the same information.

One way to do this is by using common database structures, setting data types, and defining table constraints. But if this is not enough you can set more complex rules by using other tools, such as [**Great Expectations**](https://greatexpectations.io/), and [**assertr**](https://docs.ropensci.org/assertr/).

## Common Database Structure

When creating or altering a table on a database, you need to specify the column's data types and you can set multiple constraints. These data types determine how the values look, so imagine you have a table listing employees' basic info, like their names, salary, and date of birth.

| Name | Salary    | Date of Birth |
|------|-----------|---------------|
| Ana  | 3000.00   | 1988-02-02    |
| Bob  | 4000.00   | 1970-04-12    |
| Carl | 2000.00   | 1999-07-05    |

The salary column would be of type numeric, double digits because that is how we format currency. The date of birth would be of a "date" type format and the name a simple string. These data types are a kind of constraint, if you create a table with these data types you cannot insert a new row with different data types.

But you can also add more constraints, you could set a column to never accept null/empty values, or set a column to be composed of unique values.

Setting data types and constraints is vital to maintaining data integrity because whenever someone or something (like an automatic process) tries to add or alter a piece of information on a table that has constraints it will raise an error. These errors help users avoid typos or inserting invalid options that may break other applications.

Imagine you have a system that sends a happy birthday present to an employee automatically, if that employee doesn't have a date of birth registered they'll never get their happy birthday present. So making it impossible to leave the birthday empty helps users pay attention use the database correctly and keep everything working as intended.

### Limitations

When we usually think about data unit testing, or testing your data to make sure that nothing is out of the ordinary, is easy to imagine how relying only on data constraints is a good idea. It covers most use cases that we normally use, but if we stop to think about it, we can find more uses that we don't even know we need.

In our employee example table, sure setting the salary to be a numeric data type with decimals usually stops a lot of potential mistakes, but it doesn't stop people from having a negative salary or even an absurd salary amount. Sure you can set how big a number can be, but it only defines the digits, so if your maximal salary is 10000, you will have to set it to be 9,999 or 99,999.

Of course, it helps, but it doesn't fully solve the problem.

Another example of a limitation would be complex rules, for instance, if the employees have seniority levels like I, II, and III, and these come with salary range, you can't have a custom rule saying _"from level I the salary range is 1000 to 2000"_.

## Beyond Constraints

If you need to set more custom and/or business-specific data validation rules you can use [**Great Expectations**](https://greatexpectations.io/), and [**assertr**](https://docs.ropensci.org/assertr/). Both are great for defining rules and validation parameters to test your data.

You can write tests setting min and max values, check unique values, check if the values are within a given set and many more options.

In conclusion, navigating data testing involves a careful balance between relying on conventional database structures and using specialized tools like **Great Expectations** and **assertr**. While constraints play a crucial role in maintaining data integrity, recognizing their limitations highlights the need for more nuanced and business-specific validation rules. By embracing these tools, data engineers and analysts can ensure not only the accuracy and completeness of their data but also navigate the complexities of real-world scenarios.