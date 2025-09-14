

<img width="1478" height="828" alt="Retailer_1" src="https://github.com/user-attachments/assets/78e22f1e-450b-4296-a2a5-39eb70c0eaf8" />





## Table of Contents
- [Project Overview](#project-overview)
- [Data Cleaning/Preparation](#data-cleaningpreparation)
- [Insights](#analysis-visualization-and-insights)
- [Recommendations](#recommendations)


## Project Overview
In a competitive retail landscape, businesses often struggle to extract actionable insights from vast datasets, leading to missed opportunities for growth and optimization. The Global Electronic Retailer project, built in Power BI using a Maven Analytics dataset, addresses this by transforming raw data into a dynamic, interactive dashboard that empowers data-driven decision-making. 
In this project, I utilized Power Query to load and preprocess three dimension tables (Store, Product, Customer) and one fact table, standardizing data types, eliminating redundant columns, and creating custom columns like age bins derived from customer birthdates. A star schema was established in the data model to connect tables efficiently, complemented by a custom DAX-generated date table for advanced time intelligence analysis. 
The resulting Power BI dashboard spans four insightful pages—Order Details, Customer Insights, Product Performance, and Global Revenue Overview—showcasing trends across categories, continents, countries, and months. With approximately 90 DAX measures, including key metrics, year-over-year variance, and max/min calculations, the dashboard leverages interactive features like drill-throughs, tooltips, field parameters, bookmarks, and conditional formatting to deliver a user-friendly, visually compelling experience that drives strategic business decisions and fuels revenue growth.


## Data Cleaning/Preparation

- Power Query :
    - Customers Table 
<img width="1919" height="1075" alt="Screenshot 2025-09-14 063031" src="https://github.com/user-attachments/assets/7eab1624-d81e-443e-b96d-02085e724010" />

  - Products Table 
<img width="1917" height="1079" alt="Screenshot 2025-09-14 063049" src="https://github.com/user-attachments/assets/2d2bee96-eff9-4d3a-9e85-e616bd7edaf5" />

  - Sales Table
<img width="1919" height="1069" alt="Screenshot 2025-09-14 063111" src="https://github.com/user-attachments/assets/03e4b804-814f-4cde-9ee7-e705fa04916c" />

  - Date Table using DAX
<img width="1913" height="1050" alt="image" src="https://github.com/user-attachments/assets/26629456-adfb-4e21-8629-2a3ce5541dfe" />

  - Data model, star schema to create relationships betwenn tables.
<img width="1917" height="1061" alt="image" src="https://github.com/user-attachments/assets/8dafd60b-ffac-4fa0-bc2c-5c7a37a921e6" />

- DAX: Time intelligence calculations.
  - Mains Measures
```
 - Revenue = 
SUMX(
    Sales,
    Sales[Quantity] * RELATED(Products[Unit Price USD])
    )

 - Profit = [Revenue] - [COGS]

 - Avg Delivery Days = 
AVERAGEX(
    Sales, 
    DATEDIFF(
        Sales[Order Date],
         Sales[Delivery Date],
          DAY
          )
          
    )

 - Avg Order Value = 
DIVIDE(
    [Revenue], 
    [Total orders],
    0 
     )

  - Total Customers = 
 COUNTROWS(
    Customers
    )



```


  - Previous Year Measures

```
 - PY Revenue = 
CALCULATE(
    [Revenue], 
    SAMEPERIODLASTYEAR(
        'Date Table'[Date]
    )
)


- PY COGS = 
CALCULATE(
    [COGS], 
    SAMEPERIODLASTYEAR(
        'Date Table'[Date]
    )
)

```

   - vs Previous Year Measures

```
  - vs PY Profit Margin = 
VAR _CM = [Profit Margin]
VAR _PY = [PY Profit Margin]
VAR _perc = DIVIDE(_CM - _PY, _PY)
VAR _format = 
SWITCH(
    TRUE(),
    _perc > 0, UNICHAR(11165) & " " & FORMAT(_perc, "0.0%"),
    _perc < 0, UNICHAR(11167) & " " & FORMAT(_perc * -1, "0.0%"),
    FORMAT(_perc, "0.0%")
)
RETURN _format


  - vs PY Avg Price = 
VAR _CM = [Avg Price]
VAR _PY = [PY Avg Price]
VAR _perc = DIVIDE(_CM - _PY, _PY)
VAR _format = 
SWITCH(
    TRUE(),
    _perc > 0, UNICHAR(11165) & " " & FORMAT(_perc, "0.0%"),
    _perc < 0, UNICHAR(11167) & " " & FORMAT(_perc * -1, "0.0%"),
    FORMAT(_perc, "0.0%")
)
RETURN _format

```

   - Color Formatting Measures

```
  - CF PY AOV = 
VAR _CM = [Avg Order Value]
VAR _PY = [PY AOV]
VAR _perc = DIVIDE(_CM - _PY, _PY)
VAR _format = 
SWITCH(
    TRUE(),
    _perc > 0, "DarkGreen",
    _perc < 0, "DarkRed",
    "Gray"
)
RETURN _format

  - CF PY Unit Sold = 
VAR _CM = [Unit Sold]
VAR _PY = [PY Unit Sold]
VAR _perc = DIVIDE(_CM - _PY, _PY)
VAR _format = 
SWITCH(
    TRUE(),
    _perc > 0, "DarkGreen",
    _perc < 0, "DarkRed",
    "Gray"
)
RETURN _format


- Max_Value_%_Customers = 
VAR _max_value = 
MAXX(
    ALL('Date Table'[MonthNumber]),
    [% Inac. Customers]
)

VAR _check = IF(_max_value = [% Inac. Customers], [% Inac. Customers], BLANK())
RETURN _check

  - Min_Value_COGS = 
VAR _min_value = 
MINX(
    ALL('Date Table'[MonthNumber]),
    [COGS]
)

VAR _check = IF(_min_value = [COGS], [COGS], BLANK())
RETURN _check
```
    

- Power Point: Background Wireframe 
<img width="4663" height="2671" alt="Picture_p" src="https://github.com/user-attachments/assets/559c38c9-68b1-44a2-92b4-f0e6746c5958" />



## Analysis, Visualization and Insights


 - The Power BI dashboard provides the following insights :



## Recommendations






## Link to the Dashboard


























  

































