

<img width="1478" height="828" alt="Retailer_1" src="https://github.com/user-attachments/assets/78e22f1e-450b-4296-a2a5-39eb70c0eaf8" />





## Table of Contents
- [Project Overview](#project-overview)
- [Data Cleaning/Preparation](#data-cleaningpreparation)
- [Insights](#analysis-visualization-and-insights)
- [Business Meaning](#business-meaning)
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

- DAX calculations.
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

 - Summary
   
   - Total revenue is $55.76M with a 12.1% increase vs. previous year (PY), COGS is $23.09M (12.2% vs. PY), units sold are 197,755 (12.6% vs. PY), profit is $32.66M (12.1% vs. PY), and profit margin is 58.58% (no change vs. PY). This indicates strong overall growth and stable profitability.
   - Revenue is nearly evenly split (49.18% Male, 50.82% Female), suggesting no significant gender bias in purchasing behavior.
   - Black (32.95%) and White (30.29%) dominate, with Silver (36.76%) also significant, highlighting popular product color preferences.
   - Revenue is concentrated in Europe (38.29%) and North America (61.71%), with minimal contribution from Australia (4.69%), indicating key markets.
   - Revenue peaks in January and November, with a notable dip in April, pointing to seasonal trends.
   - USA leads, followed by UK, Germany, Canada, and others, showing a strong North American and European customer base.
  
<img width="1478" height="828" alt="Retailer_1" src="https://github.com/user-attachments/assets/bf0627b0-7a50-4ec3-9a74-5a7271368dd5" />



   - Total orders are 62,884 (12.8% vs. PY), average order value is $887 (0.6% decrease vs. PY), average delivery days are 4.5 (1.9% improvement vs. PY), and average price is $356.03 (no change vs. PY).
   - Orders are split 49.42% Male and 50.58% Female, with North America (62.31%), Europe (33.01%), and Australia (4.68%) mirroring revenue distribution.

 <img width="1475" height="824" alt="Retailer_2" src="https://github.com/user-attachments/assets/b78bbc61-d185-4bb3-8ec0-4bdf7fee4b6e" />   
 
   - Computers (14,025), Cell Phones (10,158), and Music, Movies & Audio (9,169) lead, with subcategories like Desktops (6,447) and Laptops (1,600) showing specific demand.
   - Total customers are 15,266, with 11,887 active (5.0% vs. PY) and 3,379 inactive (14.3% vs. PY), equating to 22.13% inactivity.
   - The 24-44 age group is the largest active segment, while 75-95 shows higher inactivity.

<img width="1399" height="824" alt="Retailer_3" src="https://github.com/user-attachments/assets/02ccce4e-1330-4043-9a4a-51baf6da6a97" />



   - North America has the highest active customer base, with Europe and Australia showing moderate activity and inactivity.
   - Key products include desktops (e.g., Adventure Works Desktop PC2.33 XZ330 Black at $505,450 revenue, $337,986 profit) and televisions (e.g., Adventure Works 52" LCD HDTV X590 Black at $374,099 revenue, $250,153 profit). Black and Silver desktops show high revenue ($466,089 and $466,089 respectively), while White and Brown variants also perform well.
   - Computers lead with the highest revenue, followed by Home Appliances, Cameras and Camcorders, Cell Phones, TV and Video, Audio, Music, Movies & Audio, and Games and Toys, indicating a strong demand for tech and home-related products.

<img width="1474" height="823" alt="Retailer_4" src="https://github.com/user-attachments/assets/453299b3-54c2-49f5-9938-0e89c9e740f2" />


     


 ## Business Meaning
 
- The consistent growth in revenue, units sold, and profit suggests effective sales strategies and market demand. Stable profit margins indicate good cost management.
- Balanced gender revenue distribution implies marketing can target both genders equally.
- Concentration in Europe and North America highlights where to focus marketing and expansion efforts, while the low Australian revenue suggests untapped potential or weaker market penetration.
- Color preferences (Black, White, Silver) reflect customer tastes, which should guide inventory and design decisions.
- The increase in orders with a slight drop in average order value suggests more low-value purchases, possibly from new or price-sensitive customers.
- Improved delivery times enhance customer satisfaction and could boost retention.
- A growing active customer base is positive, but the rising inactive percentage (22.13%) signals potential churn risk.
- Younger age groups (24-44) are more engaged, suggesting targeted retention for older demographics.
- Regional activity patterns reinforce the focus on North America and Europe for customer engagement.
- High-performing product categories (Computers, Home Appliances, Cell Phones) and specific products (desktops, televisions) highlight areas of strength to maintain or expand and prioritize stock and promotions..




## Recommendations

 - Launch gender-neutral marketing campaigns leveraging the balanced revenue split (49.18% Male, 50.82% Female).
 - Focus seasonal promotions in January and November to capitalize on revenue peaks.
 - Invest in marketing and logistics in Australia to boost its low revenue contribution (4.69%).
 - Strengthen presence in secondary markets like Canada, Italy, and the Netherlands that need further analysis.
 - Adjust pricing strategies to improve the declining average order value ($887, 0.6% decrease vs. PY).
 - Maintain or enhance delivery efficiency (4.5 days) to sustain customer satisfaction.
 - Prioritize stock and promotions for high-demand product categories (Computers, Cell Phones).
 - Develop re-engagement strategies for the 22.13% inactive customers, especially the 75-95 age group.
 - Offer loyalty incentives to the active 24-44 age segment to maintain engagement.
 - Focus customer retention efforts on North America and Europe, where active customer bases are strongest.
 - Prioritize production and marketing for high-revenue categories like Computers and Home Appliances.
 - Continue offering popular colors (Black, Silver, White, Brown) and explore limited-edition colors for lower-performing categories like Games and Toys.
 - Maintain cost efficiency to preserve the 58.58% profit margin and investigate cost-saving opportunities for moderately performing categories (e.g., Audio, Music, Movies & Audio).





























  

































