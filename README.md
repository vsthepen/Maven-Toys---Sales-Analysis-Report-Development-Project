## Maven-Toys---Sales-Analysis-Report-Development-Project

### Report Requirements
- Provide an analysis of Sales and Profit by Product Category.
- Identify and display the Top 10 Most Sold Products.
- Highlight products with less than £100,000 in sales over the past two years.
- Present the Percentage of Sales and Profit by Store Location.
- Include visuals that compare current year and previous year metrics for both product names and categories.

### Security & Technical Requirements
- Implement Row-Level Security (RLS) to ensure each store can only access their own sales data and KPIs when viewing the report.
- Enable personalization of visuals to allow users to customize the report according to their needs.
- Restrict data export from the report server to maintain data security.
- Conduct a performance analysis of the report to identify and resolve any issues, ensuring optimal performance.
- Configure an alert notification for when sales for the current year exceeds that of the previous year.
- Schedule a daily data refresh at 5:00 A.M.

### Additional Requirements
Prepare a comprehensive report that includes:
- An executive summary highlighting key insights and trends.
- Actionable recommendations based on the analysis to guide decision-making.

### Data Cleaning and Transformation
Before diving into data cleaning and transformation, my first step is always to review the raw data, particularly when working with **“.xls”** or **“.csv”** files. This allows me to understand the data structure and relationships between different columns.
The data for this project was stored in a folder containing five CSV files: **Calendar, Inventory, Products, Sales,** and **Stores**. I imported these files into Power Query using Power BI’s folder connector, which dynamically imports all files from a selected folder.

![image](https://github.com/user-attachments/assets/4406d0f5-b9cd-4a6d-88e1-9cfb523ca2c6)


### Transformation Steps
1.	Creating Queries:
I created individual queries for each table by right clicking the “Content” column, selecting Binary > Add as New Query, and repeating the process for all five tables. (I am still exploring more efficient methods using M code.) 
2.	Optimizing Queries:
After creating the queries, I renamed them and disabled the Enable Load option in the original query to optimize model performance.
Attach Image
3.	Ensuring Data Quality:
I enabled the Column Quality feature to quickly identify and address errors in the data.
4.	Cleaning Columns:
I used delimiters to split columns containing mixed text and numbers.
5.	Removing Irrelevant Columns:
Unnecessary columns were removed from each query to reduce model size and improve performance.
6.	Fixing Locale Issues:
While transforming the Date table, I encountered an error when changing the column data type to “Date.” This was resolved by right-clicking the column header, selecting Change Type > Using Locale, and choosing the appropriate locale for the date format.
Attach Image
Once the transformation was complete, I clicked Close and Apply to load the data into Power BI.

Modeling
In the Model View, I reviewed the automatic relationships created by Power BI between the tables to ensure they aligned with the data structure.
Attach Image

Maintaining Data Integrity
•	Date Table: Marked the date table as the official “Date Table” to ensure accurate time-based analysis.
•	Data Categories: Assigned appropriate data categories (e.g., Store City) to prevent misinterpretation.
•	Nullable Columns: Disabled the Nullable option for key columns (primary and foreign keys) to enforce data integrity and enhance performance.

Measure Table
To organize and streamline the analysis, I created a dedicated measure table containing key metrics such as:
•	Total Sales
•	Total Profit
•	Total Sales Year-to-Date
•	Previous Year Sales
•	Previous Month Sales
•	Total Quantity Sold
•	Number of Transactions
While crafting these measures, I ensured DAX queries were optimized for performance. For example:
•	PREVIOUSMONTH() was used instead of DATEADD() for better efficiency in the calculation context.
•	SAMEPERIODLASTYEAR() was preferred over DATEADD() for straightforward year-over-year comparisons.
Additionally, I created a Date Hierarchy in the date table to enable drill-down functionality during analysis.
Attach Image

Data Exploration and Insights
I delved into the data to uncover meaningful insights. While the polished final report may appear seamless and user-friendly, the analysis process often involves extensive exploration and iteration.
Attach Image 1
Attach Image 2

Report Development
Based on stakeholder requirements, I built a report that effectively tells a story through various visual elements:
•	Visuals Used:
o	KPI Cards
o	Clustered Column Chart
o	Donut Chart
o	Combo Chart (Line and Clustered Column)
o	Table
•	Additional Features:
o	Designed custom SVG backgrounds in PowerPoint and imported them into Power BI.
o	Added page navigation and bookmarks to switch between visuals and configure buttons (e.g., “Show Menu”).
o	Edited visual interactions to ensure slicers target relevant visuals, adding context for end-users.
o	Hid visual headers for a cleaner, more professional look.
Attach Image 1
Attach Image 2: Full Report

Technical Requirements
1.	Data Export Restrictions:
Configured the report to prevent data export from the report server.
2.	End-User Personalization:
Allowed users to personalize visuals in the report.
(Settings: File > Options and Settings > Report Settings)
3.	Performance Analysis:
Ran the Performance Analyzer to identify and fix potential bottlenecks. The maximum report render time was ~1.6 seconds (~1648ms), well within the ideal threshold of 2 seconds.
Attach Image
4.	Row-Level Security (RLS):
To ensure each store could only access its data, I:
o	Created a unique identifier by combining store name and location, resolving cases where stores shared the same name within a city.
o	Configured 35 roles by applying filters to the Store dimension table.
o	Tested roles to confirm proper functionality before publishing.
Attach Image
5.	Scheduled Data Refresh:
Installed and configured a gateway to connect Power BI Service to the source folder. Scheduled daily refreshes at 5 a.m.
Attach Image
6.	Alerts for Key Metrics:
Set up alerts for the Sales Head to notify them when current-year sales exceed the previous year. This was done via Power BI Service by selecting the relevant KPI visual, clicking Set Alert, and configuring the details.
Attach Image

Finally, I prepared a PowerPoint presentation for our upcoming meeting with the sales team. Each slide was carefully designed to convey insights clearly and effectively, using concise interpretations of the data. To enhance understanding, I incorporated annotations and visual elements to emphasize key points and ensure the information was communicated seamlessly.
Attach images


