---
title: "SQL vs. NoSQL: Relational vs. Non-Relational Databases"
author: Raffi Khatchadourian (based on "SQL vs NoSQL" from CSCI 40500/77100, Hunter College, Spring 2021)
date: Fall 2026
semester: Fall 2026
lang: en
footer: Based on "SQL vs NoSQL", Software Engineering (CSCI 40500/77100), Hunter College, Spring 2021
license: Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)
---

# SQL vs. NoSQL: Relational vs. Non-Relational Databases

## Agenda

* Definitions of SQL and NoSQL databases.
* Comparison.
* Pros and cons.
* Performance.
* Applications.
* Demo.

## Why Does This Matter?

* Big data and cloud computing need databases that serve a *very* large number of users.
* Distributed data storage is essential for processing large amounts of data.
	* Think of the web apps run by Google, Facebook, Amazon, ...
* The web has changed the requirements for the storage systems of the next generation of applications.

> Q: Which databases have you used in your own projects? Why did you pick them?

## Definitions

### SQL Databases

* *Relational* databases with a standard query language (SQL).

### NoSQL Databases

* NoSQL stands for "*Not Only* SQL."
* *Non-relational*, often *distributed* databases.
* An alternative to SQL aimed at fast access times and no downtime during failures.
* Used by large enterprises like Facebook, Google, and Amazon.

## SQL vs. NoSQL at a Glance

| SQL | NoSQL |
|-----|-------|
| Tables with rows and columns. | Key-value pairs, documents (e.g., JSON), graphs, ... |
| Relational: links information across tables. | Non-relational: no built-in mechanism to link data. |
| Structured query language. | No standard query language. |
| Static schema. | Dynamic schema. |
| Supports ACID transactions. | Traditionally limited ACID support. |

> *Note*: The lines have blurred. Many NoSQL systems now offer query languages and transactions (e.g., MongoDB supports multi-document ACID transactions since version 4.0), and many SQL systems store JSON.

## Example: The Same Data, Two Ways

:::::::::::::: {.columns}
::: {.column width="50%"}

### Relational

**Users**

| ID | First | Last |
|----|-------|------|
| 1 | Shane | Johnson |

**User Skills**

| User ID | Skill |
|---------|-------|
| 1 | Big Data |
| 1 | Java |
| 1 | NoSQL |

**User Experience**

| User ID | Role | Company |
|---------|------|---------|
| 1 | Technical Mktg | Red Hat |
| 1 | Product Mktg | Couchbase |

:::
::: {.column width="50%"}

### Document (JSON)

```json
{
  "firstName": "Shane",
  "lastName": "Johnson",
  "skills": ["Big Data", "Java", "NoSQL"],
  "experience": [
    {
      "role": "Technical Marketing",
      "company": "Red Hat"
    },
    {
      "role": "Product Marketing",
      "company": "Couchbase"
    }
  ]
}
```

:::
::::::::::::::

