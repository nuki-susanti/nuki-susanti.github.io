---
layout: post
title: Road Crash Analysis
subtitle: Each post also has a subtitle
categories: markdown
tags: [test]
---

## **Overview - Business Problem**
According to WHO, approximately 1.3 million people die each year as a result of road traffic crashes. Between 20 and 50 million more people suffer non-fatal injuries, with many incurring a disability as a result of their injury. In addition, road traffic crashes cost most countries 3% of their gross domestic product. This worldwide-phenomenon has been quite alarming as private transportation ownership increases, resulting in volume rise of vehicles on the road as well as traffic accidents. It is therefore critical to analyze and derive insights on what factors that could potentially influence the road accident events. These insights could be used for road accidents prevention measures.

## **Goal**
The objective of this road safety analysis to help analyze the influential factors causing traffic accidents, locate accident hot-spots and perform classification supervised machine learning to predict accident severity. Python and Tableau are used for data visualization and analysis presentation. Personally, as a professional in HSE area (Health Safety Environment), I selected this analysis subject intentionally to add values and insights to my current knowledge.

## **Data**
* Open source road crash and casualties data from 2021 in South Australia collected by Department for Infrastructure and Transport. Merged and cleaned data results in 21,242 cases of traffic accidents. It can be found [here](https://data.sa.gov.au/data/dataset/road-crash-data/resource/1057e9ae-4672-4123-9c1d-1877483da401?inner_span=True).
* [Data information and dictionary](https://data.sa.gov.au/data/dataset/road-crash-data/resource/02fb14f9-8dcb-4a59-863c-5f7cc3ae1832)
* [Geojson of South Australian suburb boundaries](https://data.sa.gov.au/data/dataset/suburb-boundaries)

## **Process**
This analysis, therefore, could be divided into two parts:
1. Part 1: Data mining
2. Part 2: Data modelling

Data mining methodology called CRISP-DM(Cross Industry Standard Process for Data Mining, without deployment) is used to during this analysis process.

![Fig.1 Applied methodology: CRISP-DM](/assets/images/banners/data_mining_steps.png)

1. Business understanding
2. Data understanding\
There are 3 datasets for this analysis: ‘crash’, ‘unit’ and ‘casualty’. I started this process by reading the data dictionary provided and comparing with the content of the datasets. Data quality and integrity check were then performed for each dataset. Each step is documented as it necessary for the data cleaning.
3. Data preparation\
Based on the previous checks, data cleaning was performed which include removing duplicates, handling missing values, error typing of records and outliers. Finally, datasets were merged. Documentation of performed steps were included in the Jupyter notebook, such as below:

![Fig. 2 Sample of data preparation documentation](/assets/images/banners/data_cleaning.PNG)

4. Mining the data\
Here the data exploratory began. For number of accidents and their severity, I divided the EDA into several categories:
- General accident distribution
- Time-based analysis: hourly, daily and monthly
- People-based analysis: sex, age, alcohol and drugs involvement
- Weather-based analysis
- Spatial analysis


### General accident distribution (How severe are accidents are on the road?)





**Here is some bold text**

## Here is a secondary heading

Here's a useless table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


How about a yummy crepe?

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg)

It can also be centered!

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .center-block :}

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.
