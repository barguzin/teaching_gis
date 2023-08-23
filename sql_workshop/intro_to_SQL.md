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

While there are many tutorial available online on learning SQL this tutorial will focus on the basics and filtering, as these are more often used in GIS software for filtering spatial features, including for selections based on attributes and definition queries. 

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

Or even more complex: 

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