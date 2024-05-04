# School Library Loan Analytics: Leveraging DAX for Enhanced Data Management, Reporting, and Visualization

![janko-ferlic-sfL_QOnmy00-unsplash](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/d2080578-2f16-4e6f-962f-5aee18a469e3)

## Table of Contents
---
- [Overview](#overview)
- [Report Structure](#report-structure)
- [Key Features](#key-features)
- [ETL Process](#etl-process)

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

### ETL Process
---
- **Data Extraction**: Utilizes Microsoft Power BI to extract data from Dataverse, importing tables for students and loans.
- **Data Transformation**: Implements Power Query for data transformations, including data type changes, column removal, value replacements, and creation of new columns.
- **Data Linkage**: Establishes relationships between tables, enabling seamless navigation and integration of related data, such as book names and authors.

### Create Date Dimension table (dimDate) using DAX
---
By creating the Date Dimension table, we can effortlessly create custom calendars tailored to our specific needs, such as the Academic Year calendar crucial for this project's context. More generally, it facilitates streamlined filtering of organization-based attributes like week numbers and half-years, enhancing the precision and efficiency of our analyses. One of the table's most significant advantages lies in its ability to support historical comparisons and trend analyses. Through its comprehensive temporal framework, we can accurately calculate metrics such as sales figures from the previous year and percentage changes year on year, empowering us with invaluable insights into business performance and trends.

To format the code block for GitHub, you can use triple backticks (```) to enclose the code and specify the language after the opening triple backticks. Here's your code formatted for GitHub:

```DAX
dimDate = ADDCOLUMNS(
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

