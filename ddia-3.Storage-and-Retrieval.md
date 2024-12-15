---
author: Chinmay Mandke
pubDatetime: 2024-12-15T10:50:35.232Z
# modDatetime: 2023-12-21T09:12:47.400Z
title: Designing Data-Intensive Applications - 3. Storage and Retrieval
slug: ddia-storage-and-retrieval
featured: true
draft: false
tags:
  - ddia
  - notes
description: Understanding how the data structures used to store and retrieve data in data-intensive applications.
---

## Table Of Contents

## Data Structures in Databases

### Logs

- A **log** is an append-only sequence of records used by many databases.
  - Challenges: concurrency control, disk space management, error handling, and crash recovery.
- Benefits of append-only design:
  - Sequential writes are faster than random writes.
  - Simplifies concurrency and crash recovery.
  - Merging segments avoids file fragmentation.

### Indexes

- Indexes are additional structures derived from the primary data.
- Benefits:
  - Speed up read queries.
- Drawbacks:
  - Slow down writes due to additional overhead.

### Types of Indexing Structures

#### Hash Indexes

- Key-value stores resemble hash tables.
- Example: **Bitcask** (default storage engine in Riak).
  - Uses in-memory hash maps to map keys to byte offsets in log files.
  - Requires all keys to fit in RAM, but values can reside on disk.
- Disk space management:
  - Use segmented logs and periodically perform **compaction** to remove duplicate keys and merge segments.
  - Compaction ensures only the latest updates are kept, optimizing storage and lookup performance.

#### SSTables and LSM-Trees

- **SSTables** (Sorted String Tables): Key-value pairs sorted by key.
  - Efficient merging via algorithms like mergesort.
  - Requires only partial in-memory indexes.
  - Group records into compressed blocks for efficient storage.

- **LSM-Trees** (Log-Structured Merge Trees):
  - Uses in-memory balanced trees (e.g., red-black trees) to handle writes.
  - Periodically writes the in-memory tree to disk as SSTable files.
  - Background compaction merges and optimizes segments.
  - Examples: LevelDB, RocksDB (leveled compaction); HBase, Cassandra (size-tiered compaction).
  - Bloom filters help optimize lookups for non-existent keys.

#### B-Trees

- Widely used indexing structure.
- Key-value pairs are stored in fixed-size pages, enabling efficient lookups and range queries.
- Operations:
  - Updates and inserts modify pages and split them if needed.
  - Crash recovery handled via Write-Ahead Logs (WAL).
- Comparison with LSM-Trees:
  - Faster reads due to single-location indexing.
  - Predictable performance, but lower write throughput and higher fragmentation.

## Alternative Indexing Structures

### Secondary Indexes

- Enable efficient querying on non-primary fields.
- Techniques:
  - Store lists of matching row identifiers for each indexed value.
  - Append unique row identifiers to each index entry.

### Full-Text Search

- Fuzzy querying capabilities (e.g., synonyms, grammatical variations).
- Example: **Lucene** (used by Elasticsearch and Solr).
  - Term dictionary stored using SSTable-like structures.

### In-Memory Indexes

- Entire dataset resides in memory for low-latency access.
- Examples:
  - Memcached (cache-focused, weak durability).
  - VoltDB, MemSQL (durable in-memory databases).
- Benefits:
  - Avoid overhead of disk I/O.
  - Enable complex data models.

## Storage Engines for Different Workloads

### Transactional vs. Analytical Workloads

- **Transactional (OLTP)**: High volume of reads and writes with small queries.
- **Analytical (OLAP)**: Large, read-only queries for aggregate statistics.

### Data Warehousing

- Separate systems for analytical queries.
- Use **Extract-Transform-Load (ETL)** to prepare data for analysis.
- Common architecture: **Star schema** with fact and dimension tables.
- Technologies: Amazon RedShift, Apache Hive, Spark SQL, Cloudera Impala.

### Column-Oriented Storage

- Optimized for analytical queries.
- Stores data by columns instead of rows, enabling efficient querying and compression.
- Techniques:
  - **Bitmap encoding** for filtering and aggregation.
  - **Materialized views** for caching query results (e.g., OLAP cubes).

## Key Takeaways

- Log-structured storage engines excel at write-heavy workloads.
- B-Trees are reliable for predictable read-heavy workloads.
- Column-oriented storage is ideal for data warehouses.
- Choosing the right storage engine depends on workload patterns and data size.

---
> Contribute to this article [here](https://github.com/programmer-04/ddia-notes.git).
