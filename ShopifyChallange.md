# Winter 2022 Data Science Intern Challenge 
## Question 1: Given some sample data, write a program to answer the following: 




On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe.
We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13.
Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

### a) Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.

• On initial inspection, the AOV is skewed and needs to be investigated. Further analysis using simple excel statistics (COUNTIF) revealed that approx. 98% of the amounts were less than $1,000 per order and 71 items were very large orders (greater than $25k).   
• I then looked at each order and took the value of each unit.  I.e. I averaged the Order by the number of items of each order.  This revealed the unit costs to be between $90 and $352 of most orders

• 46 orders (less than 1%) had unit costs of $25,725 – all ordered from shop_ID were No. 78.  All these inspections were a series of sorts and countifs.

• SHOP_ID 42 and USER_ID 607 showed 2000 orders of a total of $704,000.  This equates to $352/item well inside the norm of affordable sneakers.      

There are a number of value added information that can be used to properly reflect the sales.  Since all the sales occurred in March 2017.  We can note the following:

• $15,725,640.00 sales in March 2017 (5000 orders)

• 4938 orders with an AOV of sales under $2,000.  Using an “Average IF” function results in an AOV of $302.58

• 46 orders of $25,725 / unit (Shop_ID: 78)

• 17 orders with a value of $704,000 each for a unit cost of $352.

Although, even after correcting this data from Shop_Id: 78, the AOV was still $2696.36, which seems too high for this data set. Therefore by removing the 17 orders of 704,000$ as outliers would make a more useful and reasonable average. 
  

### b) What metric would you report for this dataset?

• It is important to break down the data in segments and parts to identify the reasons for the large anomalous data, if any.  The data set may reveal reasonable outcomes … or in the case of  shop_id 78, perhaps a problem with the ordering system that is shifting the decimal system from; $257.25 to $25,725, $514.50 to 51,450, $771.75 to $77,175, $1029.00 to $102,900, $1543.50 to $154,350. 

• If we take this system correction into account, then the AOV for the entire 5000 orders is: $2696.36

• Corrected total sales for March 2017 is $13,484,482.50

• With $2696.36 as an average still seeming too high for this dataset, I would remove the 17 orders with a value of $704,000 to get a more accuarate average.  

### c) What is its value?

Prefered Solution:

• By correcting the data for Shop_Id 78, and removing the 17 orders of $704,000 as ouliers the average(mean) would be: $303.24

Other Solutions:

• By removing the "bad data" as outliers (704000, 154350, 102900, 77175, 51450, 25725) from the data set, the average(mean) would be : $302.58

• If we are going to present the average number as the economic situation, we tend to use the mean, generally after removing the outliers. In this case with many of the values being in a range and by not changing the data and calculating the median, would be: $284.00


## Question 2: For this question you’ll need to use SQL. 
Follow this link to access the data set required for the challenge. 
Please use queries to answer the following questions. 
Paste your queries along with your final numerical answers below.

### a) How many orders were shipped by Speedy Express in total?

In this questions I look at the keywords: **Orders**, **Speedy Express** + we are looking for a Quantity (therefore, a number)
Therefore I will look individually where these values are located. 
- The Shippers Table holds the ShipperName **Speedy Express**
- Orders holds the ShipperID that is associated with every **Order**, where we can calucate the amount of Shipper Id's associated with 
Speedy Express using a JOIN because they have the common value of **ShipperID**. 

1) We want the COUNT of ShipperID's column to be our final query result. : **SELECT COUNT (Shippers.ShipperID)**
2) We want to specify the Shippers table to be used in the final query result : **FROM Shippers**
3) To concatenate the data from Orders and Shippers we must use a JOIN Clause. : **JOIN Orders ON Orders.ShipperID = Shippers.ShipperID**
4) We must filter our data, as we only want to see the orders where the ShipperName is equal to "Speedy Express" : **WHERE Shippers.ShipperName = "Speedy Express";**

```
    SELECT COUNT (Shippers.ShipperID)
    FROM Shippers
    JOIN Orders ON Orders.ShipperID = Shippers.ShipperID
    WHERE Shippers.ShipperName = "Speedy Express";
```
**Result** = 54
    
### b) What is the last name of the employee with the most orders?

