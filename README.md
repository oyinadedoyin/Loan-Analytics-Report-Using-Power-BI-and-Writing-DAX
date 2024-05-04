# School Library Loan Analytics: Leveraging DAX for Enhanced Data Management, Reporting, and Visualization

![janko-ferlic-sfL_QOnmy00-unsplash](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/d2080578-2f16-4e6f-962f-5aee18a469e3)

### Table of Contents
---
- [Overview](#overview)
- [Report Structure](#report-structure)
- [Key Features](#key-features)
- [ETL Process](#etl-process)
- [Create Date Dimension table (dimDate) using DAX](#create-date-dimension-table-dimdate-using-dax)
- [Add Measures to Report using DAX](#add-measures-to-report-using-dax)
- [Using DAX to Create a Column in the Loans Table: Calculate Total Days on Loan](#using-dax-to-create-a-column-in-the-loans-table-calculate-total-days-on-loan)
- [Implementing Filters: Enhancing Data Exploration and User Experience](#implementing-filters-enhancing-data-exploration-and-user-experience)
- [Introducing a Drillthrough Page to Enhancing Report Navigation](#introducing-a-drillthrough-page-to-enhancing-report-navigation)
- [Setting up Loan Reminder Email Column with DAX](#setting-up-loan-reminder-email-column-with-dax)


### Overview
---
This project offers comprehensive insights into school library loan analytics, enabling users to manipulate data views and drill down to specific details through interactive reporting leveraging Microsoft Power BI and DAX functionalities.

### Report Structure
---
The report comprises five distinct pages:
1. **Landing Page**: Provides an overview and navigation to other sections.
2. **Filters**: Enables users to filter information across various screens.
3. **Current Loans Dashboard**: Offers a snapshot of organizational loan status, including overdue loans and customizable sliders for filtering results.
4. **Students Review**: Analyzes loan trends year-over-year, and by students, comparing total loans taken out.
5. **Details (Hidden Page)**: Allows drill-through functionality to search through to specific loan details.

  ![pbi_all_pages](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/005caaad-36c8-44c4-8062-4977b277ce2c)

### Key Features
---
- **Data Source Integration**: This report seamlessly integrates data from Dataverse, sourced from our Loans Manager Model-Driven App, providing real-time insights linked directly to our report.
- **Interactive Filters**: The Filters page empowers users to tailor data views according to specific criteria, enhancing the relevance of information displayed on other screens.
- **Current Loans Dashboard**: Visualizes organizational loan status, highlighting overdue loans and offering customizable sliders for refining results based on the number of days overdue.
- **Students Review**: Compares loan statistics year-over-year, providing valuable insights into loan trends and student borrowing behavior.
- **Drill-Through Functionality**: The hidden Details page allows users to drill down into specific loan details for in-depth analysis.
- **Loan Reminder Email**: A simple calculated column facilitates the generation of loan reminder emails, streamlining communication with students about overdue loans.
- **Model-Driven App Integration**: Users can seamlessly navigate to the corresponding records in the Loans Manager Model-Driven App to make necessary changes or updates.
- **Date Dimension Table Creation**: Implemented a Date Dimension table using DAX to enrich time-based analysis and reporting, enhancing insights into loan trends over different time periods.
- **Star Schema Data Modeling**: Created a star schema data model to establish table relationships between student, loans, dimension date tables. Utilizing DAX, this schema ensures efficient data management and optimized query performance for enhanced reporting capabilities. All relationships are one-to-many, ensuring accurate data aggregation and analysis.

![pbi_students_review](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/a3bedce1-086f-48e9-8e56-9ca0bccec4b8)

### ETL Process
---
- **Data Extraction**: Utilizes Microsoft Power BI to extract data from Dataverse, importing tables for students and loans.
- **Data Transformation**: Implements Power Query for data transformations, including data type changes, column removal, value replacements, and creation of new columns.
- **Data Linkage**: Establishes relationships between tables, enabling seamless navigation and integration of related data, such as book names and authors.

**Data Transformation in Power Query
---
![pbi_dataTransformation_PowerQuery](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/ccb4a545-f1cb-4e77-9c2e-6effb8ec8542)

### Create Date Dimension table (dimDate) using DAX
---
By creating the Date Dimension table, we can effortlessly create custom calendars tailored to our specific needs, such as the Academic Year calendar crucial for this project's context. More generally, it facilitates streamlined filtering of organization-based attributes like week numbers and half-years, enhancing the precision and efficiency of our analyses. One of the table's most significant advantages lies in its ability to support historical comparisons and trend analyses. Through its comprehensive temporal framework, we can accurately calculate metrics such as sales figures from the previous year and percentage changes year on year, empowering us with invaluable insights into business performance and trends.

To format the code block for GitHub, you can use triple backticks (```) to enclose the code and specify the language after the opening triple backticks. 

```DAX
dimDate = 
    ADDCOLUMNS(
        CALENDARAUTO(),
        "MonthID", MONTH([Date]),
        "Month", FORMAT([Date], "MMM"),
        "DayNumber", DAY([Date]),
        "DayName", FORMAT([Date], "dddd"),
        "DayID", WEEKDAY([Date], 2),
        "Year", YEAR([Date]),
        "Today", TODAY(),
        // Add a SchoolYear column that adjusts the year based on the month
        "SchoolYear", IF(
            MONTH([Date]) >= 9,
            YEAR([Date])+1,
            YEAR([Date])
        ),
        "SchoolMonthID", IF(
            MONTH([Date]) >= 9,
            MONTH([Date])-9,
            MONTH([Date])+3
        ),
        "Report Refresh", NOW() // We'll use this to display the report refresh date
    )
```

This DAX code is used to create two calculated columns: "SchoolYear" and "SchoolMonthID", based on a given date column `[Date]`.

1. **"SchoolYear" Calculation:**
   - The `IF` function is used to conditionally evaluate the month of the `[Date]` column.
   - If the month of the date is greater than or equal to September (which corresponds to the start of a typical school year), it adds 1 to the year of the date. This indicates that the school year is considered to start in the next calendar year.
   - If the month of the date is before September, it takes the year of the date as is, indicating that the school year aligns with the current calendar year.
- For months from September to December (inclusive), the year is incremented by 1, indicating the start of a new academic year. This aligns with the UK's academic calendar where the academic year typically begins in September.

2. **"SchoolMonthID" Calculation:**
   - Similarly, the `IF` function is used to conditionally evaluate the month of the `[Date]` column.
- The "SchoolMonthID" column assigns a numerical identifier to each month within the school year.
- - If the month of the date is greater than or equal to September, it subtracts 9 from the month to get the school month ID. This adjustment shifts September to month 0, October to month 1, and so on.
- For months from September to May (inclusive), the month's value is adjusted to represent the months of the school year starting from September (0) to May (8).
- For months from June to August, which are part of the summer break, these months are represented by values starting from June (9) through to August (11), effectively extending the school year into these months.


![pbi_dimDate_table2](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/fd65bf97-5b7c-42a9-95a9-768cadddaaa6)


By using these calculated columns, this reporting can easily align with the academic calendar of the school in the UK, making it possible to track and analyze academic data or events in a manner that reflects the timing and structure of the UK academic year, from September through to August.

### Data Modeling
---
Created a star schema data model to establish table relationships between student, loans, dimension date tables. Utilizing DAX, this schema ensures efficient data management and optimized query performance for enhanced reporting capabilities. All relationships are one-to-many, ensuring accurate data aggregation and analysis.

![pbi_data_modelling](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/9c373804-89a5-4d55-937b-c433c10f7f66)

### Add Measures to Report using DAX
---
To enhance data analysis and visualization capabilities, measures were incorporated into the report using DAX (Data Analysis Expressions). These measures aggregate data from various columns and perform calculations across multiple tables, enabling comprehensive insights into key metrics such as total loans last year.

Measures created within the report's visualizations include:

- **Total overdue loans**
- **Total days on loan**
- **Percentage change year on year**
- **Dynamic report header** (a text-based measure)

While measures can be created within existing tables in the report, such as the students table, a separate table named 'My Measures' was created for ease of management. This approach simplifies measure creation and organization. To accomplish this, an empty table named 'My Measures' was generated using the 'Enter Data' option from the Home tab, providing a centralized location for managing and accessing all measures within the report.

To create the first measure - Total loans, right click on the my measures table table and select new measure.
```
Total Loans = COUNTROWS(Loans)
```
**Total Overdue Loans**
```
Total Overdue Loans = CALCULATE([Total Loans], Loans[pu_isoverdue] = true() )
```
**Total Loans Last Year (LY)**
```
Total Loans LY = Calculate ([Total Loans], SAMEPERIODLASTYEAR(dimDate[Date]))
```
**Change Year on Year (YOY)**
```
Change YOY = [Total Loans] - [Total Loans LY]
```
**Percentage Change Year on Year**
```
% Change YOY = Divide (Change YOY], [Total Loans LY])
```
**Total Days on Loan**
```
Total Days on Loan = sum(Loans[Total Days on Loan]
```
**Report Header**
```
Report Header 1 = "Drillthrough -" & IF(HASONEVALUE(Student[Full Name]), VALUES(Student[Full Name]),"")

```
**Measures**
---
![pbi_measure_datapane](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/ee84cfb1-0cb9-4ff7-843f-fba1705aed11)

![pbi_measures](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/4422ce87-68a3-460b-8d2c-2dacc9da4cbc)


### Using DAX to Create a Column in the Loans Table: Calculate Total Days on Loan
---
In this section, DAX was employed to create a new column within the Loans table, specifically aimed at calculating the total number of days each loan remained outstanding. The formula utilized for this purpose is as follows:

```DAX
Total Days on Loan = IF(
    ISBLANK(Loans[Date Returned]),
    TODAY() - Loans[Date Loan Start],
    Loans[Date Returned] - Loans[Date Loan Start]
)
```

This DAX expression incorporates conditional logic to determine the total days on loan for each entry in the Loans table. If the 'Date Returned' column is blank (indicating that the loan is still outstanding), the formula calculates the difference between the current date (TODAY()) and the 'Date Loan Start' column, representing the total days the loan has been active.

Conversely, if the 'Date Returned' column contains a date value (indicating that the loan has been returned), the formula calculates the difference between the 'Date Returned' and 'Date Loan Start' columns, representing the total duration of the loan.

By leveraging DAX to create this calculated column, users can efficiently track and analyze the duration of loans within the Loans table, facilitating comprehensive loan management and analysis.


### Implementing Filters: Enhancing Data Exploration and User Experience
---
Incorporating filters into the report provides numerous benefits aimed at optimizing data exploration and improving user experience. By adding filters, users can effectively narrow down the dataset, allowing for more focused analysis and interrogation of specific data subsets. This not only streamlines the analytical process but also ensures that users only view the data relevant to their needs, minimizing cognitive load and enhancing overall comprehension.

**Key benefits of adding filters include:**

- **Dataset Size Reduction:** Filters enable users to refine the dataset, eliminating unnecessary data points and reducing the overall size of the dataset. This streamlined approach enhances data processing efficiency and expedites analytical workflows.

- **Facilitated Data Interrogation:** Filters empower users to interactively explore the data, enabling them to drill down into specific data subsets and uncover actionable insights. This dynamic interrogation capability fosters a deeper understanding of the data and facilitates informed decision-making.

- **Tailored Data Visibility:** By applying filters, users can tailor the visibility of data to meet their specific requirements, ensuring they only see the data relevant to their analysis. This customization enhances user satisfaction and promotes efficient data utilization.

To implement filters effectively, I used slicers and synchronized them across multiple pages of the report by accessing the View tab and selecting the 'Sync Slicers' option. Additionally, the 'Refresh' option can be selected to update the slicers with the latest data, while the 'Visibility' option can be deselected to hide unnecessary slicers and declutter the report interface.

![pbi_sync_slicers](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/f6805637-428d-4c5e-be85-104ec65d9372)
![pbi_Filters2](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/d9fa2750-871c-4261-a712-ef5e4eb8fb86)

### Introducing a Drillthrough Page to Enhancing Report Navigation
---
The incorporation of drillthrough pages in Power BI enriches the user experience by facilitating seamless navigation from a parent record to a child record on another page. This functionality empowers users to delve deeper into specific data subsets and extract actionable insights with ease.

**To implement a drillthrough page, several steps were undertaken:**

1. **Selection of Drillthrough Fields:** The process commenced by selecting key fields, including Students Full Name, Month, School Year, and Book Name, and adding them to the 'Add drillthrough fields' section in the visualizations pane. These fields serve as pivotal elements for navigating from the parent record to the child record.
![pbi_drillthrough](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/0b4c89fb-8250-409d-8d8f-d7ff617b7a0e)

2. **Creation of Drillthrough Report Header:** A crucial component of the drillthrough page is the header, which provides contextual information and enhances user understanding. To achieve this, the Drillthrough report header was inserted into a card visual, enabling users to click on a student and seamlessly transition to a detailed view of the loans they have taken.

3. **Integration of Filtering Attributes:** To further enhance the functionality of the drillthrough page, the report header was modified to reflect filtering attributes such as date and school year. 


```DAX
Report Header Drillthrough =
    "Drillthrough" &
    IF (
        HASONEVALUE ( Student[Full Name] ),
        " -" & VALUES ( Student[Full Name] ),
        ""
    ) &
    IF (
        HASONEVALUE ( dimDate[Month] ),
        " " & VALUES ( dimDate[Month] ),
        ""
    ) &
    IF (
        HASONEVALUE ( dimDate[SchoolYear] ),
        " " & VALUES ( dimDate[SchoolYear] ),
        ""
    )
```
**Drillthrough page showing the report header**
---
![pbi_drillthrough_Maria](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/78565b08-ee13-4be2-aa15-e2e6327a036a)

### Setting up Loan Reminder Email Column with DAX
---
In this section, a DAX expression was configured to generate loan reminder emails for overdue items. The DAX expression is as follows:

```DAX
Loans Reminder Email =
    "mailto:" & RELATED(Student[Email]) & // Pulls email address from related table
    "?subject=" & 
    "You have an outstanding loan for " & Loans[pu_bookname] &
    "&body=" & 
    "Please be advised that the due date for the book was " & Loans[Date Due] & " and it is currently " & Loans[Days Overdue] & " day(s) overdue"
```

This DAX expression constructs a mailto link, incorporating the following components:

- **Recipient Email**: Pulls the email address from the related Student table using the RELATED function.
- **Email Subject**: Specifies the subject line for the email, indicating the outstanding loan details.
- **Email Body**: Provides a message body detailing the loan status, including the book name, due date, and number of days overdue.

Additionally, the calculation for the 'Days Overdue' field is defined as follows:

```DAX
Days Overdue = IF( Loans[pu_isoverdue] = FALSE(), 0, TODAY() - Loans[Date Due] )
```

This DAX expression calculates the number of days an item is overdue based on the difference between the current date (TODAY()) and the due date specified in the 'Date Due' column.

By implementing these DAX expressions, loan reminder emails for overdue items can easily be generated, streamlining communication and facilitating timely action.

**Loans Email Column**
---

![pbi_loanEmail_icon](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/6bdcfd88-2cf1-4437-8609-cdcfdd026305)


