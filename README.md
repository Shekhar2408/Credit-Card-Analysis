# Credit Card Analysis

### Dashboard Link : https://app.powerbi.com/groups/me/reports/384d017e-e935-44dc-9e7d-1626c1a36de1/ReportSection

## Problem Statement

# Airlines-Dashboard

### Dashboard Link : https://app.powerbi.com/groups/me/reports/384d017e-e935-44dc-9e7d-1626c1a36de1/ReportSection

## Problem Statement
To develop a comprehensive credit card weekly dashboard that
provides real-time insights into key performance metrics and trends,enabling stakeholders to monitor and analyze credit card operations effectively.
This dashboard helps to understand the customers better. It helps the Credit Card Comapny know if their customers are satisfied with their services. Through different ratings, they get to know their improvement area, & thus they can improve their services by identifying these area. It also lets them know the average transaction amount and transaction count, thus since by using this dashboard they have identified this problem, they can further work on factors responsible for these unwanted disruptions.


### Steps followed 

- Step 1 : Load data into Power BI Desktop, dataset is a csv file.
- Step 2 : In order to load the data, create MySQL Query as follows :
-- 0. Create a database 
    Create database ccdb;
    use ccdb ;

-- 1. Create cc_detail table

    CREATE TABLE cc_detail (
    Client_Num INT,
    Card_Category VARCHAR(20),
    Annual_Fees INT,
    Activation_30_Days INT,
    Customer_Acq_Cost INT,
    Week_Start_Date DATE,
    Week_Num VARCHAR(20),
    Qtr VARCHAR(10),
    current_year INT,
    Credit_Limit DECIMAL(10,2),
    Total_Revolving_Bal INT,
    Total_Trans_Amt INT,
    Total_Trans_Ct INT,
    Avg_Utilization_Ratio DECIMAL(10,3),
    Use_Chip VARCHAR(10),
    Exp_Type VARCHAR(50),
    Interest_Earned DECIMAL(10,3),
    Delinquent_Acc VARCHAR(5)
);

-- 2. Create cust_detail table

    CREATE TABLE cust_detail (
    Client_Num INT,
    Customer_Age INT,
    Gender VARCHAR(5),
    Dependent_Count INT,
    Education_Level VARCHAR(50),
    Marital_Status VARCHAR(20),
    State_cd VARCHAR(50),
    Zipcode VARCHAR(20),
    Car_Owner VARCHAR(5),
    House_Owner VARCHAR(5),
    Personal_Loan VARCHAR(5),
    Contact VARCHAR(50),
    Customer_Job VARCHAR(50),
    Income INT,
    Cust_Satisfaction_Score INT
);

    Select * from cc_detail;

    load data infile 'C:/credit_card.csv'
    into table cc_detail
    fields terminated by','
    ignore 1 lines ;

    load data infile 'C:/customer.csv'
    into table cust_detail
    fields terminated by','
    ignore 1 lines ;

    Select * from cust_detail;

    Select @@secure_file_priv;

- Step 2 : Open power query editor & in view tab under Data preview section, check cust_detail dataset.
Step 3 : It was observed that in none of the columns errors & empty values were present, hence started with DAX Query operarions.
- Step 4 : For convinience an extra columns were created;
-- 1) To sort age in buckets of age groups
        Age Group = SWITCH(
    TRUE(),
        'ccdb cust_detail'[Customer_Age] < 30 ,"20-30",
        'ccdb cust_detail'[Customer_Age] >= 30 && 'ccdb           cust_detail'[Customer_Age] < 40, "30-40",
        'ccdb cust_detail'[Customer_Age] >= 40 && 'ccdb cust_detail'[Customer_Age] < 50, "40-50",
        'ccdb cust_detail'[Customer_Age] >= 50 && 'ccdb cust_detail'[Customer_Age] < 64, "50-60",
        'ccdb cust_detail'[Customer_Age] >= 60,"60+",
unknown"
)
snap of new calculated column

![Snap_1]![Age group](https://github.com/Shekhar2408/Credit-Card-Analysis/assets/167020556/6388282a-2184-49c3-89ce-6bb9f274e77a)

--2) To sort income in buckets groups as high,medium and low;
        Income Group = SWITCH(
    TRUE(),
        'ccdb cust_detail'[Income] < 35000, "Low",
        'ccdb cust_detail'[Income] >= 35000 && 
        'ccdb cust_detail'[Income] < 70000, "Med",
        'ccdb cust_detail'[Income] > 70000, "High",
        "unknown"
)
snap of new calculated column

![Snap_2]![Income group](https://github.com/Shekhar2408/Credit-Card-Analysis/assets/167020556/961fb680-d7b5-4f87-bbb3-2eee4c8a6b78)


--3) To calculate the Reveneue;
Revenue = 'ccdb cc_detail'[Annual_Fees] + 'ccdb cc_detail'[Total_Trans_Amt] + 'ccdb cc_detail'[Interest_Earned] 

snap of new calculated column

