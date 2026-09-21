# Question 1: Define Data Quality in the context of ETL pipelines. Why is it more than just data cleaning?
## Answer:
Data Quality in an ETL pipeline means making sure that the data is correct, complete, consistent,valid, and suitable for its intended use.
Data quality is more than just data cleaning because cleaning mainly focuses on fixing problems such as missing values, duplicates, or incorrect formats. Data quality also includes checking
whether the data follows business rules, has the correct relationships, and is accurate enough to be used for reporting and decision-making.
For example, removing duplicate records is data cleaning, while checking whether a transaction belongs to a valid customer is also part of data quality and validation.

# Question 2: Explain why poor data quality leads to misleading dashboards and incorrect decisions.
## Answer:

Dashboards and reports are only as reliable as the data used to create them. If the input data contains missing values, duplicate records, incorrect amounts, or wrong customer information,
the final dashboard can show incorrect results.
For example, if the same sales transaction is stored three times, total sales will appear higher than the actual amount. Similarly, if some customer records are missing, customer-related reports may
not show the complete picture.
Because of this, poor data quality can lead to wrong analysis, misleading dashboards, and incorrect business decisions.

# Question 3: What is duplicate data? Explain three causes in ETL pipelines.
## Answer:

Duplicate data means the same record or transaction appears more than once in a dataset.
Three common causes of duplicates in ETL pipelines are:
1. Repeated data from the source:
The source system may send the same record more than once.
2. ETL process errors:
A problem in the extraction or loading process may cause the same data to be inserted multiple
times.
3. Incorrect merge or join operations:
If tables are joined incorrectly, one record can match multiple records and create duplicate rows. Therefore, duplicate data should be identified and handled before it affects reports and analysis.

# Question 4: Differentiate between exact, partial, and fuzzy duplicates.
## Answer:

Exact duplicates:
These are records where all important values are exactly the same. For example, the same
Customer_ID, Product_ID, date, and transaction amount appear more than once.
Partial duplicates:
These records have some values in common but differ in one or more fields. For example, the
customer and product may be the same, but the quantity or amount may be different.
Fuzzy duplicates:
These records are not exactly the same but are very similar. They may have small spelling or
formatting differences. For example, “Rahul Mehta” and “Rahul Mehata” could refer to the same person.
In simple terms, exact duplicates are completely identical, partial duplicates match in some
fields, and fuzzy duplicates are similar but not exactly the same.

# Question 5: Why should data validation be performed during transformation rather than after loading?
## Answer:

Data validation should be performed during transformation because problems can be identified and corrected before the bad data reaches the target system.
If validation is done only after loading, incorrect data may already be stored in the database and may have affected reports or other processes.
For example, while transforming a transaction, we can check whether the transaction amount is valid, whether the Customer_ID exists, and whether the date is in the correct format.
Validating during transformation helps keep the final dataset cleaner and prevents errors from moving further through the ETL pipeline.

# Question 6: Explain how business rules help in validating data accuracy. Give an example.
## Answer:

Business rules are conditions that define what is considered valid and correct data for a particular business.
They help in checking whether the data makes sense from a business point of view, not just whether the data has the correct format.
For example, a business rule may state:Transaction amount must be greater than or equal to zero.
If a transaction has an amount of -500, it should be flagged as invalid because a negative sales amount may not be allowed under that business rule.
So, business rules help ensure that the data is not only technically correct but also meaningful for the business.

## Practical Questions

The assignment provides a sales transaction dataset containing fields such as Txn_ID,
Customer_ID, Name, Product_ID, Quantity, Txn Amount, Txn_Date, and City.
The provided records include repeated C101/P11 transactions and transactions for C105 and
C106, which are relevant to the SQL questions.

# Question 7: Write an SQL query to list all duplicate keys and their counts using the business key (Customer_ID + Product_ID + Txn_Date + Txn_Amount).
## Answer:

The business key consists of:
Customer_ID + Product_ID + Txn_Date + Txn_Amount
The SQL query is:
SELECT
 Customer_ID,
 Product_ID,
 Txn_Date,
 Txn_Amount,
 COUNT(*) AS duplicate_count
FROM Sales_Transactions
GROUP BY
 Customer_ID,
 Product_ID,
 Txn_Date,
 Txn_Amount
HAVING COUNT(*) > 1;
This query groups records using the complete business key and only shows those combinations
that occur more than once.
Result from the given dataset
The combination:
Customer_ID = C101
Product_ID = P11
Txn_Date = 2025-12-01
Txn_Amount = 4000
appears 3 times in the supplied data.
So the duplicate count is:
C101 | P11 | 2025-12-01 | 4000 | 3

# Question 8: Enforcing Referential Integrity Assume the following tables: Sales_Transactions and Customers_Master. Identify the Sales_Transactions.Customer_ID values that violate referential integrity when joined with Customers_Master and write a query to detect such violations.
## Answer:

Referential integrity means that a Customer_ID present in the Sales_Transactions table
should also exist in the Customers_Master table.
The Customers_Master table contains these customer IDs:
C101
C102
C103
C104
The sales data also contains:
C105
C106
These two customer IDs are not present in the master table, so they violate referential integrity.
SQL query
SELECT DISTINCT s.Customer_ID
FROM Sales_Transactions s
LEFT JOIN Customers_Master c
 ON s.Customer_ID = c.CustomerID
WHERE c.CustomerID IS NULL;
Expected result
C105
C106
This query uses a LEFT JOIN so that all customer IDs from Sales_Transactions are checked
against Customers_Master. When no matching customer is found in the master table, the value
appears as a referential integrity violation.
