---
layout: post
title: Rockbuster - Movie Rental Analysis
subtitle: Each post also has a subtitle
categories: [database, SQL, exploratory data analysis]
tags: [database, PostgreSQL, SQL, exploratory data analysis, EDA, visualization]
---

<p align="center">
 <img src="/assets/images/banners/rockbuster/rockbuster.png" width="500">
</p>


### **Overview - Business problem** 
Rockbuster Stealth LLC is a movie rental company that used to have stores around the world. Facing stiff competition from streaming services such as Netflix and Amazon Prime, the Rockbuster Stealth management team is planning to use its existing movie licenses to launch an online video rental service in order to stay competitive.

<p style="margin-bottom:-30px"></p>

### **Goal**
This analysis is intended to answer a series of business key questions and derive data-driven answers for company strategy in 2020.
<ol style="margin-top:-20px;">
<li>Which movies contributed the most/least to revenue gain?</li>
<li>What was the average rental duration for all videos?</li>
<li>Which countries are Rockbuster customers based in?</li>
<li>Where are customers with a high lifetime value based?</li>
</ol>

Essentially, this project aims to learn relational database management systems (RDBMS) including data structure, SQL query, data filtering and cleaning, joining tables, common table expressions (CTE) as well as visualization using Tableau.  

<p style="margin-bottom:-30px"></p>

### **Data**
Rockbuster company and data being used are all fictious. The dataset containing information about Rockbuster’s film inventory, customers, and payments, among other things is provided by Careerfoundry and can be found [here](https://www.postgresqltutorial.com/wp-content/uploads/2019/05/dvdrental.zip).

<p style="margin-bottom:-30px"></p>

### **Process**
This is SQL-based project using PostgreSQL database to analyse and essentially to answer business questions of a movie rental company. I began by drafting data dictionary that can be found [here](https://github.com/nuki-susanti/Rockbuster-Analysis/blob/main/Data_Dictionary.pdf).

<p align="center">
  <img src="/assets/images/banners/rockbuster/erd.png" width="900">
  <em>Entity Relationship Diagram (ERD)</em>
</p>

<p align="center">
  <img src="/assets/images/banners/rockbuster/data_dictionary.png">
  <em>Sample of data dictionary</em>
</p>

### Exploratory Data Analysis (EDA)

<p style="margin-bottom:-30px"></p>

#### Rockbuster overview

<p align="center">
  <img src="/assets/images/banners/rockbuster/overview_1.png">
</p>

<p align="center">
  <img src="/assets/images/banners/rockbuster/overview_table_1.png">
</p>

<p align="center">
  <img src="/assets/images/banners/rockbuster/overview_2.png">
</p>

<p align="center">
  <img src="/assets/images/banners/rockbuster/overview_table_2.png">
</p>

**For the sake of visualization, the obtained data from query is presented below.**

<p align="center">
  <img src="/assets/images/banners/rockbuster/overview.png" width="400">
</p>

#### Movies and Revenue by rating and genre 

<p align="center">
  <img src="/assets/images/banners/rockbuster/movies_revenue_by_rating.png">
</p>

<p align="center">
  <img src="/assets/images/banners/rockbuster/movies_by_rating.png" width="300">
</p>

<p align="center">
  <img src="/assets/images/banners/rockbuster/revenue_by_rating.png" width="300">
</p>

<p align="center">
  <img src="/assets/images/banners/rockbuster/revenue_by_genre_sql.png">
</p>

<p align="center">
  <img src="/assets/images/banners/rockbuster/revenue_by_genre.png" width="900">
</p>

> Genre categories:
>> PG-13 - Parents Strongly Cautioned, Some Material May Be Inappropriate for Children Under 13. <br/>
>> NC-17 - No One 17 and Under Admitted. <br/>
>> PG - Parental Guidance Suggested, Some Material May Not Be Suitable for Children. <br/>
>> R - Restricted, Children Under 17 Require Accompanying Parent or Adult Guardian. <br/>
>> G - General Audiences, All Ages Admitted. <br/>

#### Most/least profitable movies

<p align="center">
  <img src="/assets/images/banners/rockbuster/revenue_by_title.png">
</p>

The extracted data from above query is presented below.

<p align="center">
  <img src="/assets/images/banners/rockbuster/most_profitable_movie.png" width="300">
</p>

<p align="center">
  <img src="/assets/images/banners/rockbuster/least_profitable_movie.png" width="300">
</p>

#### Average rental duration

As discovered previously, the average rental duration obtained from SQL query is 4.985 days or 5 days.

#### Revenue by country

<p align="center">
  <img src="/assets/images/banners/rockbuster/revenue_by_country.png">
</p>

<p align="center">
  <img src="/assets/images/banners/rockbuster/global_market.png" width="600">
</p>

<p align="center">
  <img src="/assets/images/banners/rockbuster/sales_figure.png" width="600">
</p>

#### Top customers

<p align="center">
  <img src="/assets/images/banners/rockbuster/top_customer_sql.png">
</p>

<p align="center">
  <img src="/assets/images/banners/rockbuster/top_customer.png" width="800">
</p>

### Conclusion

<ul>
  <li style="font-weight: bold">Popularity</li>
    <ul>
      <li>Sports, Sci-Fi, Animation, Drama and Comedy are top 5 popular genres.</li>
      <li>PG-13 rating is the most popular movie segment.</li>
    </ul>
    <li style="font-weight: bold">Global market & sales figure</li>
    <ul>
      <li>India, China and US are the biggest market.</li>
      <li>Philippines has the highest rental order / customer, while Russia has the lowest.</li>
    </ul>
    <li style="font-weight: bold">Movies revenue</li>
    <ul>
      <li>Fast-moving rented movies (short-rental duration) are more profitable.</li>
    </ul>
</ul>

### Recommendations

<ul>
  <li>Top 5 movies by genre having revenue above $4,000: Sports, Sci-Fi, Animation, Drama and Comedy. Rockbuster should indeed invest more in those genres. However, these genres don‘t seem to relate to each other, thus, market (customer rental preference) by age groups shall be captured to specifically fulfill each group preferences.</li>
  <li>Movies having PG-13 rating are indeed the most profitable one. However, market for NC-17, PG and R movies seem to be promising as they share similar revenue of around $12,000, which indeed could be further exploited.</li>
  <li>Consider to take a closer look on the fast-moving rented movies (ex: 3 days but rented very often).</li>
  <li>Consider to add movies time variability and stay up to date as all movies are from the year of 2006.</li>
  <li>India, China and US are the biggest market. Those are dense populated countries, which can be commercially exploited. Rockbuster shall invest in the marketing campaign in the big population countries. The age groups data mentioned previously could help identify the market target in each respective country.</li>
  <li>Large population is indeed beneficial. However, repeated order from the same customer should be taken into account such as the case with Philippines, regardless its less population, it has the highest rental order / customer compared to others. For this purpose, such loyalty program for the top 10 customer shall be considered.</li>
</ul>

### **Limitations**

Rockbuster and dataset used are fictious, therefore, this project is customized according to the learning goals.

Find the code for this analysis [Github](https://github.com/nuki-susanti/Rockbuster-Analysis)\
Find visualization in [Tableau](https://public.tableau.com/app/profile/nuki.susanti/viz/RockbusterStealthDataAnalysis_16618096649900/RockbusterAnalysis?publish=yes)\
Connect with me! [Linkedin](https://www.linkedin.com/in/nuki-susanti-915594151/)