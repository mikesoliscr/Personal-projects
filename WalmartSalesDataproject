-- The major aim of this project is to gain insight into the sales data of Walmart 
-- to understand the different factors that 
-- affect sales of the different branches.

-- CREATE DATABASE
create database if not exists salesDataWalmart;

-- CREATE TABLE
create table if not exists sales(
invoice_id VARCHAR(30) not null primary key,
branch VARCHAR(5) not null,
city VARCHAR(30) NOT NULL,
customer_type varchar(30) not null,
gender varchar(10) not null,
product_line varchar(100) not null,
unit_price DECIMAL(10,2) NOT NULL,
quantity INT NOT NULL,
tax_pct FLOAT(6,4) NOT NULL,
total DECIMAL(12, 4) NOT NULL,
date DATETIME NOT NULL,
time TIME NOT NULL,
payment VARCHAR(15) NOT NULL,
cogs DECIMAL(10,2) NOT NULL,
gross_margin_pct FLOAT(11, 9),
gross_income DECIMAL(12, 4),
rating FLOAT(2, 1)
);




-- -----------------------------------------------------------------------------------------------------------------
-- ------------------------Feature engineering----------------------------------------------------------------------
-- Feature Engineering: This will help use generate some new columns from existing ones.
-- DATA CLEANING
-- Ad the time_of_day column

select time,
(CASE
WHEN `time` between "00:00:00" and "12:00:00" then "Morning"
WHEN `time` between "12:01:00" and "16:00:00" then "Afternoon"
ELSE "Evening"
END
) as time_of_day
from sales;

alter table sales add column time_of_day varchar(20);

-- For this to work turn off safe mode for update
-- Edit > Preferences > SQL Edito > scroll down and toggle safe mode
-- Reconnect to MySQL: Query > Reconnect to server


UPDATE sales
set time_of_day = (
case
WHEN `time` between "00:00:00" and "12:00:00" then "Morning"
WHEN `time` between "12:01:00" and "16:00:00" then "Afternoon"
ELSE "Evening"
end
);

-- day_name

select date,
DAYNAME(date) as day_name
from sales;

alter table sales add column day_name varchar(10);

update sales
set day_name = DAYNAME(date);

-- month_name monthname is a function to extract the name of the months as well in day name

select date,
monthname(date)
from sales;

alter table sales add column month_name varchar(10);

-- to add the info into the table
update sales
set month_name = monthname(date);

-- ------------------------------------------------------------------------------------------------------
-- Exploratory Data Analysis (EDA): Exploratory data analysis is done to answer the listed questions 
-- and aims of this project.
-- ---------------------------------------------------------------------------------------------------
-- ----------------------------Generic Questions------------------------------------------------------
-- How many unique cities does the data have?
select distinct city from sales;

-- In which city is each branch?
select distinct city, branch from sales;

-- ----------------------Product questions----------------------------------------------------------------
-- How many unique product lines does the data have?
select distinct product_line from sales;

-- What is the most common payment method?
select payment, count(payment) as cnt from sales group by payment 
order by cnt asc;

-- What is the most selling product line?
select product_line, count(product_line) as cnt from sales group by product_line order by cnt desc;

-- What is the total revenue by month?
select month_name as month,
sum(total) as total_revenue
from sales
group by month
order by total_revenue desc;

-- What month had the largest COGS?
select month_name as month,
sum(cogs) as cogs
from sales
group by month_name
order by cogs desc;

-- What product line had the largest revenue?
select product_line,
SUM(total) AS total_revenue
from sales
group by product_line
order by total_revenue desc;

-- What is the city with the largest revenue?
select city, branch,
sum(total) as total_revenue
from sales
group by city, branch
order by total_revenue desc;

-- What product line had the largest VAT?
select product_line,
avg(tax_pct) as avg_tax
from sales
group by product_line
order by avg_tax desc;

-- Which branch sold more products than average product sold?
select branch,
sum(quantity) as qty
from sales
group by branch
having sum(quantity) > (select avg(quantity) from sales);

-- What is the most common product line by gender?
select gender,
product_line,
count(gender) as total_cnt
from sales
group by gender, product_line
order by total_cnt desc;

-- What is the average rating of each product line?
select 
round(avg(rating),2) as avg_rating,
product_line
from sales
group by product_line
order by avg_rating desc;

-- -------------------------------------------------------------------------
-- ----------------------sales----------------------------------
-- Number of sales made in each time of the day per weekday
select * from sales;
select time_of_day,
COUNT(*) as total_sales
from sales
where day_name = "Tuesday"
group by time_of_day
order by total_sales desc;

-- Which of the customer types brings the most revenue?
select customer_type,
sum(total) as total_rev
from sales
group by customer_type
order by total_rev desc;

-- Which city has the largest tax percent/ VAT (Value Added Tax)?
select CITY,
AVG(TAX_PCT) AS TAX_PCT
FROM SALES
GROUP BY CITY
ORDER BY TAX_PCT DESC;

-- Which customer type pays the most in VAT?
select
customer_type,
AVG(TAX_PCT) AS TAX_PCT
from sales
GROUP BY CUSTOMER_TYPE
ORDER BY TAX_PCT DESC;

-- ------------------------------------------------------------------------
-- ----------------------------Customer------------------------------------------

-- How many unique customer types does the data have?
select 
distinct customer_type
from sales;

-- How many unique payment methods does the data have?
select 
distinct payment
from sales;

-- What is the most common customer type?
select customer_type,
count(*) as count
from sales
group by customer_type
order by count;

-- What is the gender of most of the customers?
select
gender,
count(*) as gender_cnt
from sales
group by gender
order by gender_cnt desc;

-- What is the gender distribution per branch?
select gender,
count(*) as gender_cnt
from sales
where branch = "C"
group by gender
order by gender_cnt;

-- Which time of the day do customers give most ratings?
select time_of_day,
avg(rating) as avg_rating
from sales
group by time_of_day
order by avg_rating desc;

-- Which time of the day do customers give most ratings per branch?
select time_of_day, branch,
avg(rating) as avg_rating
from sales
group by time_of_day
order by avg_rating desc;

-- Which time of the day do customers give most ratings per branch?
select time_of_day, branch,
avg(rating) as avg_rating
from sales
where branch = "C"
group by time_of_day
order by avg_rating desc;

-- Which day fo the week has the best avg ratings?
select day_name,
avg(rating) as avg_rating
from sales
group by day_name
order by avg_rating desc;

-- Which day of the week has the best average ratings per branch?
select day_name,
avg(rating) as avg_rating
from sales
where branch = "C"
group by day_name
order by avg_rating desc;

-- --------------------------------Sales------------------------
-- ------------------------------------------------------------------

-- Number of sales made in each time of the day per weekday
select time_of_day, day_name,
count(*) as total_sales
from sales
where day_name = "Monday"
group by time_of_day
order by total_sales;

-- Which of the customer types brings the most revenue?
select customer_type,
sum(total) as total_revenue
from sales
group by customer_type
order by total_revenue;

-- -- Which city has the largest tax/VAT percent?
select city,
round(avg(tax_pct), 2) as avg_tax_pct
from sales
group by city
order by avg_tax_pct;

-- Which customer type pays the most in VAT?
select customer_type,
avg(tax_pct) as total_tax
from sales
group by customer_type
order by total_tax;
-- ------------------------------
-- ---------------------------------------
