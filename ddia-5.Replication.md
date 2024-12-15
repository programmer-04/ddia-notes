---
author: Chinmay Mandke
pubDatetime: 2024-12-15T13:37:27.173Z
# modDatetime: 2023-12-21T09:12:47.400Z
title: Designing Data-Intensive Applications - 5. Replication
slug: ddia-replication
featured: true
draft: false
tags:
  - ddia
  - notes
description: Understanding how data is replicated.
---

## Table Of Contents

Replication involves keeping copies of the same data on multiple machines connected via a network. This serves several purposes:

- **Reduce latency**: Keep data geographically close to users.
- **Increase availability**: Allow the system to continue working even if parts fail.
- **Scale read throughput**: Spread read queries across multiple machines.

## Leader-Based Replication

### Overview

- One replica is designated as the **leader** (or master/primary).
- All writes go to the leader, which updates its local storage and sends changes to followers (or replicas).
- Followers apply changes in the same order as the leader and serve read-only queries.

### Technologies

- **Relational Databases**: PostgreSQL, MySQL, Oracle Data Guard, SQL Server.
- **Non-Relational Databases**: MongoDB, RethinkDB.
- **Message Brokers**: Kafka, RabbitMQ.

### Synchronous vs. Asynchronous Replication

- **Synchronous**:
  - Guarantees up-to-date data on followers.
  - Slower and may block writes if a follower doesn't respond.
  - Often used with a single synchronous follower while others are asynchronous.
- **Asynchronous**:
  - Faster but may lag behind the leader.
  - Commonly used due to its practicality and lower latency.

### Setting Up a New Follower

1. Take a consistent snapshot of the leader’s database.
2. Copy the snapshot to the new follower.
3. Sync the follower with the leader by applying all changes since the snapshot.

### Failure Scenarios

- **Follower Failure**: The follower requests all missed changes from the leader upon recovery.
- **Leader Failure (Failover)**:
  - Detect failure (e.g., via timeout).
  - Promote a follower to leader.
  - Reconfigure clients and other followers to sync with the new leader.
  - Challenges:
    - Potential data loss if updates were not propagated.
    - Risk of split-brain scenarios if the old leader resumes as active.

### Replication Logs

#### Statement-Based Replication

- Sends SQL statements to followers.
- Issues:
  - Nondeterministic functions (e.g., `NOW()`, `RAND()`).
  - Dependency on execution order.
  - Side effects (e.g., triggers, stored procedures).

#### Write-Ahead Log (WAL) Shipping

- Leader writes changes to a log and shares it with followers.
- Followers apply changes to maintain an identical state.

#### Logical Log Replication

- Tracks changes at the row level (insert, update, delete).
- Decouples replication from storage engine internals.

#### Trigger-Based Replication

- Custom application logic logs changes.
- Used for specific replication needs but introduces additional overhead and potential bugs.

## Consistency Models

### Read-After-Write Consistency

- Ensures users see their updates immediately after submission.
- Strategies:
  - Route user-specific reads to the leader.
  - Prevent queries on followers with significant replication lag.

### Monotonic Reads

- Guarantees users don’t see out-of-order updates.
- Achieved by consistently reading from the same replica.

### Consistent Prefix Reads

- Ensures write sequences are read in the same order.
- Requires causal writes to be routed to the same partition and applied in order.

### Eventual Consistency

- Followers eventually sync with the leader but may lag significantly.
- Stronger guarantees (e.g., read-after-write, monotonic reads) are needed if lag is unacceptable.

## Multi-Leader Replication

- Leaders exist in multiple data centers, allowing independent writes.
- Benefits:
  - Improved performance by reducing latency.
  - Tolerance for network issues and data center failures.

### Challenges

- Write conflicts occur when data is concurrently modified in different leaders.
- **Conflict Resolution Strategies**:
  - Assign unique IDs to writes (e.g., highest ID wins).
  - Prioritize replicas by ID.
  - Merge conflicting values.
  - Record conflicts explicitly for application-specific handling.

### Topologies

- **All-to-All**: No single point of failure but risks causality issues.
- **Circular**: Simple but vulnerable to single-node failures.
- **Star**: Centralized leader communication but introduces a single point of failure.

### Offline Operation

- Common in mobile devices where each acts as a local leader.
- Synchronization occurs asynchronously when devices reconnect.

## Leaderless Replication (Dynamo-Style)

- No single leader; writes and reads are sent to multiple replicas in parallel.
- Found in systems like Dynamo, Riak, Cassandra, and Voldemort.

### Mechanisms for Handling Inconsistencies

1. **Read Repair**:
   - Detects stale replicas during reads and updates them.
2. **Anti-Entropy Process**:
   - Background process resolves inconsistencies across replicas asynchronously.

### Quorums

- If:
  - `n`: Total replicas.
  - `w`: Replicas required for a write.
  - `r`: Replicas queried for a read.
  - Ensures consistency if `w + r > n`.

### Conflict Resolution

- **Last Write Wins (LWW)**: Prioritizes the most recent write (sacrifices durability).
- **Happens-Before**: Detects concurrent writes requiring resolution.
- **Merge Values**: Preserves all conflicting data for application-specific handling.
- **Version Vectors**: Tracks version numbers for each replica to detect inconsistencies.

---
> Contribute to this article [here](https://github.com/programmer-04/ddia-notes.git).
