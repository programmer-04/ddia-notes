---
author: Chinmay Mandke
pubDatetime: 2024-12-15T09:59:23.232Z
# modDatetime: 2023-12-21T09:12:47.400Z
title: Designing Data-Intensive Applications - 1. Reliable, Scalable, and Maintainable Applications
featured: true
draft: false
tags:
  - ddia
  - notes
description: Understanding what makes today's data-intensive systems unique.

---

## Table Of Contents

## Key Building Blocks of Data-Intensive Applications

Applications often integrate multiple standard building blocks:

- **Databases**: Store and retrieve data
- **Caches**: Remember expensive operation results to speed up reads
- **Search Indexes**: Enable keyword search and data filtering
- **Stream Processing**: Send messages to be processed asynchronously
- **Batch Processing**: Periodically process large accumulated datasets

## Fundamental Design Principles

### 1. Reliability

**Definition**: The system's ability to continue working correctly despite adversities.

**Key Characteristics**:

- Fault-tolerant and resilient
- Continues to perform the correct function at the desired performance level
- Tolerates faults rather than attempting to completely avoid them

**Types of Faults**:

- **Hardware Faults**:

  - Physical component failures (e.g., disk failures)
  - Statistically predictable (e.g., in a 10,000-disk cluster, expect ~1 disk failure per day)

- **Software Errors**:
  - Systemic errors like leap second bugs
  - Correlated failures across multiple nodes
  - Often more dangerous than uncorrelated hardware faults

- **Human Errors**:
  - Mitigated through:
    - Quick recovery mechanisms
    - Gradual code rollouts
    - Detailed monitoring
    - Clear performance and error rate tracking

### 2. Scalability

**Definition**: The system's capacity to handle increased load effectively.

**Key Considerations**:

- **Load Parameters**: Metrics describing system load (varies by system architecture)
  - Example: For Twitter, followers per user determines fan-out load

- **Performance Measurement**:
  - Impact of load increase on system resources
  - Resources needed to maintain consistent performance
  - Focus on response time percentiles, not just mean averages

**Scaling Approaches**:

- **Vertical Scaling (Scale Up)**: Move to more powerful machines
- **Horizontal Scaling (Scale Out)**: Distribute load across multiple machines

**Practical Advice**:

- Early-stage startups should prioritize feature iteration over hypothetical future scaling

### 3. Maintainability

**Definition**: Ensuring the system remains adaptable and understandable throughout its lifecycle.

**Key Principles**:

1. **Operability**:
   - Facilitate smooth operations
   - Preserve organizational knowledge
   - Provide predictable system behavior

2. **Simplicity**:
   - Remove accidental complexity
   - Create good abstractions
   - Ease understanding for new engineers

3. **Evolvability (Extensibility)**:
   - Enable easy system modifications
   - Adapt to unanticipated use cases
   - Support changing requirements

## System Design Philosophy

- No single tool meets all data processing needs
- Break work into efficiently manageable tasks
- Create custom data systems by combining general-purpose components
- Focus on functional and non-functional requirements

---
> Contribute to this article [here](https://github.com/programmer-04/ddia-notes.git).
