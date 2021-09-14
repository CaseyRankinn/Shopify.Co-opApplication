# Winter 2022 Data Science Intern Challenge 
Question 1: Given some sample data, write a program to answer the following: 

On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe.
We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13.
Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

a) Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.
      
- How I approached this problem was simple. I thought to myself, what makes this data "incorrect"? When I quickly skimmed 
through the dataset there were few large numbers that were most like the result of the average calculation outcome as $3145.13. I rearranged the numbers in the column
from smallest value to largest value and I saw that the numbers 704000, 154350, 102900, 77175, 51450, 25725 didn't match with the average data with the overall 
data set of order_amount, as there was a big jump from 1760 to 25725. These numbers are the ouliers and are effecting the accuracy of the data. 

- Therefore, the issue with this calculation is that there are a few values under the order_amounts column that jump a large amount comparative to the rest of the dataset.
A better method to calculate this data would be to remove these outliers and then calculate the average (mean).
- Another method to solving this problem: If one did not want to remove any data from the data set, it would be appropriate in this case to find the median instead. 
Since the dataset is large and a majority of the values fall withing a range, finding the median would be more accurate than the mean. 
  

b) What metric would you report for this dataset?

- The metric I would use for this dataset is to find the median value instead of the mean because you are still able to keep all of the data in the problem and still have 
an accurate evaluation. This is a great example of the importance of data analysis because we are able to interperate the data and improve the efficiency of the dataset.  




c) What is its value?
- By removing the outliers (704000, 154350, 102900, 77175, 51450, 25725) from the data set, the average(median) would be : 302.58$
- By not changing the data and calculating the median, the average would be: 284.00$

Question 2: For this question you’ll need to use SQL. 
Follow this link to access the data set required for the challenge. 
Please use queries to answer the following questions. 
Paste your queries along with your final numerical answers below.

How many orders were shipped by Speedy Express in total?
    
    SELECT COUNT (Shippers.ShipperID)
    FROM Shippers
    JOIN Orders ON Orders.ShipperID = Shippers.ShipperID
    WHERE Shippers.ShipperName = "Speedy Express";

**Result** = 54
    
What is the last name of the employee with the most orders?

    SELECT Employees.LastName
    FROM Employees 
    JOIN Orders ON Orders.EmployeeID = Employees.EmployeeID
    GROUP BY Employees.EmployeeID
    ORDER BY COUNT (Employees.EmployeeID) 
    DESC LIMIT 1; 
 
**Result** = Peacock  

  
What product was ordered the most by customers in Germany?

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
   - I want to group together rows that have common values of ProductName so that I don't get multiple values of ProductName. : **GROUP BY Products.ProductName** 
   - Now I want to put the data in Order by sum of the Quantity of orders so that I know which ProductName was ordered the most in descending order. : **ORDER BY SUM(OrderDetails.Quantity) DESC** 
   - Finally, to clean it all up, we want a clear answer and just display the top result. : **LIMIT 1;**
  
    
  console.log(  
    'SELECT Products.ProductName 
    FROM Products  
    JOIN OrderDetails ON OrderDetails.ProductID = Products.ProductId 
    JOIN Orders ON Orders.OrderID = OrderDetails.OrderId 
    JOIN Customers ON Customers.CustomerID = Orders.CustomerId 
    WHERE Customers.Country = "Germany" 
    GROUP BY Products.ProductName  
    ORDER BY SUM(OrderDetails.Quantity) DESC 
    LIMIT 1; ')
    
**Result** = Boston Crab Meat

