---
title: "SQL vs. NoSQL: Relational vs. Non-Relational Databases"
author: Raffi Khatchadourian (based on a "SQL vs NoSQL" student presentation from CSCI 40500/77100, City University of New York (CUNY) Hunter College, Spring 2021)
date: October 7, 2026
semester: Fall 2026
lang: en
footer: Based on a "SQL vs NoSQL" student presentation from Software Engineering (CSCI 40500/77100), City University of New York (CUNY) Hunter College, Spring 2021
license: Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)
---

# SQL vs. NoSQL: Relational vs. Non-Relational Databases

## Agenda

* Definitions of SQL and NoSQL databases.
* Comparison.
* Pros and cons.
* Performance.
* Applications.
* Choosing a database for your project.

## Why Does This Matter?

* Big data and cloud computing need databases that serve a *very* large number of users.
* Distributed data storage is essential for processing large amounts of data.
	* Think of the web apps run by Google, Meta, Amazon, ...
* The web has changed the requirements for the storage systems of the next generation of applications.

> Q: Which databases have you used in your own projects? Why did you pick them?

::: notes
Open-ended. Typical answers: SQLite or PostgreSQL (came with the framework, e.g., Django/Rails), MySQL (hosting default), MongoDB (JSON fits JavaScript stacks), Firebase/Firestore (no backend to run). Point out that most choices are driven by familiarity and tooling, not by data shape or scale; the rest of the lecture gives better criteria.
:::

## Definitions

### SQL Databases

* *Relational* databases with a standard query language (SQL).

### NoSQL Databases

* NoSQL stands for "*Not Only* SQL."
* *Non-relational*, often *distributed* databases.
* An alternative to SQL aimed at fast access times and no downtime during failures.
* Used by large enterprises like Meta, Google, and Amazon.

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

![Relational tables vs. a JSON document](graphics/relational-vs-document.svg){title="Adapted from Couchbase."}

