# RDAMP-Sales-Analysis
This project is the first of four projects that I have been working on as part of the Realcare Tech MarK LTD mentorship program. I have decided to utilize my knowledge of Excel and Power BI as tools to create a report that answers foundational business questions. This project gives an overview of key sales performance trends for ACE, a nationwide retail chain, between 2023 - 2025.
This report will serve as a baseline to identify regional performance gaps, customer behaviour patterns, and product category profitability, helping the executive team of ACE focus on high-impact areas in subsequent strategy sessions.
# Dataset Provided
- Ace Superstore Retail Dataset.csv
- Columns: [Order ID], [Order Date], [Order Mode], [Customer ID],	[City], [Postal Code], [Country],	[Region],	[Product ID],	[Product Name],	[Category],	[Sub-Category],	[Sales],	[Cost Price],	[Quantity],	[Discount]
- Store Locations.xlsx
- Columns: [City],	[Postal Code],	[Country],	[Region]
# Tools utilized for data analysis
- Microsoft Excel
- Microsoft Power BI
# Exploratory Data Analysis
## Analysis using Excel
-	The datasets I am working with are Ace Superstore Retail and Store Locations.
-	I ensured all columns within each dataset were of the correct datatype.
- Columns Country and Region within the Ace Superstore Retail dataset had missing information in several cells. I used XLOOKUP in excel to fill in the missing information. The columns with similar names within the Store Locations dataset had complete information that was able to fill in the missing information.
-	I created two new columns because of the above step called Country-Adjusted and Region-Adjusted and removed the original columns Country and Region.
-	I checked both datasets for duplicate rows. None were present.
## Analysis using Power BI
-	I imported the new datasets to Power BI to conduct further EDA and cleaning.
-	I noticed that within my Adjusted Region column thaere were both "Yorkshire & the Humber" and "Yorkshire and the Humber". I used the Replace Value operation to replace the "&" with "and" so the datset had the correct Yorkshire and the Humber as a region.
-	For the Category column, I needed to separate the information into two separate columns, Category and Segment. The information within the original Category column was separerated by "-". I was able to create the two columns using Custom Column operation using the M-codes:
  if Text.Contains([Category], " - ") 
    then Text.BeforeDelimiter([Category], " - ") 
    else Text.BeforeDelimiter([Category], " ")
 	Named column Category1
 	&
 	if Text.Contains([Category], " - ") 
    then Text.AfterDelimiter([Category], " - ") 
    else Text.AfterDelimiter([Category], " ")
 	Named column Segment
- I removed error from the column named Category1.
- The segment column created had null values as a result of the initial Category column containing information without "-"
- Filled the null values in the Segment column by creating another custom column using the M-code:
  if [Segment] = null or Text.Trim([Segment]) = "" then [Category] else [Segment]
  Named new column Segment1
- Removed original Category column and the Segment column and renamed the Category1 column as Category and the Segment1 column as Segment.
-	Discount column had null values so I treating these as no discount and fill the null values with 0s.
-	I promoted the first row as header for the Store Locations dataset.
-	Renamed tables: Ace Superstore RetailFact and Store LocationsDim
-	Changed the Order Date datatype to Short Date format.
### Creation of Measure using Power BI
-	Created the following measures to aid in analysis prior to visualization:
  1. Gross Profit per Unit = 
AVERAGEX(
    FILTER('Ace Superstore RetailFact', 'Ace Superstore RetailFact'[Sales] > 0 && 'Ace Superstore RetailFact'[Cost Price] > 0),
    'Ace Superstore RetailFact'[Sales] - 'Ace Superstore RetailFact'[Cost Price]
)
  2. Profit Margin (%) = 
AVERAGEX(
    FILTER('Ace Superstore RetailFact', 'Ace Superstore RetailFact'[Sales] > 0 && 'Ace Superstore RetailFact'[Cost Price] > 0),
    DIVIDE('Ace Superstore RetailFact'[Sales] - 'Ace Superstore RetailFact'[Cost Price], 'Ace Superstore RetailFact'[Sales])
)
  3. Profit per Unit = Sum('Ace Superstore RetailFact'[Sales]) - Sum('Ace Superstore RetailFact'[Cost Price])
  4. Total Cost = 
SUMX(
    'Ace Superstore RetailFact', 
    IF('Ace Superstore RetailFact'[Cost Price] > 0, 'Ace Superstore RetailFact'[Cost Price] * 'Ace Superstore RetailFact'[Quantity], 0)
)
  5. Total Discount = AVERAGE('Ace Superstore RetailFact'[Discount])
  6. Total Revenue = 
SUMX(
    'Ace Superstore RetailFact', 
    IF('Ace Superstore RetailFact'[Sales] > 0, 'Ace Superstore RetailFact'[Sales] * 'Ace Superstore RetailFact'[Quantity], 0)
)
  7. Total Units = SUM('Ace Superstore RetailFact'[Quantity])
- As the Sales and Cost Price columns contained negative values, I created the above measures to manage these without the analysis being affected.

