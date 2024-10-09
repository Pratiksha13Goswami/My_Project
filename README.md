

# Nexa Insights Sales Performance

### Dashboard Link : "C:\nexa_insights_sales_performance.pbix"
Objective: 

The primary objective of this project is to analyze sales data to identify trends improve profitability and provide insights on key business performance metrics The tools I have used in this project is Excel , SQL  ,and Power BI.


Data Sources and Tools Used

Analysis using Excel:

 In Excel, I perform initial data cleaning and validation to remove duplicates, handle missing values and ensure the problem is formatted correctly.

Challenges faced:

while cleaning data in Excel I faced an issue while extracting the pin code from the address column, then I used “Flashfill” to solve this issue, after using “FlashFill” there was one error, it also took out some address column characters with the pin code.

so I used the left function and deleted some values manually These steps saved my time and effort and also performed accuracy by quickly splitting and organising complex address data.

The dates were not in the proper format, and there was a considerable difference between the meeting and calling dates. It was corrected since the calling date should be earlier than the meeting date. 
• The main challenge was structuring the data into the proper format, which required significant time and effort.
 • There were many spelling mistakes throughout the meeting data, which were corrected as required


Analysis using SQL:

Sql plays a crucial role in my project for handling large data sets ,performing queries and aggregating data ,to generate insights .
I used it to calculate key cells matrix ,such as total sales, profit, and performance by regional or business category. it used to extract ,transform and analyse data, from four different tables,such as Business_Info,Business_Category,Sales_data,Meeting_data , sql allowed me to filter records, and segments data based on specific conditions.

For Example:

I queried sales data for only those clients who had more than one meeting 

Challenges faced:

During Cleaning the data in sql, I Faced data loading errors in sql and faced various types of errors such as duplicate records and foreign key constant validation. to handle duplicate entries I used Excel to remove duplicates and ensure that the sales total should be accurate .

we can also handle these errors in sql ,by using distinct keyword, I ensure that values of foreign key in the child class and the primary key in the parent class must matched ,to avoid foreign key constraint violation.

Importing tables into MySQL created a problem as Table 1 had duplicate Business_ID values due to many instances of the same Business_Name.
 So, after assigning the primary key, it gave a primary key-foreign key constraint failure error. To resolve this, duplicates were removed from Table 1 based on the Business_Category, and then it was imported into MySQL.

For Again Further Calculation Term, I  have to create “Expense” and “Profit” column which is 60% of the “Net sales amount “Respectively so again using formulas and created Two Columns as required, and  the “GST” is 18% of the sales amount in table 4 , so we created and imputed values using formulas. These four tables were created in SQL using this format. Thus, the data is ready to import into Power Bi.

I have performed various queries ,to analyze data :

  
1. What is the demographic profile of the clients and how does it vary across districts?
   
    SELECT b. City, b.State, COUNT(DISTINCT b.Business_ID) AS num_clients

    FROM Business_info1 b

    GROUP BY b.City, b.State

    ORDER BY num_clients DESC;

