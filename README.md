
# Credit Card and Customer Usage Analysis 

### Dashboard Link :[https://app.powerbi.com/groups/me/reports/76decb57-0827-40db-b3c5-61867e6cf551/ReportSection?experience=power-bi]


## Problem Statement
To develop a comprehensive Credit Card Weekly Dashboard that provides real-time insights to key performanse metrics and trends enabling stakeholders to monitor and analyze credit card operations effectively.

## Methodology
1) Data collection: Utilize publicly available open data source for the different kind of credit cards usage.
2) Data analysis: Employ statistical analysis and data visualization techniques to explore the revenue and usage patterns.
3) Comparative analysis: Conduct comparative analysis to identify differences in transactions among different cards.
4) Data Creation: Wrote MySql codes to create the database and run some querries to get key insights.
5) Data Visualization: After running querries and transforming data on Power BI created an interactive dashboard to highlight the key points. 

## Related DAX Queries 
1) Current Week Revenue
        Current_Week_Revenue = CALCULATE(
            sum('ccdb cc_detail'[Revenue]),
            filter(
                all('ccdb cc_detail'),
                'ccdb cc_detail'[Week_num2] = max('ccdb cc_detail'[Week_num2])))

2) Previous Week Revenue
        Previoust_Week_Revenue = CALCULATE(
            sum('ccdb cc_detail'[Revenue]),
            filter(
                all('ccdb cc_detail'),
                'ccdb cc_detail'[Week_num2] = max('ccdb cc_detail'[Week_num2])-1))

3) Total Revenue
           Revenue = 'ccdb cc_detail'[Annual_Fees] + 'ccdb cc_detail'[Total_Trans_Amt] + 'ccdb cc_detail'[Interest_Earned]

4) Week Description 
           Week_num2 = WEEKNUM('ccdb cc_detail'[Week_Start_Date].[Date] )

5) Week Over Week Revenue
           Week_over_week_revenue = DIVIDE(([Current_Week_Revenue] - [Previoust_Week_Revenue]),[Previoust_Week_Revenue])

6) Age Group
           Week_over_week_revenue = DIVIDE(([Current_Week_Revenue] - [Previoust_Week_Revenue]),[Previoust_Week_Revenue])

7) Income Group
   Income Group = SWITCH(
         TRUE(),
         'ccdb cust_detail'[Income] < 35000, "Low",
         'ccdb cust_detail'[Income] >= 35000 && 'ccdb cust_detail'[Income] < 70000, "Med",
         'ccdb cust_detail'[Income] > 70000, "High",
         "unknown"
        )