In this question I look at the keywords: **Last Name**, **Employee**, **Orders**.
- Employees Table holds the **LastName** Column.
- Orders table holds the Quantity of **Orders** related to the EmployeeID which is a common value in the Employees Table. 

1) We want the final query to show us a result under the LastName column. : **SELECT Employees.LastName**
2) We want to Specify the Employees table to be used in the final query result: **FROM Employees**
3) We need to concatenate the data from the Orders table and the Employees Table with the common values of EmployeeID so that we can count how many orders are associate 
with a corresponding employeeId. : **JOIN Orders ON Orders.EmployeeID = Employees.EmployeeID**
4) We must group together rows that have common values of EmployeeId so that we don't get multiple values of EmployeeId.: **GROUP BY Employees.EmployeeID**
5) We use the ORDER BY , COUNT and DESC clause to order by the amount a EmployeeID occures. This will give us the number of orders that an Employee's Id has made (ie. most orders to least orders). : **ORDER BY COUNT (Employees.EmployeeID) DESC**
6) Finally, to get a final answer we just want to Top of the list which will be the most occuring EmployeeId's Last Name to appear : **LIMIT 1;**  

```
    SELECT Employees.LastName
    FROM Employees 
    JOIN Orders ON Orders.EmployeeID = Employees.EmployeeID
    GROUP BY Employees.EmployeeID
    ORDER BY COUNT (Employees.EmployeeID) DESC
    LIMIT 1; 
 ```
**Result** = Peacock  

  
### c) What product was ordered the most by customers in Germany?

In this question I look at the key words : Product, Ordered, Customers, Germany. 
Therefore I look individually where these values are located. 
- There is a Product table that holds Product Names which will tell me which **Product** is ordered by name, but I still need more data.
- There is a Customer table that tells me the info of the purchasers, here I am able to see the countries, where I can get **Germany** data from.
- Now I need to find a link to these tables so that I can find how many and which products were ordered from germany. 
- OrderDetails table has the **Quantity** column I am looking for. Now that I know where the data I am looking for is, I start typing my Query. 
- I need to Write a Query that will allow all this data to connect and filter through the data. 

1) We want the ProductName columns to be listed in the final query results : **SELECT Products.ProductName**
2) We want to specify the Products table to be used in the final query result :  **FROM Products** 
3) I am using JOIN clauses because I want to obtain data from a number of tables. To get the answer we want, we have to compare data from multiple tables.    
   - First, we want to concatenate the rows of OrderDetails Table with Products in order to get a list of all the Products that match the 
   OrderDetails, by ProductId. : **JOIN OrderDetails ON OrderDetails.ProductID = Products.ProductId**  
   - Now we want to concatenate the Orders Table with the OrderDetails Table, with their common values of OrderId to be able to connect the Products table, 
   Customer table and OrderDetails Table. : **JOIN Orders ON Orders.OrderID = OrderDetails.OrderId**
   - Lastly, concatenate the Customers Table with Orders with the common values of Customer Id : **JOIN Customers ON Customers.CustomerID = Orders.CustomerId**
4) Now, we want to filter through our tables data to display the correct query result we are looking for. 
   - We want to specify the rows in Table Customer, Row Country that matches "Germany" to be listed. :  **WHERE Customers.Country = "Germany"**
   - We want to group together rows that have common values of ProductName so that I don't get multiple values of ProductName. : **GROUP BY Products.ProductName** 
   - Now I want to put the data in Order by sum of the Quantity of orders so that I know which ProductName was ordered the most in descending order. : **ORDER BY SUM(OrderDetails.Quantity) DESC** 
   - Finally, to clean it all up, we want a clear answer and just display the top result. : **LIMIT 1;**
  
  ```
      SELECT Products.ProductName 
      FROM Products  
      JOIN OrderDetails ON OrderDetails.ProductID = Products.ProductId 
      JOIN Orders ON Orders.OrderID = OrderDetails.OrderId 
      JOIN Customers ON Customers.CustomerID = Orders.CustomerId 
      WHERE Customers.Country = "Germany" 
      GROUP BY Products.ProductName  
      ORDER BY SUM(OrderDetails.Quantity) DESC 
      LIMIT 1;
   ```
**Result** = Boston Crab Meat

