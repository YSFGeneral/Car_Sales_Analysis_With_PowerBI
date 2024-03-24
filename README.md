# Car_Sales_Analysis_With_PowerBI

![](autosales.png)
___

## Background

This company is a car dealership that sells various car models. To effectively track and analyze their sales performance, they need a comprehensive Car Sales Dashboard in Power BI. 
**Objective**: The objective of this project is to design and develop a dynamic and interactive Car Sales Dashboard using Power BI. The dashboard will visualize critical KPIs related to car sales, helping understand sales performance over time and make data-driven decisions.

### About Dataset

There is one .xlsx File in this dataset. This Dataset contains 16 Features/Columns covering Twenty-three thousand, nine hundred and seven (23,907) records. This dataset consists of various features.

## Requirements

**Problem Statement 1: KPI’s Requirement**

The dashboard should provide real-time insights into key performance indicators (KPIs) related to our sales data. This will enable us to make informed decisions, monitor our progress, and identify trends and opportunities for growth.
**1.	Sales Overview:**
- Year-to-Date (YTD) Total Sales
- Month-to-Date (MTD) Total Sales
- Year-over-Year (YOY) Growth in Total Sales
- Difference between YTD Sales and Previous Year-to-Date (PTYD) Sales
**2.	Average Price Analysis:**
- YTD Average Price
- MTD Average Price
-	YOY Growth in Average Price
-	Difference between YTD Average Price and PTYD Average Price
**3.	Cars Sold Metrics:**
-	YTD Cars Sold
-	MTD Cars Sold
- YOY Growth in Cars Sold
-	Difference between YTD Cars Sold and PTYD Cars Sold

**Problem Statement 2: Charts Requirement**

-	**YTD Sales Weekly Trend:** Display a line chart illustrating the weekly trend of YTD sales. The X-axis should represent weeks, and the Y-axis should show the total sales amount.
-	**YTD Total Sales by Body Style:** Visualize the distribution of YTD total sales across different car body styles using a Pie chart.
-	**YTD Total Sales by Color:** Present the contribution of various car colors to the YTD total sales through a pie chart.
-	**YTD Cars Sold by Dealer Region:** Showcase the YTD sales data based on different dealer regions using a map chart to visualize the sales distribution geographically.
-	**Company-Wise Sales Trend in Grid Form:** Provide a tabular grid that displays the sales trend for each company. The grid should showcase the company name along with their YTD sales figures.
-	**Details Grid Showing All Car Sales Information:** Create a detailed grid that presents all relevant information for each car sale, including car model, body style, colour, sales amount, dealer region, date, etc

### Skills

Microsoft Power BI is used throughout the project
-	Data Cleaning and Processing
-	Data Manipulation
-	Data Analysis Expression (DAX)
-	Data Modeling
-	Visualization

### Data Cleaning/Processing/Manipulation

I performed some data cleaning and also data manipulation with Replace value to change the misspellings

### Data Analysis Expression (DAX)

Used DAX to create a new table named Calendar Table. The Calendar Table contain all the dates from the start to the end (including holidays) i.e. it contains from minimum to maximum date. It gives the ability to perform different date operations like YTD, YTDP, YTM operations . Calendar Table help to perform all the date function.
- Date = CALENDAR(MIN(car_data[Date]),MAX(car_data[Date]))
- Year = YEAR('Calendar Table'[Date])
- Month = FORMAT('Calendar Table'[Date],"MMMM")

### Data Modelling

There are two tables which are the Car_data table and the Calendar Table created. The relationship is created from the Calendar Table to the Car_data table through Date column. The Date column is a primary key on the Calendar Table and foreign key on the Car_data table. The relationship is a one-to-many relationship.
 
### Data Visualization