Example adapted from [Couchbase](https://adtmag.com/articles/2016/06/22/couchbase-4-5.aspx).

> Q: What does each representation make easy? What does each make hard?

::: notes
- Relational, easy: no duplication (each fact stored once), so updates are simple and consistent; ad hoc queries across entities (e.g., "all users who know Java" or "all companies"); constraints and foreign keys enforce integrity.
- Relational, hard: reading one user means joining three tables; changing structure needs a schema migration.
- Document, easy: reading or writing one user is a single lookup with no joins; the document maps directly to objects in code; adding a field needs no migration; easy to shard by user.
- Document, hard: queries across documents (e.g., "everyone who worked at Red Hat") need extra indexes or scans; duplicated data (e.g., a company's name in many documents) must be updated everywhere; no enforced structure.
:::

## Data Model

* NoSQL databases use many modeling techniques:
	* Key-value pairs (e.g., Redis, Amazon DynamoDB).
	* Documents (e.g., MongoDB, Couchbase).
	* Graphs (e.g., Neo4j).
	* Wide columns (e.g., Google Bigtable, Apache Cassandra).
* A NoSQL system may combine two or more of these models.
* Not *relational* tables: no fixed columns or joins across tables.
* Schema-less, so very efficient at handling *unstructured* data.

## Scalability

### Vertical Scaling (Scale Up)

* Relational databases traditionally scale *vertically*: add more hardware (RAM, CPU, ...) to one machine.
* Costly, and limited by what a single machine can hold.

### Horizontal Scaling (Scale Out)

* NoSQL databases are designed to scale *horizontally*: add more machines.
* Traditional SQL databases are not built around this model.

> *Note*: Relational databases can also scale out, e.g., via sharding (Vitess for MySQL, Citus for PostgreSQL) or distributed SQL databases (Google Spanner, CockroachDB). It is harder than with NoSQL, not impossible.

> Q: What new problems appear once data is spread across many machines?

::: notes
- Partitioning: choosing a shard key; hot spots; rebalancing when adding machines.
- Queries across machines: cross-shard joins and aggregations are slow, hence denormalization (as in the JSON example).
- Transactions: atomic updates across machines need coordination (e.g., two-phase commit), which is slow and blocks on failures.
- Replication and consistency: replicas lag, so reads may be stale (eventual consistency); concurrent writes can conflict.
- Partial failures and the CAP theorem: during a network partition, choose consistency or availability, not both.
- Time and ordering: clocks disagree, so "which write was last?" is hard.
- Operations: monitoring, backups, upgrades, and debugging get harder.
- Tie-back: these are the costs of scaling out. NoSQL accepts some (weaker consistency, no joins); distributed SQL (e.g., Spanner) pays to solve them.
:::

## Cloud

* Relational databases were traditionally less suited to cloud environments.
	* Hard to grow and shrink capacity on demand.
* NoSQL databases fit cloud databases well.
	* Their defining characteristics (distribution, flexible schema, horizontal scaling) are exactly what cloud databases need.

> *Note*: Today, managed relational services (e.g., Amazon RDS and Aurora, Google Cloud SQL, Azure SQL Database) are among the most widely used cloud databases. The provider handles replication, failover, backups, and resizing, and some (e.g., Aurora Serverless) grow and shrink capacity on demand.

## Big Data Handling

* Scaling relational databases to big data takes extra effort.
	* The solution is scaling and distributing data, either vertically or horizontally.
* Horizontal scaling means *partitioning* data across multiple servers.
	* Adds complexity and performance costs to these operations.
* NoSQL databases are *designed* for big data.
	* They implement methods to improve the performance of storing and retrieving data at scale.

## Complexity

* In relational databases, users must convert data into tables.
* When data does not fit those tables, the database structure can become complex, difficult, and slow to process.
* Code works with objects, not tables (the *object-relational impedance mismatch*).
	* Object-Relational Mapping (ORM) libraries (e.g., Hibernate, Django ORM, SQLAlchemy) bridge the gap.
	* Document databases reduce the mismatch: a document maps directly to an object.
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

::: notes
- Risks: inconsistent records (e.g., `firstName` vs. `first_name`, a field missing or of a different type in old documents); typos silently create new fields; code must handle every historical shape of the data; bugs show up at read time rather than write time.
- Who enforces it: the application code ("schema-on-read"). Common tools: ORM/ODM models (e.g., Mongoose), validation libraries, optional database-side validation (e.g., MongoDB JSON Schema validation), tests, and migration scripts that rewrite old documents.
- Takeaway: there is always a schema; the question is whether the database or the application enforces it.
:::

## Performance

* Performance of relational databases can degrade with very large datasets unless partitioned or sharded.
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

::: notes
- Data is structured and highly related, with many-to-many relationships and frequent joins.
- Strong consistency and multi-row transactions are required (e.g., banking, inventory, orders).
- Integrity must be enforced by the database (constraints, foreign keys).
- Many ad hoc queries, reporting, or analytics, where SQL shines.
- Data fits on one machine, or can be scaled with replicas or sharding; that covers most applications.
- The team knows SQL and the tooling is mature.
- Reasonable default: start relational and add NoSQL for specific needs (e.g., caching, search, very high write volume).
:::

## Choosing a Database for Your Project

* Start with the kind of data your application processes:
	* Tabular, related data (users, orders, enrollments, ...) → relational database (e.g., PostgreSQL, or SQLite for small projects).
	* Semi-structured records whose fields vary (e.g., JSON) → consider a document database (e.g., MongoDB).
	* Highly connected data queried by its relationships (e.g., social networks) → consider a graph database (e.g., Neo4j).
* Then check how you will query it:
	* Joins, transactions, or ad hoc queries → lean relational.
	* Whole records by key at very high scale → lean NoSQL.
* When unsure, start relational; mixing is common (e.g., PostgreSQL plus Redis for caching).

> Q: What kind of data will your project process? Which database would you choose? Why?

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