![Snap_1]![1 image](https://github.com/user-attachments/assets/f086ba3c-0aea-42db-8954-ad275a92f21a)
thus we get to know that Client is more from Nagpur ,Bhopal from Madhya Pradesh and Chandrapur from Maharashtra.

2. How the Biz have performed over the years. Give their detailed analysis year & month-wise.
    SELECT 
    YEAR(STR_TO_DATE(Login_date, '%Y-%m-%d')) AS Year,

    MONTH(STR_TO_DATE(Login_date, '%Y-%m-%d')) AS Month,

    SUM(sales_Amount)  AS TotalSales,

    SUM(GST_Amount ) AS TotalGST, SUM(NetAmount ) As 
    TotalNetAmount,

    sum(Profit ) As TotalProfit 

    FROM sales_data

    GROUP BY YEAR(STR_TO_DATE(Login_date, '%Y-%m-%d')), MONTH(STR_TO_DATE(Login_date, '%Y-%m-%d'))

    ORDER BY Year, Month;


![2](https://github.com/user-attachments/assets/540aa6ad-20b6-49c8-8daf-ea74560c121a)
        
 3. What are the most common types of clients and how do they differ in terms of usage and profitability?

    SELECT 

    BC.Business_Category,

    COUNT(DISTINCT BC.Business_ID) AS NumberOfClients,

    SUM(SD.sales_Amount) AS TotalSales,

    SUM(SD.NetAmount) AS TotalNetAmount,

    SUM(SD.Profit) AS TotalProfit

    FROM Business_Category1 BC

JOIN Sales_data SD ON BC.Business_ID = SD.Business_ID

GROUP BY BC.Business_Category

ORDER BY NumberOfClients DESC;


 ![3](https://github.com/user-attachments/assets/5f24ca8e-d622-41c6-bc16-c2b6cb449ea9)

 Most Common type of client is from Hospital, Clinic and School .

 
 4. Which types of products are most frequently used by the clients and what is the overall profitability of the client need?

    SELECT 

    BC.product_proposal,  -- Assuming product_proposal is in sales_data

    COUNT(*) AS Frequency,

    SUM(SD.sales_Amount) AS TotalSales,

    SUM(SD.sales_Amount - SD. GST_Amount) AS NetAmount,

    SUM(0.60 * (SD.sales_Amount - SD. GST_Amount)) AS TotalProfit

    FROM  sales_data SD

    JOIN  Business_Category1 BC ON SD.Business_Id = BC.Business_Id

    GROUP BY  BC.product_proposal

    ORDER BY Frequency DESC;

    ![4](https://github.com/user-attachments/assets/0091353e-725f-472f-b6c7-e6ac9624478b)

 
Gmvt product is most used by client having highest usage and most profitable product followed by Gmvt+Fb.

 
 
 5. What are the major expenses of the Biz and how can they be reduced to improve profitability?

SELECT

bi.Business_Name,

SUM(td.Expenses) AS Total_Expenses,

SUM(td.profit) AS Total_Profits

FROM Business_info1 bi

JOIN sales_data td ON bi.Business_ID = td.Business_ID

GROUP BY bi.Business_Name

ORDER BY Total_Expenses DESC;
![5](https://github.com/user-attachments/assets/1b6e7ffc-2e85-4ab6-ab98-8d03ca373829)
 
Major Expense is from Ayushman Hospital, Maitreya Developers, Farme, we should focus on Meeting the Confirmation of Customers and should reduce costs by searching for alternatives in the market.

6. What is the client portfolio and how does it vary across different purposes and client segments?

    SELECT 

    B.Business_Name AS Business_Name,  

    BC.Business_Category AS Purpose, 

    COUNT(*) AS NumberOfTransactions, 

    SUM(SD.Sales_Amount) AS TotalRevenue   

FROM  Business_info1 B

JOIN  Business_Category1 BC ON B.Business_Id = BC.Business_Id

JOIN   Sales_Data SD ON B.Business_Id = SD.Business_Id

GROUP BY  B.Business_Name, BC.Business_Category

ORDER BY B.Business_Name, TotalRevenue DESC;

![6](https://github.com/user-attachments/assets/39fef7e2-bb81-4695-a947-d098556ff815)
 
Maximum Client is of 114 Nutriyant having purpose as Nutrition centre  followed by 3M car care then enterprises.

 
7. How are telecallers role in the sales.  

    select 

    MD.Telecaller_name,

    SUM(SD.sales_Amount) AS total_sales

FROM meeting_data1 MD

JOIN sales_data SD ON MD.Business_ID = SD.Business_ID

GROUP BY MD.Telecaller_name;

![7](https://github.com/user-attachments/assets/85201850-68f1-4a4b-a1cb-b51d8d6114ab)
 
Mayuri makes most of profit followed by Dolly and Pawan.

8. What is BDM's indivisual performance with various segments of client.  

    SELECT 

    MD.BDM_name,

    BC.Business_Category,

    SUM(SD.sales_Amount) AS TotalSales

FROM meeting_data1 MD

JOIN Business_info1 BI ON MD.Business_Id = BI.Business_Id

JOIN sales_data SD ON BI.Business_Id = SD.Business_Id

JOIN Business_Category1 BC ON BI.Business_Id = BC.Business_Id

WHERE MD.BDM_name IS NOT NULL AND MD.BDM_name != ''

GROUP BY MD.BDM_name, BC.Business_Category

ORDER BY MD.BDM_name, TotalSales DESC;

![8](https://github.com/user-attachments/assets/b1a99ada-6e59-4649-9cee-af2901013eb9)
 
Pranali has most number of sales and makes most sales in Grocery Dealer  same as Aashutosh sir in Institute and Abhinay in Music Academy.

9. How many businesses retain with same or different product. 

    SELECT COUNT(b.Business_ID) AS num_businesses, p.product_proposal

    FROM Business_info1 b

    JOIN Business_Category1 p ON b.Business_ID = p.Business_ID

    GROUP BY p.product_proposal;

    ![9](https://github.com/user-attachments/assets/d9348f43-6c66-41c7-b40a-0596d3a79580)
 
Most of business is retained by GMVT product followed by Gmvt+Social Media.

10. Which is best selling product and category.  

    SELECT 

    BC.Business_Category,

    product_proposal,

    SUM(SD.sales_Amount) AS TotalSales

FROM  sales_data SD

JOIN Business_info1 BI ON SD.Business_Id = BI.Business_Id

JOIN Business_Category1 BC ON BI.Business_Id = BC.Business_Id

GROUP BY  BC.Business_Category,product_proposal

ORDER BY  TotalSales DESC;

![10](https://github.com/user-attachments/assets/8e5dcf4d-f62b-48d6-866b-e99aefdb1ba7)
 
Social Media Management is best selling product followed by GMVT.    

 11. What is popular selling amount.

     SELECT 

     SD.sales_Amount, 

     COUNT(*) AS Popular_selling_amount

     FROM sales_data SD

     GROUP BY SD.sales_Amount

     ORDER BY Popular_selling_amount DESC;

     ![11](https://github.com/user-attachments/assets/5245ee4d-1a02-4886-aeed-604eec0c821e)
 
Popular selling Amount is of 10000 which is 412.

12. just find out city less than 10 business  ------ meeting info  

    SELECT City, COUNT(Business_ID) AS Total_Meetings

    FROM Business_info1

    GROUP BY City

    HAVING COUNT(Business_ID) < 10;

    ![12](https://github.com/user-attachments/assets/1980b3b0-c539-4141-8022-40d1b57d2a8f)
 

As Indore has less than 10 business meetings which is 3, followed by chindwada and Pune.

Analysts using Power bI:

Power BI is a cloud based business intelligence tool ,used to create reports and dashboards. I load data in power BI, and while loading the data, I first transform the data in power query editor, and delete some unwanted columns from each individual table .
After transforming the data I load the data in power bi ,after that I build relationships between four tables such as business_info, meeting_data, sales_data and business category.

this allowed me to perform cross table analysis such as comparing meeting frequency with sales outcomes.

 I have used different types of visuals such as bar charts, line charts, Pie charts , and slicers.

•	I choose bar chart and line chart, because they provide clear comparisons and trend analysis,which are crucial for sales.

•	 pie charts are helpful for showing proportional distribution of categories ,I avoided using more complex visuals like ribbon chart or waterfall chart ,as they were not necessary for Straight  forward analysis in the project.

•	I used slicers to make the data interactive by allowing users to filter the visuals by region and product category.

Thus after this solving required Problem Statement.

1.What is the demographic profile of the clients and how does it vary across districts?

![powerbi 1](https://github.com/user-attachments/assets/3aba0c04-b5af-47f2-bc97-b51201cc860c)
 
Thus, here we get to Know Most of Clients are from Nagpur, Aurangabad and Pune.

2. How the Biz have performed over the years. Give their detailed analysis year & month-wise.

![2 p](https://github.com/user-attachments/assets/57be8b7f-0753-4d32-a91a-eff331b46fd4)

 
Thus Biz had the most sales in 2019.

3. What are the most common types of clients and how do they differ in terms of usage and profitability?

![3p](https://github.com/user-attachments/assets/a3fcf54b-1b4c-45e6-9bdc-b9c3ab69c4b4)
 
Most of the common types of Clients are from clinics, Hospitals and dental clinics.

4.Which types of product are most frequently used by the clients and what is the overall profitability of the client need?
![4p](https://github.com/user-attachments/assets/effa07bf-4e47-485b-b152-7493cfc09363)
 
The most frequently used products are Gmvt, Gmvt+fb.

5. What are the major expenses of the Biz and how can they be reduced to improve profitability?

![5p](https://github.com/user-attachments/assets/cf676b83-a578-49a1-9018-917a20440b96)
 
Major Expenses are from Ayushman Hospital, Maitreya Developers, Farme so we need to focus on this Business to find out why expenses are higher in these Businesses.

6.What is the client portfolio and how does it vary across different purposes and client segments?
![6p](https://github.com/user-attachments/assets/26edd40e-3627-4557-b42d-99af3298700e)
 
Thus we get to Know that Clinics, Hospitals and Dental Clinics have the most profit in the category.

7. How are telecaller's role in the sales.
![7p](https://github.com/user-attachments/assets/d5aeee54-9fbb-4905-b7d4-7d0d547c1625)
 
Mayuri has scheduled the maximum number of meetings followed by Shital and Gaurav.

8.What is BDM's individual performance with various segments?
![8p](https://github.com/user-attachments/assets/0d7c469f-062b-4362-8d55-1b4b14bfa869)
 
Dupendra is the most effective Business Development Manager followed by Gaurav and Vikrant.

9. How many businesses retain with same or different product?
![9p](https://github.com/user-attachments/assets/93bc5bec-3c56-4290-8c6f-c3fd088a1b7c)
 
Saarthi Education, and Sahara Nursing,  have different retention products and K2 Club & lunge, Sagar Land, and Newel Computer Education, businesses have of same product retention.

10.Which is best selling products and category ?
![10p](https://github.com/user-attachments/assets/df92839d-dad2-4cea-b0db-9f6dee6a1131)
  
GMVT is most Selling Product and in Category Hospital is Trending one.

11.What is popular selling amount?
![11p](https://github.com/user-attachments/assets/1d4ddab2-637a-4a26-b9c8-e4d93744eb33)
 
10000 is most popular selling amount.

12. just find out city less than 10 business  ------ meeting info 
 ![12p](https://github.com/user-attachments/assets/61c39340-c579-4bef-b174-add1ece432f5)         
 
Nagpur has less than 10 Business meetings followed by Bhopal and Chandrapur.