## ACE SUPERSTORE RETAIL PERFORMANCE & STRATEGIC INSIGHTS
Upon analyzing ACE Superstore Retail sales figures for the reporting period between 2023 – 2025 some key KPIs to highlight are as follows:
-	Total Revenue = £3.10M
-	Total Sales = £293.89K
-	Total Cost = £1.02M
-	Total Units Sold = 113K
-	Profit Margin (%) = 68%
## SALES
A summary of total sales, Revenue and discount by Region and Segment indicated the following:
Total Sales in relation to all 12 regions ranged from £47,906 (East Midlands), the highest, to £2,943 (North East), which is the lowest.
Total Sales in relation to product segment ranged from £34,729 (Outdoor), the highest, to £7 (Dressing), which is the lowest.
### The top 5 performing regions in Total Sales are:
-	East Midlands - £47,906
-	Yorkshire and the Humber - £40,909
-	Scotland - £35,036
-	London - £32,535
-	South East - £28,257
### The top 5 underperforming regions in Total Sales are:
-	West Midlands - £22,321
-	East of England - £15,927
-	Northern Ireland - £8,896
-	Wales - £3,854
-	North East - £2,943
 ### The top 5 performing segments in Total Sales are:
-	Outdoor - £34,729
-	Kitchen - £34,496
-	Home - £25,604
-	Electronics - £21,804
-	Fitness - £14,049
### The top 5 underperforming segments in Total Sales are:
-	Vegetarian - £20
-	Protein - £18
-	Spreads - £17
-	Salad Toppings - £10
-	Dressing - £7
## REVENUE
Total Revenue in relation to all 12 regions ranged from £510,544 (East Midlands), the highest, to £34,987 (Nort East), which is the lowest.
Total Revenue in relation to product segment ranged from £378,040 (Outdoor), the highest, to £55 (Dressing), which is the lowest.
### The top 5 performing regions in Total Revenue are:
-	East Midlands - £510,544
-	Yorkshire and the Humber - £423,635
-	Scotland - £379,396
-	London - £351,379
-	South West - £300,969
### The top 5 underperforming regions in Total Sales are:
-	West Midlands - £223,809
-	East of England - £158,604
-	Northern Ireland - £94,763
-	Wales - £37,979
-	North East - £34,987
### The top 5 performing segments in Total Revenue are:
-	Outdoor - £378,040
-	Kitchen - £359,589
-	Home- £263,826
-	Electronics – £210,649
-	Fitness - £151,464
### The top 5 underperforming segments in Total Revenue are:
-	Vegetarian - £175
-	Spreads - £106
-	Salad Toppings - £103
-	Protein - £90
-	Dressing - £55
## DISCOUNT
-	Average Discount in relation to all 12 regions range from 0.167 (North East), the highest to 0.149 (South West), which is the lowest.
-	Average Discount in relation to segment range from 0.200 (Baking & Cooking), the highest to 0.080 (Apps), which is the lowest.
## TOP SELLERS
A summary of top selling and underselling products indicated the following:
### The top 5 selling products by revenue are:
-	Portable Refrigerator Freezer - £51,380
-	Portable Solar Generator - £51,174
-	Electric Bike - £47,708
-	Compact Digital Camera - £33,252
-	Compact Dishwasher - £32,738
### The top 5 underselling products by revenue are:
-	Herb Seasond Rice - £18
-	Flavored Rice Cakes - £18
-	Canned Black Beans - £9
-	Baking Soda - £9
-	Cinnamon Raisin Bagels - £6


## PROFIT MARGIN
A summary of high and low profit margin product by category and sub-category indicated the following:
### The top 5 product categories with the highest profit margin:
-	Grooming – 70.4%
-	Storage – 70.3%
-	Baby – 70.1%
-	Wearable – 70.0%
-	Food – 69.1%
### The top 5 products categories with the least profit margin:
-	Crafts – 63.4%
-	Sports – 61.1%
-	Footwear – 60.7%
-	Furniture – 60.0%
-	Apps – 59.7%
### The top 5 products sub-categories with the highest profit margin:
-	Frozen Potatoe Products – 82.6%
-	Fruit Dips – 80.9%
-	Vinaigrettes – 80.5%
-	Oatmeal – 80.0%
-	Olives – 79.6%
### The top 5 product sub-categories with the least profit margin: 
-	Pre-Packaged Produce Kits – 57.8%
-	Gourmet Ice Cream – 57.7%
-	Dairy Desserts – 57.1%
-	Wraps and Flatbreads – 56.8%
-	Root Vegetables – 49.6%

## SALES CHANNEL
A summary of the sales, and revenue by type of order indicated the following:
Key KPIs: There are 2 order types
-	Online
-	In-Store

-	The total sales made online – 51.37%
-	The total revenue made online – 51.48%
-	The total sales made in-store – 48.63%
-	The total revenue made in-store – 48.52%
-	Over time, online revenue and in-store revenue grew when we compared figures in 2023 to 2024.
-	As the data contained information up to 31 March 2025 when compared to the same period in 2024, the data revealed 2024 recording higher revenue for both online and in-store channels than 2025.

## PRODUCT GROSS PROFIT
A summary of gross profit per unit across country, region, and city indicated the following:
Key KPIs:
-	Gross Profit per Unit: £18.29
-	Number of products: 1695
-	Gross Profit per Unit ranges from £540 (Electric Bike), the highest to £1 (Zucchini), which is the lowest.

# Dashboard images



