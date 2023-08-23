# Introduction to SQL 

## Familiarizing with the graphical user interface (GUI) 

1. Open DBeaver 
2. Help > Create Sample Database
3. Familiarize yourself with the menu (explanation from the instructor will guide you through this)
4. Maximize *Tables* under the tree view panel 
5. Double click on *Employee* table. 
   1. While in the Data view switch from Grid to Text representation 
   2. Look into Entity Relations (ER) Diagram for this table 
   3. Look at Properties view. Which data types are there in the table? 
   4. Repeat these steps for the table of your choosing. 
6. Right-click on the 'DBeaver SAmple Database' > View Diagram. This shows you all the information about entities in the table. 
7. Right-click on the 'DBeaver SAmple Database' > SQL Editor > New SQL Script. 

## Writing Scripts 

While there are many tutorials available online on learning SQL this tutorial will focus on the basics and filtering, as these are more often used in GIS software for filtering spatial features, including for selections based on attributes and definition queries. 

---

### Your first query 

```sql
SELECT 
    * 
FROM
    Employee e; 
```

> What does this query do? 

#### Selecting specific columns

We can specify which columns we want by including their names in the select portion of the query: 

```sql
SELECT 
    firstName, LastName, Title, HireDate
FROM 
    Employee e; 
```

Notice how we can change the order of the columns in the select statement. 

> How many records are selected with this statement? 

#### Sorting records 

We can sort records using 'ORDER BY', where the sort type is denoted with ASC (Ascending) and DESC (descending). We can also use 'LIMIT' clause to limit the number of rows on the screen.  

```sql
SELECT 
    firstName, LastName, Title, HireDate
FROM 
    Employee e
ORDER BY LastName DESC
LIMIT 5; 
```

### Select unique values 

Unique values can be selected with the 'DISTINCT' function. 

```sql
SELECT 
    DISTINCT (City) as unique_cities
FROM 
    customer c;
```

> List unique countries in the customers table and sort by Country

---

### Filtering records

#### Filtering character variables 

The records are filtered using the 'WHERE' clause, followed by logical operators. 

```sql
SELECT 
    * 
FROM 
    customer c
WHERE
    City = 'Stuttgart';
```

> What is the name of the customer residing in Oslo? 

> What is the name of the customer residing in Prague? 

> What is the name of the customer residing in Rome?

> List customers residing in Brazil. 

> List customers residing in Austria. 

> List customers residing in USA. 

#### Filtering numeric variables 

```sql
SELECT 
    *
FROM 
    invoice i
WHERE 
    total > 5; 
```

You can make your filter more complex by adding more filtering conditions 

```sql
SELECT 
    *
FROM 
    invoice i
WHERE 
    total > 5 AND BillingCountry = 'Belgium' 
```

#### Combining several conditions for filtering 

```sql
SELECT 
    *
FROM 
    invoice i
WHERE 
    total > 5 AND 
        (BillingCountry = 'Belgium' OR BillingCountry = 'Netherlands')
```

> Select the records with invoices over 3 for USA and Canada. 

> Repeat for any other two countries. 

---

### Aggregating Data 

SQL allows to use the so-called 'aggregate functions' (AVG, SUM, COUNT, MAX, MIN) to calculate summary statistic for groups of rows / variables. 

```sql
SELECT 
    sum(total)
FROM 
    invoice i; 
```

> Calculate minimal invoice, maximal invoice, average invoice

We can use COUNT function to count the number of records in our selection. Check out the example below: 

```sql
SELECT 
    --count(BillingAddress) --412
	count(DISTINCT(BillingAddress)) --59
FROM 
    invoice i; 
```

> What is the difference between line 2 and line 3 in the query above? 

> Count the number of unique customers in the **invoice** table

---

### Aggregating Data with Groups 

There are many questions in data analytics that require you to generate summaries for groups of objects (e.g. post offices in the State of Iowa, coffee shops in the city of Santa Barbara, average customer spending per restaurant). We can use the GROUP BY function to help with this query. 

```sql
SELECT
    BillingCity, avg(total) as avg_spending_city
FROM
    invoice
GROUP BY 
    BillingCity;
```

In the example above our grouping variable is *BillingCity*. Grouping variables are typically of categorical type. You can use various aggregate functions with GROUP BY. 

> Calculate total invoice and average invoice by the country of billing. 


--- 

### Joining Data 

There are different types of join: left join, right join, outer join, inner join. Imagine we wanted to find the emails of the customers with the highest invoice amounts for our email list and special discounts / offers. However, the invoice table does not have customers' emails. Therefore, we will need to join the customer data to the invoice data. This is easily accomplished using a JOIN operator and ON proposition to specify which fields / columns we would like to join the two tables on. Since both invoice and customer tables share CustomerId key, we can use it for a join. 

```sql
SELECT
    *
FROM 
    invoice i 
LEFT JOIN customer c 
	ON i.CustomerId  = c.CustomerId; 
```

#### Advanced join with aggregate 

If we wanted to get only the top five paying customers we can combine aggregate functions and a join. 

```sql 
SELECT
    c.CustomerId , c.FirstName , c.LastName , c.City , c.Country , sum(i.Total) as sum_customer
FROM 
    invoice i 
LEFT JOIN customer c 
	ON i.CustomerId  = c.CustomerId
GROUP BY i.CustomerId 
ORDER BY SUM(i.Total) DESC
LIMIT 10; 
```

Here are a few things to note on the query above. First, once we have defined alias for a table we can refer to the table with that alias. This is what you see in line 2 after ==SELECT==. c.CustomerId is equivalent to customer.CustomerId. Second, we need to duplicate the sorting rule both in the ORDER BY and SELECT clause.  

> Which genres make the most (least) profit? 

> Which artists make the most profit? 

> Which albums make the most profit? 

--- 

==The answer to the last questions will be posted soon!!!== 🥇