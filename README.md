# Maven Market Customers Dashboard



## Basic Settings

 - 1 . Deselect the "Autodetect new relationships after data is loaded" option in the Data Load tab.
    ![1](https://github.com/manjunath528/maven_dashboard/assets/109943347/15bcd02e-d635-4fd8-b30d-af4017017a94)

 - 2 . Make sure that Locale for import is set to "English (United States)" in the Regional Settings tab.
    ![2](https://github.com/manjunath528/maven_dashboard/assets/109943347/a6cc56fa-9680-443d-b21b-715c538294d8)
     

## Connecting and Shaping the Data

### Connecting  to the MavenMarket_Customers csv file

* Named the table as "Customers".
    ![3](https://github.com/manjunath528/maven_dashboard/assets/109943347/e229838f-645e-476b-b47b-0cae5f768058)    

* Confirmed that data types are accurate (Note: "customer_id" should be whole numbers, and both "customer_acct_num" and "customer_postal_code" should be text).

* Added a new column named "full_name" to merge the the "first_name" and "last_name" columns, separated by a space.
    ![4](https://github.com/manjunath528/maven_dashboard/assets/109943347/9290e33a-78ee-4771-a51a-683d624627e0)

* Created a new column named "birth_year" to extract the year from the "birthdate" column, and format as text.
    ![5](https://github.com/manjunath528/maven_dashboard/assets/109943347/76f8310c-2bc7-41ae-8b5e-0870a8dbb6b1)

* Created a conditional column named "has_children" which equals "N" if "total_children" = 0, otherwise "Y".
    ![6](https://github.com/manjunath528/maven_dashboard/assets/109943347/0feea57f-d3bd-4e12-ad9f-98d8de07a646)

### Connecting  to the MavenMarket_Products csv file

* Named  the table as "Products".
    ![7](https://github.com/manjunath528/maven_dashboard/assets/109943347/f5f405e0-d669-4090-9ca6-8736c6d7ab9a)

* Confirmed that data types are accurate (Note: "product_id" should be whole numbers, "product_sku" should be text), "product_retail_price" and "product_cost" should be decimal numbers).
* Used the statistics tools to return the number of distinct product brands, followed by distinct product names.
    * Spot check: It  has 111 brands and 1,560 product names.

    ![8](https://github.com/manjunath528/maven_dashboard/assets/109943347/71570143-d16f-4fb6-9cd5-905e78840590)

* Added a calculated column named "discount_price", equal to 90% of the original retail price.
    * Formatted as a fixed decimal number, and then used the rounding tool to round to 2 digits.

    ![10](https://github.com/manjunath528/maven_dashboard/assets/109943347/e9b2585a-f67f-416d-9d2a-f06ad1b5474c)

* Selected the  "product_brand" and used the Group By option to calculate the average retail price by brand, and named the new column as “Avg Retail Price".
    * Spot check: It has  an average retail price of $2.18 for Washington products, and $2.21 for Green Ribbon.

    ![9](https://github.com/manjunath528/maven_dashboard/assets/109943347/a915310e-bc41-4f7e-b6b5-d02461826971)


* Deleted the last applied step to return the table to its pre-grouped state.

[]
* Replaced "null" values with zeros in both the "recyclable" and "low-fat" columns.

[]

### Connecting to the MavenMarket_Stores csv file

* Named the table  as “Stores". 

    ![11](https://github.com/manjunath528/maven_dashboard/assets/109943347/c3aeba4f-0f0e-4bb9-b0c8-891748bb1831)
* Confirmed that data types are accurate (Note: "store_id" and "region_id" should be whole numbers).

* Added a calculated column named "full_address", by merging "store_city", "store_state", and "store_country", separated by a comma and space.
    ![12](https://github.com/manjunath528/maven_dashboard/assets/109943347/cc2992d1-e255-4515-bed5-a50c2eeeda5d)

* Added a calculated column named "area_code", by extracting the characters before the dash ("-") in the "store_phone" field. 
    ![13](https://github.com/manjunath528/maven_dashboard/assets/109943347/24582ea6-6e53-4e13-b86f-01dd54033bb4)

### Connecting  to the MavenMarket_Regions csv file
* Named the table "Regions".
    ![14](https://github.com/manjunath528/maven_dashboard/assets/109943347/8a6ec7e7-24bc-4e30-b735-143fc865e405)

* Confirmed that data types are accurate (Note: "region_id" should be whole numbers).

### Connecting to the MavenMarket_Calendar csv file
* Named the table "Calendar".

    ![15](https://github.com/manjunath528/maven_dashboard/assets/109943347/2303bed9-4008-4bf7-b384-38059d6d60b6)

* Used the date tools in the query editor to add the following columns:
    * Start of Week 
    * Name of Day
    * Start of Month
    * Name of Month
    * Quarter of Year
    * Year

    ![16](https://github.com/manjunath528/maven_dashboard/assets/109943347/acef1d6a-9f95-44eb-8e17-f2fb67d5d624)



### Connecting to the MavenMarket_Returns csv file

* Named the table "Return_Data".

    ![17](https://github.com/manjunath528/maven_dashboard/assets/109943347/b3bc9e50-4b2d-47e8-8d9a-cf7780303d1b)
* Confirmed that data types are accurate (all ID columns and quantity should be whole numbers).

### Connecting to the folder

* Connected to the folder path which contains the both files MavenMarket_Transactions_1997 and MavenMarket_Transactions_1998 csv files. 

* Clicked the "Content" column header (double arrow icon) to combine the files, then removed the "Source.Name" column.
* Named the table "Transaction_Data".

    ![1](https://github.com/manjunath528/maven_dashboard/assets/109943347/be60da7b-f3a6-4ba1-b00f-0278d6d7cfe5)
[]
* Confirmed that data types are accurate (all ID columns and quantity should be whole numbers).
    * Spot check: It has the  data from 1/1/1997 through 12/30/1998 in the "transaction_date" column.

## Creating the Data Mode

-  In the MODEL view, arranged tables with the lookup tables above the data tables.

   * Connected Transaction_Data to Customers, Products, and Stores using valid primary/foreign keys.

   * Connected Transaction_Data to Calendar using both date fields, with an inactive "stock_date" relationship.

   * Connected Return_Data to Products, Calendar, and Stores using valid primary/foreign keys.

   * Connected Stores to Regions as a"snowflake" schema.


- Confirmed the following:

   * All relationships follow one-to-many cardinality, with primary keys (1) on the lookup side and foreign keys (*) on the data side.

   * Filters are all one-way (no two-way filters).

   * Filter context flows "downstream" from lookup tables to data tables.

   * Data tables are connected via shared lookup tables (not directly to each other).


- Hidden all foreign keys in both data tables from Report View, as well as "region_id" from the Stores table.

    ![2](https://github.com/manjunath528/maven_dashboard/assets/109943347/ce369673-1f94-4610-90e8-97528b2c258e)

- In the DATA view, completed the following:

* Updated all date fields (across all tables) to the "M/d/yyyy" format using the formatting tools in the Modelling tab.

* Updated "product_retail_price", "product_cost", and "discount_price" to Currency ($ English) format.

* In the Customers table, categorized "customer_city" as City, "customer_postal_code" as Postal Code, and "customer_country" as Country/Region.

* In the Stores table, categorized "store_city" as City, "store_state" as State or Province, "store_country" as Country/Region, and "full_address" as Address.



## Added DAX Measures

- In the DATA view, added the following calculated columns:
   * In the Calendar table, added a column named "Weekend"
       * Equals "Y" for Saturdays or Sundays (otherwise "N")  
               
               Weekend = 
               IF([Day Name] == "Saturday","Y",IF([Day Name] == 
               "Sunday","Y","N"))
            
    * In the Customers table, added a column named "Current Age"
      * Calculates current customer ages using the "birthdate" column and the TODAY() function

             Age = 
             DATEDIFF('Customers'[birthdate],TODAY(),YEAR)
      
    * In the Customers table, added a column named "Priority"
       * Equals "High" for customers who own homes and have Golden membership cards (otherwise "Standard") 
             
             Priority = 

             IF(
             'Customers'[homeowner] == "Y",
             "High",
             "Standard"
             )  
            
    * In the Customers table, added a column named "Short_Country"
      * Returns the first three characters of the customer country, and converts to all uppercase 
           
               Short_Country = 
               UPPER(LEFT('Customers'[Country/Region],3))
    
    * In the Customers table, added a column named "House Number"
      * Extracts all characters/numbers before the first space in the "customer_address" column.
              
              House Number = 
              VAR spaceposition = SEARCH(" ",'Customers'    
              [customer_address],1)
              RETURN
              IF(spaceposition >0,
              LEFT('Customers'[customer_address],spaceposition-1),
              'Customers'[customer_address]
              )


    * In the Products table, added a column named "Price_Tier"
       * Equals "High" if the retail price is >$3, "Mid" if the retail price is >$1, and "Low" otherwise.
         
               Price_Tier = 
               IF('Products'[product_retail_price] > 3,"High",
               IF('Products'[product_retail_price] > 1,"Mid","Low")
               )

- In the REPORT view, added the following measures
    * Created a new measures named "Quantity Sold" and "Quantity Returned" to calculate the sum of quantity from each data table
      * Spot check: It has  total Quantity Sold = 833,489 and total Quantity Returned = 8,289
           
              Quantity Sold = 
              SUM('Transactions'[quantity])

    * Created a new measures named "Total Transactions" and "Total Returns" to calculate the count of rows from each data table
      * Spot check: It has 269,720 transactions and 7,087 returns
          
               Total_Transactions = 
               COUNTROWS('Transactions')
   
   * Created a new measure named "Return Rate" to calculate the ratio of quantity returned to quantity sold (format as %)    
      * Spot check: It has an overall return rate of 0.99%

             Return Rate = 
               ([Quantity Returned]/[Quantity Sold])*100

   * Created a new measure named "Weekend Transactions" to calculate transactions on weekends
      * Spot check: It has 76,608 total weekend transactions.
           
            Weekend Transactions = 
             CALCULATE(
             [Total_Transactions],
             'Calendar'[Weekend] = "Y"
             )
   
   * Created a new measure named "% Weekend Transactions" to calculate weekend transactions as a percentage of total transactions (format as %)
      * Spot check: It has 28.4% weekend transactions.

            % Weekend Transactions = 
            ([Weekend Transactions]/[Total_Transactions])
                  
   * Created a new measure to calculate "Total Revenue" based on transaction quantity and product retail price, and format as $ 
      * Spot check: It has a total revenue of $1,764,546
         
            Total Revenue = 
            SUMX('Transactions',
            'Transactions'[quantity]
             *
            RELATED(
            'Products'[product_retail_price]
            )
            )
  
   * Created a new measure to calculate "Total Cost" based on transaction quantity and product cost, and format as $ 
      * Spot check: It has a total cost of $711,728
                 
                Total Cost = 
               SUMX(
               'Transactions',
               'Transactions'[quantity]
               *
               RELATED('Products'[product_cost]
               )
               )


    * Created a new measure named "Total Profit" to calculate total revenue minus total cost, and format as $
      * Spot check: It has a total profit of $1,052,819.

            Total Profit = 
            [Total Revenue]-[Total Cost]
    
    * Created a new measure to calculate "Profit Margin" by dividing total profit by total revenue calculate total revenue (format as %)
      * Spot check: It has an overall profit margin of 59.67%.

            Profit Margin = 
            ([Total Profit]/[Total Revenue])
   
   * Created a new measure named "Unique Products" to calculate the number of unique product names in the Products table
      * Spot check: It has 1,560 unique products.

            Unique Products = 
            DISTINCTCOUNT('Products'[product_name])
   
   * Created a new measure named "YTD Revenue" to calculate year-to-date total revenue, and format as $
      * Spot check: Created a matrix with "Start of Month" on rows; it has $872,924 in YTD Revenue in September 1998.

            YTD Revenue = 
            CALCULATE([Total Revenue],
            DATESYTD('Calendar'[date])
            )


    * Created a new measure named "60-Day Revenue" to calculate a running revenue total over a 60-day period, and format as $
      * Spot check: Created a matrix with "date" on rows; it has $97,570 in 60-Day Revenue on 4/14/1997.

              60-day Revenue = 
              CALCULATE(
              [Total Revenue],
              DATESINPERIOD(
              'Calendar'[date],
              MAX('Calendar'[date]),
              -60,DAY))
   
    * Created new measures named  "Last Month Transactions", "Last Month Revenue", "Last Month Profit", and "Last Month Returns"
      * Spot check: Created a matrix with "Start of Month" on rows to confirm accuracy.

               Last Month Transactions = 
               CALCULATE(
               [Total_Transactions],
               DATEADD(
               'Calendar'[date],
               -1,
              MONTH))

   * Created a new measure named "Revenue Target" based on a 5% lift over the previous month revenue, and format as $
      * Spot check: It has a Revenue Target of $99,223 in March 1998.

              Revenue Target = 
             [Last Month Revenue]*1.05

## Building the Report

   * Renamed the tab "Topline Performance" and inserted the Maven Market logo
     
      ![Screenshot (1211)](https://github.com/manjunath528/maven_dashboard/assets/109943347/d094450f-cce5-4961-bd47-c40d7083ef8b)

   * Inserted a Matrix visual to show Total Transactions, Total Profit, Profit Margin, and Return Rate by Product_Brand (on rows).
    
     * Added conditional formatting to show data bars on the Total Transactions column, and color scales on Profit Margin (White to Green) and Return Rate (White to Red)
  
     * Added a visual level Top N filter to only show the top 30 product brands, then sort descending by Total Transactions
        
        ![Screenshot (1212)](https://github.com/manjunath528/maven_dashboard/assets/109943347/f4505b25-c9b9-4922-b78a-15b872005fbb)

   * Added a KPI Card to show Total Transactions, with Start of Month as the trend axis and Last Month Transactions as the target goal.

         
      * Updated the title to "Current Month Transactions".
      * Created two more copies: one for Total Profit (vs. Last month Profit) and one for Total Returns (vs. Last Month Returns) and updated their titles and changed the Returns chart to color coding to "Low is Good".

      ![Screenshot (1213)](https://github.com/manjunath528/maven_dashboard/assets/109943347/58b8f1e3-84b0-41a1-83ff-7ccd9392e2f2)

   * Added a Map visual to show Total Transactions by store city
      * Added a slicer for store country 
      * Under the "selection controls" menu in the formatting pane, activated the "Show Select All" option.

     ![Screenshot (122)](https://github.com/manjunath528/maven_dashboard/assets/109943347/097af19a-a7dd-4f64-835e-54c6c7b71522)
   * Next to the map, added a Treemap visual to break down Total Transactions by store country
      * Pulled in store_state and store_city beneath store_country in the "Group" field to enable drill-up and drill-down functionality.

       ![Screenshot (1216)](https://github.com/manjunath528/maven_dashboard/assets/109943347/f2a7ce6c-c1fa-4633-ad8b-eb974f5a0e9c)

   * Beneath the map, added a Column Chart to show Total Revenue by month.
      * Added a report level filter to only show data for 1998
      * Updated the title to "Monthly Revenue Trending"
      
      ![Screenshot (1217)](https://github.com/manjunath528/maven_dashboard/assets/109943347/3716eb27-dfe9-4379-a8d0-75e24c3e406f)

   * In the lower right, added a Gauge Chart to show Total Revenue against Revenue Target (as either "target value" or "maximum value").
      * Added a visual level Top N filter to show the latest Start of Month.
      * Removed data labels, and updated the title as "Revenue vs. Target".
        
        ![Screenshot (1218)](https://github.com/manjunath528/maven_dashboard/assets/109943347/df43d10f-aa54-411e-966c-97238a733fb3)

   * Selected the Matrix and activated the  Edit interactions option to prevent the Treemap from filtering.


* Finally the dashboard looks as shown below :

  ![Screenshot (123)](https://github.com/manjunath528/maven_dashboard/assets/109943347/89bf5c10-0fa8-4a1b-abee-63e78dbf8451)