I used Time Diligence DAX functions to find the following:
- YTD Total Sales - I created a New Measure named Year To Date (YTD) Total Sales using 
- YTD Total Sales = TOTALYTD(SUM(car_data[Price ($)]), 'Calendar Table'[Date]) and I visualize it.
_Hint: TotalYTD function is used to automatically detect the latest date in the data_
- PYTD Total Sales – I created Previous Year To Date Total Sales as required in the business statements using
- PYTD Total Sales = CALCULATE(SUM(car_data[Price ($)]), SAMEPERIODLASTYEAR('Calendar Table'[Date]))
- Sales Difference – I created Sales Difference by subtracting the Previous Year To Date Total Sales from Year To Date Total Sales using
- Sales Difference = [YTD Total Sales] - [PYTD Total Sales]
- I created a conditional formatting to show different color for the sales different using
- Sales Diff Color = IF([Sales Difference]>0, "Green", "Red")
and did the color formatting
- YoY Sales Growth – I created a new measure for Year over Year Growth and use the same formatting as Sales Difference so that positive should be Green and negative should be Red
- MTD Total Sales – I created Month To Date Total Sales to show the total sales made monthly and formatted it to indicate the MTD Total Sales on the dashboard.
- MTD Total Sales = TOTALMTD(SUM(car_data[Price ($)]), 'Calendar Table'[Date])
- And then formatted it using;
_MTD KPI = CONCATENATE("MTD Total Sales : ", FORMAT([MTD Total Sales] / 1000000, "$0.00M"))_

Average Price – Created a new measure to calculate Average price which will help in calculating YTD Average Price 
Avg Price = SUM(car_data[Price ($)]) / COUNT(car_data[Car_id])
YTD Avg Price – Calculated Year To Date Average Price using
YTD Avg Price = TOTALYTD([Avg Price], 'Calendar Table'[Date])
PYTD Avg Price – Created measure to calculate Previous Year to Date Average Price using
PYTD Avg Price = CALCULATE([Avg Price], SAMEPERIODLASTYEAR('Calendar Table'[Date]))
Avg Price Difference – Calculated the difference in Average Price between the previous year and current year and format the color to be dynamic.
Avg Price Difference = [YTD Avg Price] - [PYTD Avg Price]
YoY Avg Price Growth – Calculated the Year on Year (Year over Year) Average Price Growth
YoY Avg Price Growth = [Avg Price Difference] / [PYTD Avg Price]
MTD Avg Price – Created Month To Date Average Price using
MTD Avg Price = TOTALMTD([Avg Price], 'Calendar Table'[Date])
Formatted it using 
MTD Average Price KPI = CONCATENATE("MTD Avg Price : ", FORMAT([MTD Avg Price] / 1000, "$0.00K"))

YTD Cars Sold – YTD Cars Sold = TOTALYTD(COUNT(car_data[Car_id]), 'Calendar Table'[Date])
PYTD Cars Sold – 
PYTD Cars Sold = CALCULATE(COUNT(car_data[Car_id]), SAMEPERIODLASTYEAR('Calendar Table'[Date]))
Cars Sold Diff = [YTD Cars Sold] - [PYTD Cars Sold]
Cars Sold Color = IF([Cars Sold Diff]>0, "Green", "Red") to format the color
YoY Car Sold Growth – 
YoY Car Sales Growth = [Cars Sold Diff] / [YTD Avg Price]
MTD Cars Sold – Calculated the Month to Date Cars Sold
MTD Cars Sold = TOTALMTD(COUNT(car_data[Car_id]), 'Calendar Table'[Date].[Date])
Formatted it using; 
MTD Cars Sold KPI = CONCATENATE("MTD Cars Sold : ", FORMAT([MTD Cars Sold] / 1000, "$0.00K"))

Total Sales = SUM(car_data[Price ($)])
Max Point = IF(MAXX(ALLSELECTED('Calendar Table'[Week]), [Total Sales]) = [Total Sales], MAXX(ALLSELECTED('Calendar Table'[Week]), [Total Sales]), BLANK())
Created an interactive and insightful dashboard as requested which has the overview page and details page.

# Car Sales Dashboard Overview
 ![](Dashboard_Overview.png)
 
# Car Sales Dashboard Details
![](Dashboard_Details.png)
