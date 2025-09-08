# IBM HR Analytics: Employee Attrition Analysis

## Project Objective
The primary goal of this project is to analyze the key factors contributing to employee attrition within a company. By identifying high-risk employee profiles and influential drivers, the business can develop targeted retention strategies to improve employee satisfaction and reduce turnover costs.

## Dataset
The analysis is based on the "IBM HR Analytics Employee Attrition & Performance" dataset, publicly available on [Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset).

## Tools Used
*   **Power BI:** For data modeling, analysis, and visualization.
*   **Power Query:** For data extraction, transformation, and loading (ETL).
*   **DAX:** For creating custom calculations and key performance indicators (KPIs).

---

## Methodology

### 1. Data Cleaning and Transformation (ETL)
The raw data was loaded into Power BI and transformed using the Power Query Editor. Key steps included:
*   Checking and adjusting data types for all columns.
*   Renaming columns for better readability.
*   Replacing numerical values to categorical (e.g. Education levels from 1-5 to 'Below College', 'College' etc.).
*   Creating **conditional columns** to group data into meaningful categories (e.g. generating salary brackets from the 'MonthlyRate' column).
*   Ensuring data integrity and removing inconsistencies.

### 2. DAX Calculations
Several key measures were created using DAX to drive the analysis:

*   **Attrition:** Count of all attrition. Opposite mesaure: **No Attrition**.

    ```dax
    Attrition = CALCULATE(
        COUNT('HR-Employee-Attrition'[Attrition]), 'HR-Employee-Attrition'[Attrition] = "YES")
    ```

*   **Attrition Ratio:** A core KPI to measure the ratio of employees who have left the company to those who stayed.
    ```dax
    Attrition Ratio = 
    DIVIDE([Attrition], [No Attrition], "Full Attrition")
    ```

*   **Total Number of Employees:** A simple count of all employees in the dataset.
    ```dax
    Total Number of Employees = COUNTROWS('HR-Employee-Attrition')
    ```

*   **Dynamic Drill-through Title:** A sophisticated measure that dynamically displays the active drill-through filter context from the main page. This provides a clear and contextual title on the detailed view (e.g., "Education: Bachelor").
    ```dax
    Drill Through Mesaure = 
    VAR SelectedFeatureName = MAX('Select Feature'[Select Feature])

    VAR SelectedValue =
    SWITCH(
        SelectedFeatureName,
        "Education", SELECTEDVALUE('HR-Employee-Attrition'[Education]),
        ...
        BLANK()
    )

    RETURN
        IF(
            ISBLANK(SelectedValue),
            "No Active Filter",
            SelectedFeatureName & ": " & SelectedValue
        )

    ```

### 3. Report Design and Interactivity
The report was designed to be intuitive, insightful, and fully interactive:

*   **Field Parameters:** This feature was implemented to allow end-users to dynamically change the dimensions on charts. For example, a user can switch a single bar chart to show attrition by `Job Role`, `Department`, or `Marital Status` without needing separate visuals.
*   **Interactive Visualizations:** A mix of charts, graphs, and cards was used to tell a clear story. Slicers were added for easy filtering by differen features.
*   **Navigation:** The report spans multiple pages, each focusing on a different aspect of the analysis. User-friendly navigation buttons (arrows) were implemented to guide the user seamlessly through the report.
*   **Drill-through:** Users can click on a data point (e.g., a specific department) and drill through to a detailed page that is pre-filtered for that selection, allowing for in-depth exploration.

---

## Key Findings & Insights

The analysis yielded several actionable insights:

1.  **Salary Is Not a Primary Driver:** Contrary to common assumptions, the analysis revealed no significant correlation between an employee's monthly rate and their decision to leave.
2.  **Demographics Play a Key Role:** The highest attrition rates are observed among **young, single employees**, suggesting that this group may be seeking different opportunities or have fewer ties to their current location.
3.  **Job Role Is a Critical Factor:** The **'Sales Representative'** position experiences a significantly higher attrition rate compared to other roles in the company. This indicates a potential issue with job satisfaction, workload, or expectations in this specific role.

## Dashboard Overview

### Page 1: AI-Driven Key Influencers Summary

![Dashboard Page 1](/IBM-HR-Analytics-Employee-Attrition//images/Page1.png)

*   **Purpose:** To provide a high-level overview and identify the most statistically significant drivers of attrition using Power BI's built-in AI capabilities.
*   **Key Features:**
    *   A pie chart on the left shows the overall company attrition rate for context.
    *   The **Key Influencers** visual on the right automatically analyzes all data points to determine which factors increase the likelihood of an employee leaving.
    *   This page delivers immediate, data-driven insights.

### Page 2: Attrition by Categorical Features

![Dashboard Page 2](/IBM-HR-Analytics-Employee-Attrition//images/Page2.png)

*   **Purpose:** The counterpart to the first page, this view allows for the analysis of **categorical employee data**.
*   **Key Features:**
    *   A **Field Parameter** slicer lets users switch between features like `Department`, `Education`, `Job Role`, and `Marital Status`.
    *   Multiple visuals (bar chart, pie chart, ratio chart) provide a comprehensive look at the selected category's impact on attrition.
    *   A **Drill-through** button allows users to dive deeper into a specific category shown on the charts.

### Page 3: Drill-through Detail Page

![Dashboard Page 3](/IBM-HR-Analytics-Employee-Attrition//images/Page3.png)

*   **Purpose:** To offer a granular, filtered view of a specific employee segment selected from the previous page.
*   **Key Features:**
    *   The page title and cards are **dynamic**, updating to reflect the drilled-through context
    *   All visuals on this page are filtered for the selected segment, enabling a deep-dive analysis. For example, a user can see the attrition patterns by `Years in Current Role` or `Monthly Rate` specifically for employees with a Master's degree.    

### Page 4: Attrition by Numerical Features

![Dashboard Page 4](/IBM-HR-Analytics-Employee-Attrition//images/Page4.png)

*   **Purpose:** To dynamically explore the relationship between various **numerical employee attributes** and attrition.
*   **Key Features:**
    *   A **Field Parameter** slicer on the left allows users to select a feature (e.g., `Age`, `Distance From Home`, `Years At Company`).
    *   The line chart displays the absolute number of employees who left versus the total, plotted against the selected feature.
    *   The stacked area chart shows the proportional attrition rate, making it easy to spot trends and spikes at specific values.

### Page 5: Salary and Compensation Analysis

![Dashboard Page 5](/IBM-HR-Analytics-Employee-Attrition//images/Page5.png)

*   **Purpose:** To perform a focused analysis on whether compensation is a significant driver of employee attrition.
*   **Key Features:**
    *   Visualizations are dedicated to `Monthly Rate` and `Hourly Rate`.
    *   For each compensation type, the report shows both the absolute count of employees and the attrition ratio across different pay brackets.
    *   This view helps quickly validate or debunk the hypothesis that lower pay leads to higher attrition, revealing a relatively stable attrition rate across all salary levels.




