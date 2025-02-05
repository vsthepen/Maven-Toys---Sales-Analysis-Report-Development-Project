## Maven-Toys---Sales-Analysis-Report-Development-Project

### Overview

Maven Toys, a toy company with 35 stores across various cities and locations, has provided two years of data (2022 and 2023) for analysis. The objective is to assess their performance, derive insights and build a report aligned with the specified requirements below, supporting data-driven decision-making to achieve their business goals.

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

## STEPS

### Data Cleaning and Transformation
Before diving into data cleaning and transformation, my first step is always to review the raw data, particularly when working with *“.xls”* or *“.csv”* files. This allows me to understand the data structure and relationships between different columns.
The data for this project was stored in a folder containing five CSV files: *Calendar, Inventory, Products, Sales,* and *Stores*. I imported these files into Power Query using Power BI’s folder connector, which dynamically imports all files from a selected folder.

![image](https://github.com/user-attachments/assets/4406d0f5-b9cd-4a6d-88e1-9cfb523ca2c6)

### Transformation Steps
1.	**Creating Queries:**
I created individual queries for each table by right clicking *Binary > Add as New Query*, and repeating the process for all five tables. (I am still exploring more efficient methods using M code.) 
2.	**Optimizing Queries:**
After creating the queries, I renamed them and disabled the *Enable Load* option in the original query to optimize model performance.

![image](https://github.com/user-attachments/assets/194ee0e4-e67b-4915-913c-d4be3936ab65)

3.	**Ensuring Data Quality:**
I enabled the Column Quality feature to quickly identify and address errors in the data.
5.	**Cleaning Columns:**
I used delimiters to split columns containing mixed text and numbers.
6.	**Removing Irrelevant Columns:**
Unnecessary columns were removed from each query to reduce model size and improve performance.
7.	**Replace Values:**
The Unit price and cost columns included numbers combined with currency symbols. I used the *Replace Values* option to remove the currency symbols, leaving only the numeric values. Afterward, I updated the datatype to ensure the columns were correctly formatted.
8.	**Fixing Locale Issues:**
While transforming the Date table, I encountered an error when changing the column data type to *Date*.This was resolved by right-clicking the column header, selecting *Change Type > Using Locale*, and choosing the appropriate locale for the date format.

![image](https://github.com/user-attachments/assets/2369503d-4dea-458f-a4f6-cff9f6015151)

Once the transformation was complete, I clicked *"Close and Apply"* to load the data into Power BI.

### Modeling
In the Model View, I reviewed the automatic relationships created by Power BI between the tables to ensure they aligned with the data structure. The relationships between the fact and dimension tables resulted in a **snowflake schema**.

![image](https://github.com/user-attachments/assets/563b75f9-acc0-46b6-9ed1-4b883bb29e9c)

**Maintaining Data Integrity**

- **Date Table:** Marked the date table as the *"official Date Table"* to ensure accurate time-based analysis.
- **Data Categories**: Assigned appropriate data categories (e.g. Store City) to prevent misinterpretation.
- **Nullable Columns**: Disabled the Nullable option for key columns (primary and foreign keys) to enforce data integrity and enhance performance.

**Measure Table**

For organization purposes, I created a dedicated measure table containing key metrics as shown below:

![image](https://github.com/user-attachments/assets/4146391e-c2b4-494b-abbf-c87fccc0f7d4)

While crafting these measures, I ensured DAX queries were optimized for performance. For example:

- *PREVIOUSMONTH()* was used instead of *DATEADD()* for better efficiency in the calculation context.
- *SAMEPERIODLASTYEAR()* was preferred over *DATEADD()* for straightforward year-over-year comparisons.
  
Additionally, I created a Date Hierarchy in the date table to enable drill-down functionality during analysis.

![image](https://github.com/user-attachments/assets/d03b7b06-122c-4fec-a4ce-ce08c9214643)

### Data Exploration and Insights

I delved into the data to uncover meaningful insights. While the polished final report may appear seamless and user-friendly, the analysis process often involves extensive exploration.

*Image showing part of the data exploration process*  ![Data Exploration](https://github.com/user-attachments/assets/b06cbbdf-d86b-4ad6-991e-f61acb880845)

### Report Development

Based on stakeholder requirements, I built a report that effectively tells a story through various visual elements:

•	Visuals Used:
- KPI Cards
- Clustered Column Chart
- Donut Chart
- Combo Chart (Line and Clustered Column)
- Table

#### Additional Features:

- Designed custom SVG backgrounds in PowerPoint and imported them into Power BI.
- Added page navigation and bookmarks to switch between visuals and configure buttons (e.g., “Hide Menu”).
- Edited visual interactions to ensure slicers target relevant visuals, adding context for end-users.
- Hid visual headers for a cleaner, more professional look.
  
![image](https://github.com/user-attachments/assets/91fd328f-1479-402e-bc65-6b26eba7ad97)


*Full Report Overview* ![Full Report Overview] ![image](https://github.com/user-attachments/assets/5ae17813-38ef-485c-b4d2-b736ae64958e)




### Technical Requirements

**1.	Data Export Restrictions:**
   
Configured the report to prevent data export from the report server.

(Settings: File > Options and Settings > Report Settings)

![image](https://github.com/user-attachments/assets/d41cd0ed-fa39-484e-8457-973d00c0a7bb)


**2.	End-User Personalization:**
   
Allowed users to personalize visuals in the report.

(Settings: File > Options and Settings > Report Settings)

**3.	Performance Analysis:**
   
Ran the Performance Analyzer to identify and fix potential bottlenecks. The maximum report render time was ~1.6 seconds (~1648ms), well within the ideal threshold of 2 seconds.

Before performance analyzer was run, I cleared the visual cache to ensure accurate execution time was captured. When visual cache is not cleared, Power BI retrieves result directly from memory rather than executing the full DAX query or refreshing data from the source which can give misleading low execution times.

![image](https://github.com/user-attachments/assets/19866680-a3e3-4b02-9e39-327464006073)

**4.	Row-Level Security (RLS):**
   
To ensure each store could only access its data, I:

- Created a unique identifier by combining store name and location, resolving cases where stores shared the same name within a city.
- Configured 35 roles by applying filters to the Store dimension table.
- Tested roles to confirm proper functionality before publishing.

![image](https://github.com/user-attachments/assets/88cb423b-4c1d-445f-8492-b0b632991fc9)

![image](https://github.com/user-attachments/assets/74b9b3a2-dc70-4917-b78b-6df18e19b2be)

**5.	Scheduled Data Refresh:**
    
Installed and configured a gateway to connect Power BI Service to the source folder. Scheduled daily refreshes at 5 a.m.

![image](https://github.com/user-attachments/assets/d102cbe1-1688-4e8a-b86f-4e6d6279435d)

![image](https://github.com/user-attachments/assets/461bfdc3-47e1-48af-be29-acaebc07528a)

**6.	Alerts for Key Metrics:**

Set up alerts for the Sales Head to notify them when current-year sales exceed the previous year. This was done via Power BI Service by selecting the relevant KPI visual, clicking Set Alert, and configuring the details.

![image](https://github.com/user-attachments/assets/1a7f6e74-ad4a-4bbc-9167-3ed254e4ec6c)

Finally, I prepared a PowerPoint presentation for our upcoming meeting with the sales team. Each slide was carefully designed to convey insights clearly and effectively, using concise interpretations of the data. To enhance understanding, I incorporated annotations and visual elements to emphasize key points and ensure the information was communicated seamlessly.

*Here's a few slides from the presentation:*

![Slide2](https://github.com/user-attachments/assets/1c1a5fd8-217c-4c57-af44-3398e0c13930)

![Slide3](https://github.com/user-attachments/assets/8439b1ca-71ae-4eb2-a3bc-7e50ce629a86)

![Slide5](https://github.com/user-attachments/assets/b2a16235-48ab-4521-8504-4ebad75f739d)

