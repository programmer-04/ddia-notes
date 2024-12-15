---
author: Chinmay Mandke
pubDatetime: 2024-12-15T10:25:29.232Z
# modDatetime: 2023-12-21T09:12:47.400Z
title: Designing Data-Intensive Applications - 2. Data Models and Query Languages
slug: ddia-data-models-and-query-languages
featured: true
draft: false
tags:
  - ddia
  - notes
description: Understanding how to structure and interpret data.
---

## Table Of Contents

## Driving Forces Behind NoSQL Databases

NoSQL databases emerged due to several key motivations:

- Greater scalability beyond relational database capabilities, requiring high throughput
- Preference for free and open-source software
- Support for specialized query operations not well-supported by relational model
- Desire for more dynamic and flexible data models than restrictive SQL schemas

## Relational vs. Document Databases

### Relational Model Characteristics

- Hides implementation details behind a clean interface
- Generalizes well across diverse computing purposes
- Uses normalized tables with foreign key references
- Provides robust *support for joins and complex relationships*

### Some relational databases

- Oracle
- IBM DB2
- Microsoft SQL Server
- PostgreSQL
- MySQL

### Document Model Characteristics

- Offers schema flexibility
- Provides better performance through data locality
- Closer alignment with application data structures
- Native support for one-to-many relationships

### Some document databases

- MongoDB
- Cassandra
- Apache CouchDB

### The Object-Relational Impedance Mismatch

A key criticism of relational databases is the translation layer required between:

- Object-oriented programming languages
- Relational database structure (tables, rows, columns)

## Representing Complex Data Structures

### Handling One-to-Many Relationships

Relational databases manage these through:

1. Separate tables with foreign key references
2. Structured datatypes (XML, JSON)
3. Encoded documents within text columns

### Example: LinkedIn Profile Representation

**Relational Approach**: Multiple normalized tables

**Document Approach**: Single JSON document with embedded data

```json
{
    "user_id": 251,
    "first_name": "Bill",
    "last_name": "Gates",
    "positions": [
        {"job_title": "Co-chair", "organization": "Bill & Melinda Gates Foundation"},
        {"job_title": "Co-founder, Chairman", "organization": "Microsoft"}
    ],
    "education": [
        {"school_name": "Harvard University", "start": 1973, "end": 1975},
        {"school_name": "Lakeside School, Seattle"}
    ]
}
```

## Schema Approaches

### Schema-on-Write (Relational)

- Explicit schema enforced by the database
- Data must conform to predefined structure
- Rigorous data integrity

### Schema-on-Read (Document)

- Implicit schema interpreted when data is read
- More flexible and adaptable
- Structure not enforced at the database level

## When to Choose Relational Databases

1. Complex Relationships and Joins
2. Strong Data Integrity and Consistency
3. ACID Transactions
4. Many-to-one/Many-to-many relationships (If you have many many-to-many relationships, you may also consider graph databases)

## When to Choose Document Databases

1. Schema changes frequently
2. Entire document needs to be accessed in a single read
3. Data has a document-like structure (tree of one-to-many relationships)

## Query Languages

### SQL Advantages

- Declarative nature
- Specifies result pattern, not querying mechanism
- Easier to parallelize across multiple machines

### Alternative Query Approaches

- MapReduce: Low-level distributed execution model
- Cypher: Declarative query language for graph databases

## Graph Data Model

Particularly suitable for:

- Data with many-to-many relationships
- Complex interconnected data structures
- Applications requiring advanced relationship tracking

## Practical Considerations

### Performance Limitations

- Keep documents relatively small
- Avoid writes that significantly increase document size
- Be cautious of potential performance bottlenecks

## Future of Data Models

A hybrid approach is emerging, with:

- Relational and document models converging
- Polyglot persistence (using multiple database types)
- Increased flexibility and specialized data storage solutions (graph databases)

---
> Contribute to this article [here](https://github.com/programmer-04/ddia-notes.git).