Example adapted from [Couchbase](https://adtmag.com/articles/2016/06/22/couchbase-4-5.aspx).

> Q: What does each representation make easy? What does each make hard?

## Data Model

* NoSQL databases use many modeling techniques:
	* Key-value pairs.
	* Documents.
	* Graphs.
	* Wide columns.
* A NoSQL system may combine two or more of these models.
* Tables are *not* the storage structure.
* Schema-less, so very efficient at handling *unstructured* data.

## Scalability

### Vertical Scaling (Scale Up)

* Relational databases traditionally scale *vertically*: add more hardware (RAM, CPU, ...) to one machine.
* Costly, and limited by what a single machine can hold.

### Horizontal Scaling (Scale Out)

* NoSQL databases are designed to scale *horizontally*: add more machines.
* Traditional SQL databases are not built around this model.

> Q: What new problems appear once data is spread across many machines?

## Cloud

* Relational databases are less suited to cloud environments.
	* Hard to scale beyond a limit.
	* Weak support for full-text content search.
* NoSQL databases fit cloud databases well.
	* Their defining characteristics (distribution, flexible schema, horizontal scaling) are exactly what cloud databases need.

## Big Data Handling

* Big data is an issue for relational databases.
	* The solution is scaling and distributing data, either vertically or horizontally.
* Horizontal scaling means *partitioning* data across multiple servers.
	* Adds complexity and performance costs to these operations.
* NoSQL databases are *designed* for big data.
	* They implement methods to improve the performance of storing and retrieving data at scale.

## Complexity

* In relational databases, users must convert data into tables.
* When data does not fit those tables, the database structure can become complex, difficult, and slow to process.
* NoSQL databases can store data that is:
	* Unstructured,
	* Semi-structured, or
	* Structured.

## NoSQL and Agile Development

* NoSQL is often a good fit for Agile development.
* Dynamic schemas and scalability go hand in hand with the Agile approach.
* Incremental, feature-driven development benefits from being able to choose or change the data model *during* development.
* Flexible handling of unstructured data accommodates changing specifications.
	* Agile avoids strict, detailed up-front planning of the product.
* Makes it easier to expand the product's functionality to reach new users.

> Q: What is the risk of a schema that can change at any time? Who enforces the structure instead?

## Performance

* Performance of relational databases can degrade with very large datasets.
* NoSQL was developed to overcome these performance issues.
* NoSQL provides high scalability, but historically lacked a standard query language.
	* One reason it lags behind SQL in number of users.

## Applications

NoSQL databases are well suited for:

* Business applications with continuously growing unstructured data.
* Applications accessed by a large number of users.
	* E.g., e-commerce websites.
* Websites with huge and growing data.
	* E.g., social media networks (millions of posts daily).

> Q: When would you still choose a relational database?

## Demo

### SQL

```sql
CREATE TABLE users (id INT PRIMARY KEY, first VARCHAR(50), last VARCHAR(50));
CREATE TABLE user_skills (user_id INT REFERENCES users(id), skill VARCHAR(50));

INSERT INTO users VALUES (1, 'Shane', 'Johnson');
INSERT INTO user_skills VALUES (1, 'Big Data'), (1, 'Java'), (1, 'NoSQL');

SELECT u.first, u.last
FROM users u JOIN user_skills s ON u.id = s.user_id
WHERE s.skill = 'Java';
```

### NoSQL (MongoDB)

```javascript
db.users.insertOne({
  firstName: "Shane",
  lastName: "Johnson",
  skills: ["Big Data", "Java", "NoSQL"]
});

db.users.find({ skills: "Java" }, { firstName: 1, lastName: 1 });
```

## Summary

* NoSQL does not use the relational data model, and thus not SQL as its primary language.
* NoSQL stores large volumes of data.
* In distributed environments (data spread across machines), NoSQL is designed to keep working.
	* A fault or failure on one machine need not interrupt the service.
* Many NoSQL databases are open source and free to use.
* NoSQL stores records without a fixed schema.
* NoSQL traditionally relaxes ACID properties.
* NoSQL scales horizontally, so performance grows roughly linearly with machines.

## References

1. Dave, M. (2012). SQL and NoSQL Databases. *International Journal of Advanced Research in Computer Science and Software Engineering*.
1. Mohamed, M., Altrafi, O., & Ismail, M. (2014). Relational vs. NoSQL Databases: A Survey. *International Journal of Computer and Information Technology (IJCIT)*, 3, 598.
1. Nayak, A., Poriya, A., & Poojary, D. (2013). Type of NoSQL Databases and Its Comparison with Relational Databases. *International Journal of Applied Information Systems*, 5, 16–19.
1. Padhy, R. P., Patra, M. R., & Satapathy, S. C. (2011). RDBMS to NoSQL: Reviewing Some Next-Generation Non-Relational Database's. *International Journal of Advanced Engineering Sciences and Technologies*, 11(1), 15–30.
1. [MongoDB](https://www.mongodb.com/).
1. [Why NoSQL Is the Perfect Fit for Agile Development](https://www.dragonspears.com/blog/why-nosql-is-the-perfect-fit-for-agile-development). DragonSpears.
