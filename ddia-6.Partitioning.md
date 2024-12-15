---
author: Chinmay Mandke
pubDatetime: 2024-12-15T13:57:57.173Z
# modDatetime: 2023-12-21T09:12:47.400Z
title: Designing Data-Intensive Applications - 6. Partitioning
slug: ddia-partitioning
featured: true
draft: false
tags:
  - ddia
  - notes
description: Understanding how data is partitioned.
---

## Table Of Contents

Partitioning, also known as sharding, is a technique to break a large database into smaller, manageable pieces. It is essential for scalability and handling datasets that exceed the capacity of a single database.

## Partitioning and Replication

- **Partitioning**: Divides data into smaller subsets, each belonging to a single partition.

- **Replication**: Often works in conjunction with partitioning to ensure fault tolerance.

  - These processes are mostly independent but complement each other.

## Partitioning Key-Value Data

The goal is to distribute data evenly across nodes to avoid **skewed partitions** and **hot spots**.

### Strategies

1. **Key-Range Partitioning**:

   - Data is sorted by keys and split into ranges.
   - Suitable for range queries.
   - **Challenges**:
     - Requires continuous adjustment of boundaries.
     - Risk of hot spots.

2. **Hash-Based Partitioning**:

   - Uses a hash function (e.g., MD5 or Fowler–Noll–Vo) to assign keys to partitions.
   - Ensures uniform distribution of data.
   - **Advantages**:
     - Handles skewed data effectively.
     - Supports partitioning of compound primary keys.
   - **Limitations**:
     - Range queries are not supported (e.g., MongoDB's hash-based sharding).
     - Extreme read/write skew on a single key requires application-level handling.

### Technologies

- **Key-Range Partitioning**: Bigtable, HBase, RethinkDB, MongoDB (pre-v2.4).

- **Hash-Based Partitioning**: Cassandra, Voldemort, Couchbase, MongoDB (hash-sharding).

## Partitioning Secondary Indexes

Secondary indexes complicate partitioning as they don't map neatly to partitions.

### Techniques

1. **Partition-Local Secondary Indexes**:

   - Each partition has its own index for the data it contains.
   - Challenges:
     - For queries spanning multiple partitions, the client must query all partitions.

2. **Global Secondary Indexes**:

   - Partitioned by terms rather than documents.
   - Efficient for reads but increases write complexity.
   - Updates are often asynchronous for performance.

## Rebalancing Partitions

Over time, data rebalancing is necessary to distribute load evenly across nodes.

### Requirements

1. Fair load distribution after rebalancing.
2. Continued acceptance of reads/writes during the process.
3. Minimal data movement between nodes.

### Methods

1. **Hash Mod N**:
   - Inefficient; most keys are relocated when the number of nodes changes.

2. **Fixed Number of Partitions**:

   - Pre-define more partitions than nodes (e.g., 1,000 partitions for 10 nodes).
   - Allows dynamic assignment to nodes as the cluster scales.
   - Technologies: Riak, Elasticsearch, Couchbase, Voldemort.

3. **Dynamic Partitioning**:
   - Partitions grow or shrink based on size thresholds.
   - Adaptable to the total data volume.
   - Works like a top-level B-tree.

4. **Proportional Partitioning**:

   - Fixed number of partitions per node.
   - New nodes split random partitions and take half the data.
   - Risk of uneven splits.

5. **Manual Intervention**:

   - While automation exists, human oversight helps prevent unpredictable behavior.

## Request Routing

Mapping keys to partitions and their hosting nodes is critical for efficient routing.

### Approaches

1. **Node-Forwarding**:

   - Clients connect to any node, which forwards requests to the correct node if needed.

2. **Routing Tier**:

   - A partition-aware load balancer directs requests to the appropriate node.

3. **Partition-Aware Clients**:

   - Clients directly connect to the correct node, eliminating intermediaries.

### Coordination Services

- **ZooKeeper**:

  - Manages metadata (e.g., partition-to-node mappings).
  - Tracks cluster changes, such as node additions or partition reassignments.
  - Notifies clients, routing tiers, or nodes of updates.

---
> Contribute to this article [here](https://github.com/programmer-04/ddia-notes.git).
