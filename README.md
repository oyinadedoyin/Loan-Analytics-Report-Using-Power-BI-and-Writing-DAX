# School Library Loan Analytics: Leveraging DAX for Enhanced Data Management, Reporting, and Visualization

![janko-ferlic-sfL_QOnmy00-unsplash](https://github.com/oyinadedoyin/Loan-Analytics-Report-Using-Power-BI-and-Writing-DAX/assets/44920093/d2080578-2f16-4e6f-962f-5aee18a469e3)

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
