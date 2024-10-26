---
title: "Why Odd Number of etcd Nodes Ensures High Availability in Production ?"
description: "An in-depth guide on why an odd number of etcd nodes is recommended for high availability in production Kubernetes clusters."
excerpt: "Learn how choosing an odd number of etcd nodes optimizes fault tolerance, cost-effectiveness, and resilience in Kubernetes clusters."
date: 2024-10-26T09:54:50+05:30
lastmod: 2024-10-26T09:54:50+05:30
draft: false
weight: 100
images: [etcd.png]
categories: ["Kubernetes", "High Availability", "etcd"]
tags: ["etcd", "Kubernetes", "High Availability", "Raft Consensus"]
contributors: ["anishbista60"]
pinned: false
homepage: false
comments: true
---

### Introduction

In building resilient Kubernetes clusters, **etcd** is crucial. As Kubernetes’ primary database, it stores the entire cluster state, making etcd essential for smooth operation. One often-overlooked aspect of etcd’s configuration is the cluster's node count. But why is an *odd number of nodes* recommended for production?

In this blog, we’ll explore why this best practice exists, examining the Raft consensus algorithm, high availability, and an example comparing different setups.

### Understanding etcd and High Availability

etcd is a **distributed key-value store** for storing all cluster data, configurations, secrets, and service discovery information. For Kubernetes clusters to be resilient, etcd must also be highly available—this is where the concept of **quorum** comes into play.

### The Role of the Raft Consensus Algorithm

etcd uses the **Raft consensus algorithm** to replicate data across all nodes, ensuring all nodes agree on data consistency. In Raft, one node acts as the leader, with the rest as followers. When a leader fails, a **quorum** (majority of nodes) elects a new leader. This is why having an odd number of nodes is essential to avoid **split-brain scenarios** and simplify leader election.

### What is a Quorum?

A quorum is the **minimum number of nodes** required to agree on any transaction. The quorum size calculation ensures the cluster can still operate even if some nodes fail:
```
Quorum = (N/2) + 1
```


Here,  `N` is the total number of nodes in the cluster. This formula ensures the cluster maintains write capability during failures.

### Why Odd Number of Nodes?

Having an odd number of nodes simplifies the quorum calculation and optimizes availability.

1. **Optimal Quorum Calculation:** In a 3-node cluster, quorum is 2, allowing 1 node to fail. A 4-node cluster also has a quorum of 3, still tolerating just 1 node failure. The 4th node adds no value in terms of availability.
2. **Leader Election & Split-Brain Prevention:** Odd numbers prevent ties, streamlining leader election.
3. **Cost-Effectiveness:** Odd-numbered setups avoid redundant resource expenses.

### 3-Node vs. 4-Node etcd Clusters: A Practical Example

#### Scenario 1: 3-Node etcd Cluster
In a 3-node cluster:
```
Quorum = (3/2) + 1 = 2
```

- **Fault Tolerance:** The cluster tolerates 1 node failure with quorum maintained.
- **High Availability:** Write operations continue with 2 nodes, maintaining consistency.

#### Scenario 2: 4-Node etcd Cluster
For a 4-node cluster:
```
Quorum = (4/2) + 1 = 3
```

- **Fault Tolerance:** Only 1 node failure tolerance, the same as the 3-node cluster.
- **High Availability:** No added availability, making the extra node redundant.

### When to Consider More Nodes: The 5-Node Cluster

If higher availability is necessary, the next logical step is a 5-node cluster:
```
Quorum = (5/2) + 1 = 3
```

- **Fault Tolerance:** Tolerates up to 2 node failures while maintaining quorum.
- **High Availability:** Provides greater resilience than 3- or 4-node clusters, justifying the added resources.

### Conclusion

The Raft algorithm in etcd relies on a majority for data consistency and leader election. Opting for an odd number of nodes ensures optimal fault tolerance without overspending. For most setups, a 3-node cluster is sufficient, while a 5-node cluster is a strong option if higher availability is needed. 

By choosing an odd number of etcd nodes, you optimize for **performance, cost, and resilience**, building a more robust Kubernetes cluster.