![Snap_3]![revenue](https://github.com/Shekhar2408/Credit-Card-Analysis/assets/167020556/af9adc8a-ac37-46c5-a3c1-a280dbd54abf)

- Step 5 : For calculating weekly data about revenue and transaction created a a new column in cc_detail dataset;
        Week_num2 = WEEKNUM('ccdb cc_detail'[Week_Start_Date].[Date] ) 

snap of new calculated column

![Snap_4]![Week no](https://github.com/Shekhar2408/Credit-Card-Analysis/assets/167020556/dbd0b43a-ea43-40fb-852f-736967cfbfbc)

    
- Step 6 : New measures created to get the information about current week revenue and previous week as well
--a)Current_Week_Revenue = CALCULATE(
    sum('ccdb cc_detail'[Revenue]),
    filter(
        all('ccdb cc_detail'),
        'ccdb cc_detail'[Week_num2] = max('ccdb cc_detail'[Week_num2])))
--b)Previous_Week_Revenue = CALCULATE(
    sum('ccdb cc_detail'[Revenue]),
    filter(
        all('ccdb cc_detail'),
        'ccdb cc_detail'[Week_num2] = max('ccdb cc_detail'[Week_num2])-1))
- Step 7 : Another measure regarding week over week revenue was created for better explaination;
Week_over_week_revenue = DIVIDE(([Current_Week_Revenue] - [Previoust_Week_Revenue]),[Previoust_Week_Revenue])
- Step 8 : Firstly the size of the canvas adjusted from the format section.
- Step 9 : The background of the report was created in Power Point and saved as picture for the first dashboard on cc_detail.
- Step 10 : The picture was imported to Power BI and placed as canvas background.
- Step 11 : For data visualization line and stacked column chart was taken representing Revenue pre quater with transaction count.

  ![Snap_5]![revenue trns](https://github.com/Shekhar2408/Credit-Card-Analysis/assets/167020556/11aa7aad-843b-4334-bc74-2e004b447f42)
  
- Step 12 : Stacked bar chart was added to canvas depicting the Revenue by Expenditure Type.
- Step 13 : Area chart added showing Revenue by Job type along with funnel chart and line chart representing Revenue by chip used and Card Category repectively.
- Step 14 : A Donut chart was created to represent the Revenue generated by Education Level.
        Basic formating and editing for the above charts were done in the format section.
- Step 15 : A new Visual Card was inserted into the canvas and four fields were added to the same;

  (a) Total Revenue - 55.3 M

  (b) Total Interest Earned - 7.84 M
  
  (c) Total Transaction Count - 656 K
  
  (d) Total Transaction Amount - 45 M
  
- Step 16 : In the report view, under the Build section , a table was generated showcasing the Revenue generated Card Category, Transaction Amount and Interest Earnet all together.
- Step 17 : After inserting all the key components and charts, now adding tree maps on the basis of ;

  (a) No of Quaters

  (b) Gender Based
  
  (c) Income Group
  
  (d) Card Category
  
- Step 14 : Lastly slicer added enabling to filter the data on the basis of week start date.

![dashboard 1](https://github.com/Shekhar2408/Credit-Card-Analysis/assets/167020556/e872e6fc-df05-4a92-a9e5-40c492658bf8)

--[Similary for the second dashboard about cust_detail]
- Step 15 : Keeping the same bakground add another page to report view and start visualising data.
- Step 16 : The main theme for this dashboard is to design and interpret all the visuals according to the gender basis.
- Step 17 : For data visualization line chart was taken representing Revenue by Week based upon gender.

  [Snap 1]![Revenue 2](https://github.com/Shekhar2408/Credit-Card-Analysis/assets/167020556/01119d66-8c17-4b69-87b0-cfe5da94eecb)
  
- Step 18 : Line chart was inserted to represent the revenue by each state and conditional filtering was done to show only the top 5 states using the filters section.
- Step 19 : Stacked bar chart was taken to show the data Revenue Vs Marital Status.
- Step 20 : A ribbon chart and two stacked bar charts added to the canvas to depict relations between Revenue Vs Income Group, Dependent Count and Education Level.
- Step 21 : Another stacked bar chart for revnue by age group as added to the report view.
- Step 22 : A new Visual Card was inserted into the canvas and four fields were added to the same;

  (a) Total Revenue - 55.3 M

  (b) Total Interest Earned - 7.84 M
  
  (c) Income - 576 M
  
  (d) Cust Satisfaction Score - 3.19

- Step 23 : In the report view, under the Build section , a table was generated showcasing the Revenue generated, Card Category, Transaction Amount and Customer Job all together.
- Step 24 : After inserting all the key components and charts, now adding tree maps on the basis of ;

  (a) No of Quaters

  (b) Gender Based
  
  (c) Income Group
  
  (d) Chip Used 

- Step 25 : Lastly slicer added enabling to filter the data on the basis of week start date.

![Dashboard 2](https://github.com/Shekhar2408/Credit-Card-Analysis/assets/167020556/04c6d5b7-42f0-4a2b-b40c-e0fca64b9f60)

### Key insights

Overview YTD:
  (a) Overall revenue is 57M

  (b) Total interest is 8M
  
  (c) Total transaction amount is 46M
  
  (d) Male customers are contributing more in revenue 31M, female 26M

  (e) Blue & Silver credit card are contributing to 93% of overall
transactions
  
  (d) TX, NY & CA is contributing to 68%

  (e) Overall Activation rate is 57.5%
  
  (d) Overall Delinquent rate is 6.06%



