

<img width="1478" height="828" alt="Retailer_1" src="https://github.com/user-attachments/assets/78e22f1e-450b-4296-a2a5-39eb70c0eaf8" />





## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Data Cleaning/Preparation](#data-cleaningpreparation)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
- [Recommendations](#recommendations)


## Project Overview


## Data Sources



## Data Cleaning/Preparation

- Power Query in Power BI: Loaded and inspected 4 tables to identify and correct errors. Standardized data types and formats to validate data integrity. Calculate custom columns and remove the unnecessaries, etc....
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



## Exploratory Data Analysis (EDA)




##  The Power BI dashboard provides the following insights :



## Recommendations






## Link to the Dashboard


























  

































