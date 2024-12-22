---
title: "Prisma or SQL: Which Do You Choose?"
# summary: Take full control of your personal brand and privacy by migrating away from the big tech platforms!
date: 2024-04-01

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: "post image"

authors:
  - admin

tags:
  - SQL
  - Prisma
  - ORM
  - Database
  - TypeScript
---

### Introduction

Every developer has at some point developed some application with some dummy data. The beginning stages of the learning journey are usually more focused on getting the projects to work, and there is not much need to use real data to achieve that. However, when it comes to real-world projects, databases are not to be taken for granted. E-commerce websites or educational portals; the information displayed from the front-end has to be consumed somewhere, and the new data made by the users has to be stored in a place.

There are multiple ways to retrieve and insert data into databases, but a very popular and generalized way for relational databases is through SQL (Structured Query Language). Prisma, on the other hand, is an object-relational mapping tool that provides an abstraction for interacting with the database, thereby providing a way to do it in the language of choice. What factors should then be considered when deciding between SQL and Prisma?

### Understanding SQL

**Traditional SQL** databases are databases that are relational, which means that data is organized into tables. A database can contain one or more tables. Those tables can contain columns and rows, and the data in those rows can be of different types accepted by the database. Those tables can also be related to one another, and that relationship is established through keys.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707049286979/6576cc1a-0769-4ef6-ac6f-9329997cbd65.png align="center")

The language used for querying and manipulating the database is called SQL and it is a long-standing and popular language, An example of how it can be used to retrieve all users from a table is

```sql
SELECT * FROM users;
```

### Understanding ORM

**Object Relational Mapping (ORM)** tools are frameworks that aim to bridge the gap between relational databases and the OOP (Object Oriented Programming) model. These tools act as a wrapper around a programming language and provide a higher level of abstraction for developers to interact with database entities as if they were regular objects in that programming language. For these reasons, they usually work best for programming languages that have support for object-oriented programming. Some examples of these ORM tools are Hibernate (Java), Django ORM (Python), and Prisma (JavaScript/TypeScript).

```typescript
const users = await prisma.user.findMany();
```

### Prisma vs SQL

Prisma is a powerful ORM that is commonly used in Node.js environments and can be used with both JavaScript and Typescript. Data queries and manipulations can be made through Prisma in Typescript, and Prisma in turn converts those into their SQL equivalent.

Some of the **advantages** of using Prisma are

1. **Cross-database compatibility**: Since Prisma supports a wide range of popular databases by many DBMS (Database Management System), including PostgreSQL, CockroachDB, SQLite, MySQL, and even MongoDB, there is a wide range of options to choose from. Prisma code written for retrieving data from a Microsoft SQL Server should usually just work fine when the project switches to using a PostgreSQL database
2. **Automated Migrations**: Prisma manages changes to the schema by generating migration scripts; this means that you do not need to write complex SQL queries in order to update the database structure. A database can be built entirely using the prisma migrations.
3. **Type Safety**: My favorite advantage of using Prisma is that it is type-safe (types are generated for the objects created for the database entities). This means that potential errors can be caught very early in the development phase and can help to write very reliable and robust applications
4. **Developer Experience**: Having the ability to query and manipulate a database in the same language as the application is written in comes as a plus, as opposed to having to learn a separate language (SQL) to interact with the database.

Some of the **disadvantages** of using Prisma include:

1. **Limited query capabilities**: Prisma does a conversion of object-oriented code into SQL, and it supports a wide range of queries. However, it would be impossible for it to cover every possible use case. Cases where heavily customized and intricate SQL queries are needed are not the best for using ORM tools. Prisma supports writing pure SQL queries as a workaround for this limitation, as shown in the example below.
2. ```typescript
   const result = await prisma.$queryRaw`
     SELECT * FROM users WHERE email = ${email}
   `;
   ```
3. **Performance Overhead**: This is usually negligible and not a cause for concern in most cases, but the overhead introduced by Prisma's abstraction layer compared to writing pure SQL might present an issue in some performance-critical applications
4. **Learning curve**: For teams that are using pure SQL or another ORM tool and are being newly introduced to Prisma, additional learning would be required just as with any other new technology.
5. **Customization**: Technologies that provide an abstraction layer over another one trade off customization for ease of use. This means that Prisma is not as easily customizable as pure SQL can be, and that might present some challenges in rare cases.

### Wrapping Up

Deciding whether to use SQL or Prisma for a project is usually a question that depends on the requirements of that project. My personal advice would be that in cases where there are type safety concerns, increased development complexities, and potential for code duplication, then Prisma or any other ORM would be best for this use case. However, in projects where full control and customization are needed and where performance is a very critical metric, then using pure SQL might be the best choice.

It is also important to remember that, despite Prisma's strong support at the moment, ORM tools tend to dwindle in popularity and be replaced by other ORM tools in terms of support and usage. This suggests that it might be wise to invest some time in learning the fundamentals of SQL. Even if you primarily use Prisma or other ORMs, having a solid understanding of SQL will give you more flexibility and make you less reliant on specific tools that could potentially become outdated or unsupported (a developer's nightmare) in the future.
