---
author: Chinmay Mandke
pubDatetime: 2024-12-15T11:49:45.232Z
# modDatetime: 2023-12-21T09:12:47.400Z
title: Designing Data-Intensive Applications - 4. Encoding and Evolution
slug: ddia-encoding-and-evolution
featured: true
draft: false
tags:
  - ddia
  - notes
description: Understanding how data flows among data-intensive applications.
---

## Table Of Contents

## Formats for Encoding Data

Data is typically represented in two forms:

1. **In-memory representation**: Data structures such as objects, arrays, hash tables, or trees optimized for efficient access and manipulation by the CPU.
2. **Serialized representation**: Self-contained byte sequences (e.g., JSON, XML, or binary formats) used for storage or network transmission.

### Encoding and Decoding

- **Encoding**: Translating in-memory data to a byte sequence.
- **Decoding**: Reconstructing in-memory data from a byte sequence.

### Language-Specific Encodings

- Examples: Java’s `Serializable`, Python’s `pickle`, Ruby’s `Marshal`.
- **Limitations**:
  - Tied to specific programming languages.
  - Security vulnerabilities.
  - Performance issues.
  - Poor interoperability.

### Textual Formats

- **JSON** and **XML**:
  - Human-readable.
  - Language-independent.
  - Good for small datasets.

### Binary Formats

- **Advantages**:
  - Compact storage and faster parsing.
  - Better suited for large datasets.
- **Libraries**:
  - Apache Thrift and Protocol Buffers (protobuf).
  - Use schemas to describe the data structure.
  - Provide code generation tools to create schema-specific classes.

## Models of Dataflow

Data can flow between processes using:

1. **Databases**
2. **Service calls** (e.g., REST, RPC).
3. **Asynchronous message passing**.

### Dataflow Through Databases

- Acts as sending a future message to oneself.
- Requires compatibility during schema changes.
- Large databases often face challenges with schema migrations.

### Service-Oriented Architectures (SOA)

- Services are independently deployable and evolvable.
- Service-to-service communication can use APIs instead of direct database access.

### Service Calls

- **REST**:
  - Design philosophy using simple data formats and URLs.
  - Common for public APIs.
- **SOAP**:
  - XML-based protocol with strong type definitions.
  - Clients access remote services as if calling local methods.

#### Remote Procedure Calls (RPC)

- Simulates local function calls across a network.
- Challenges:
  - Network unpredictability (latency, retries, timeouts).
  - Potential for duplicate actions without idempotency.
  - Compatibility between versions of requests and responses.
- **Frameworks**:
  - Thrift, gRPC, Finagle, Rest.li.
  - Explicitly acknowledge network differences through asynchronous handling (e.g., futures).

### Asynchronous Message Passing

- Uses intermediaries like message brokers or queues.
- **Advantages**:
  - Buffers messages when recipients are unavailable.
  - Handles retries and message delivery to multiple recipients.
  - Decouples sender and receiver.
- **Limitations**:
  - One-way communication; replies require a separate channel.
- **Message Brokers**:
  - Organize messages into topics for publication and subscription.
  - Enable chaining processes and publishing replies.

---
> Contribute to this article [here](https://github.com/programmer-04/ddia-notes.git).
