
# Credit Card and Customer Usage Analysis 

### Dashboard Link :[https://app.powerbi.com/groups/32ceb88c-466b-4f10-8125-abdb2877f749/reports/ca743fb6-c72d-4e29-97c1-e506c5474083/ReportSection0acb0ebe544604966858?experience=power-bi]


## Problem Statement
To develop a comprehensive Credit Card Weekly Dashboard that provides real-time insights to key performance metrics and trends enabling stakeholders to monitor and analyze credit card operations effectively.

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

3) Previous Week Revenue
   
        Previoust_Week_Revenue = CALCULATE(
            sum('ccdb cc_detail'[Revenue]),
            filter(
                all('ccdb cc_detail'),
                'ccdb cc_detail'[Week_num2] = max('ccdb cc_detail'[Week_num2])-1))

5) Total Revenue
   
           Revenue = 'ccdb cc_detail'[Annual_Fees] + 'ccdb cc_detail'[Total_Trans_Amt] + 'ccdb cc_detail'[Interest_Earned]

7) Week Description
   
           Week_num2 = WEEKNUM('ccdb cc_detail'[Week_Start_Date].[Date] )

9) Week Over Week Revenue
    
           Week_over_week_revenue = DIVIDE(([Current_Week_Revenue] - [Previoust_Week_Revenue]),[Previoust_Week_Revenue])

11) Age Group
    
           Week_over_week_revenue = DIVIDE(([Current_Week_Revenue] - [Previoust_Week_Revenue]),[Previoust_Week_Revenue])

13) Income Group
    
           Income Group = SWITCH(
         TRUE(),
         'ccdb cust_detail'[Income] < 35000, "Low",
         'ccdb cust_detail'[Income] >= 35000 && 'ccdb cust_detail'[Income] < 70000, "Med",
         'ccdb cust_detail'[Income] > 70000, "High",
         "unknown"
        )
