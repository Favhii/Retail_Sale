# Retail Sale Analysis

## Table of Contents

- [Project Overiew](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Data Cleaning](#data-cleaning)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Insights](#insights)
- [Recommendation](#recommendation)

## Project Overiew

This project contains a dataset containing information about a retail sales business. The goal is to perform an exploratory data analysis (EDA) to uncover patterns, trends, insights that can help the retail sales business make informed decisions. 

## Data Source

The dataset used in this project was provided by the company Oasis Infobyte.

## Tools

-	EXCEL: The dataset was gotten as a CSV, so I used Excel to open it. Go through the dataset to know the information it contains.
-	SQL: The dataset was loaded to SQL for cleaning and querying.
-	Power BI: This tool was used for visualization.

## Data Cleaning

-	Confirming the total normal of dataset and columns.
-	Making sure they all represent their various data type.
-	Checking for duplicates and null values.

## Exploratory Data Analysis

- Descriptive Statistics: Finding the mean, median, mode, standard deviation of various data.
- Time Series Analysis: Analyze sales trends over time using time series techniques.
- Customer and Product Analysis: Analyze customer demographics and purchasing behavior.

## Data Analysis

``` SQL
SELECT * FROM retail_sales_dataset;

-- Data Quality
-- row count
select count(*)
from retail_sales_dataset;

-- column count
select count(column_name) 
from information_schema.columns
where table_name = 'retail_sales_dataset';

-- data type
select column_name, data_type from information_schema.columns
where table_name = 'retail_sales_dataset';

select date,
str_to_date(date, '%m/%d/%Y')
from retail_sales_dataset;

update retail_sales_dataset
set date = str_to_date(date, '%m/%d/%Y');

select date
from retail_sales_dataset;

-- check for duplicates
with duplicate_cte as
(
select
row_number() over 
(partition by Transaction_ID, date, Customer_ID, 
gender, age, Product_Category, Quantity, Price_per_Unit, Total_Amount) as row_num
from retail_sales_dataset
)

select *
from duplicate_cte
where row_num >1;

-- now analysis

-- average age (mean), Standard deviation age
select round(avg(age)), round(stddev(age), 2)
from retail_sales_dataset;

-- average price per unit
select round(avg(Price_per_Unit), 2)
from retail_sales_dataset;

-- most frequent price per unit 
select Price_per_Unit, count(*) as total_count
from retail_sales_dataset
group by 1
order by 2 desc;

-- standard deviation of price per unit
 select round(stddev(Price_per_Unit), 2)
from retail_sales_dataset;

 -- percentage of people that can afford products
select
count(*) as number_of_people,
round((
count(*) / (select count(*) from retail_sales_dataset)
 ) * 100, 2) as pct_number_of_people
 from retail_sales_dataset
 where Price_per_Unit > 179;
 

-- which product sells the most? 
select distinct product_category, count(*) as total_count
from retail_sales_dataset
group by 1
order by 2 desc;

-- which gender influneces product the most 
select product_category, gender, count(*) as total_count
from retail_sales_dataset
group by 1, 2
order by 3 desc;

-- which gender patronizes the most?
select gender, count(total_amount)
from retail_sales_dataset
group by 1
order by 2 desc;

-- relationship bwtween age, spending and product preference
select distinct Product_Category, 
case
    when age >30 then 'adult'
    when age <= 30 then 'youth'
    end as Age_group,
count(*) as total_count
from retail_sales_dataset
group by age_group, Product_Category
order by Product_Category, total_count desc;

-- age group
select  
case
    when age >30 then 'adult'
    when age <= 30 then 'youth'
    end as Age_group,
count(*) as total_count
from retail_sales_dataset
group by age_group;

-- how does customer age and gender influence their purchasing behavior
select gender, 
case
    when age >30 then 'adult'
    when age <= 30 then 'youth'
    end as Age_group,
count(*) as total_count
from retail_sales_dataset
group by age_group, gender
order by total_count desc;

-- products and total number of times purchased (this shows people's product preference)
select distinct Product_Category, count(quantity)
from retail_sales_dataset
group by 1
order by 2 desc;

select Customer_ID, max(Quantity), max(Total_Amount)
from retail_sales_dataset
where Product_Category = 'electronics'
group by 1
order by 3 desc;
-- limit 10;

select Product_Category, max(Price_per_Unit), min(Price_per_Unit)
from retail_sales_dataset
group by 1;
```


## Insights

1. General Insights
   - Average price is 179. However, the percentage of customers who cannot afford the product is significantly higher than those who can afford the product.
   - The price distribution of product is high, but may be dependent on the items in the various product.
   - Clothing is the most purchased product with a total of 351 purchase.
2. Customer and Prodduct Insight
   - Adult patronize the most in all product.
   - The female gender buys more product from the store than the male gender. 
   - The male gender leads in all products except Beauty, where the female dominates. Although the margin is narrow between males and females in Electronics and Clothing, it’s significantly wider in favour of females in Beauty products.
3. Sales Insight
   - The sales trend primarily covers the year 2023, but with a partial inclusion of 2024 (only January). However, I considered the 2024 data insignificant for the sales analysis. 
   - The monthly revenue declined in March, June, July, September, November.
   - The store experienced its lowest monthly revenue in November with a 36% decline and its highest monthly revenue in October with a 97% increase.

## Recommendation

1. General
   - Except for luxury items targeting affluent customers, prices should be moderately set to facilitate easier sales and revenue growth. 
   - Given the high demand for clothing products, I recommend ensuring a consistent stock supply in-store, focusing on (key) attributes that drive customer purchases, such as design, quality, etc.
2. Customer and Prodduct Insight
   - Given that the general population is largely youth-dominated, it’s logical to assume that young folks will be more inclined to the products than adults. Therefore, I recommend a targeted marketing campaign to capture the youth’s interest, unless our product’s target audience is specifically adults.
3. Sales Insight
   - There should be follow up on marketing every month to ensure profitability. Leverage channels like social media, word of mouth, and targeted online strategies to drive growth.
   - Giving discounts to the least performing product to attract more sales.
   - Prioritize customer follow ups: Gather feedback on each product purchased, ensure customer satisfaction, encourage referrals and recommendations.




