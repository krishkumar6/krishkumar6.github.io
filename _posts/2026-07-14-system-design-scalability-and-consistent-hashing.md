---
layout: post
title: "System Design Notes: Scalability, Load Balancing & Consistent Hashing"
---

## Understand Scalability

Scalability refers to the capability of a system to grow and manage increased demand (in terms of user load, data volume, or transaction throughput).

### Two ways to increase scalability

**1. Vertical Scaling** — This involves adding more power (CPU, RAM, etc.) to an existing server.

**2. Horizontal Scaling** — This method adds more machines (servers) to a system and distributes the load across them. It's more complex but offers greater flexibility.

### Horizontal vs. Vertical Scaling

| # | Horizontal | Vertical |
|---|-----------|----------|
| 1 | Load balancing required | No need — it has only one huge server |
| 2 | Resilient (if one server fails, the load transfers to other servers) | Single point of failure (only one server) |
| 3 | Network calls are slower because of complex structure | Communicates via inter-process communication (IPC) and shared memory, so it is fast |
| 4 | Data inconsistency — data is spread across a chain of servers, like restaurants trying to sync their menu over the phone: a synchronization problem | Data consistency — data lives in a single "kitchen" where one chef sees everything, no synchronization problem |
| 5 | Scales well as users increase | Hardware limit — you can only upgrade one machine/server so much |

## What is Load Balancing?

Distribute incoming requests to a set of servers.

<div class="diagram">
<svg viewBox="0 0 480 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Three clients connect to a load balancer, which distributes requests to three servers">
  <style>
    .box { fill: #f5f5f5; stroke: #333; stroke-width: 1.5; }
    .lbl { font: 14px sans-serif; fill: #222; text-anchor: middle; }
    .arrow { stroke: #333; stroke-width: 1.5; fill: none; marker-end: url(#arrowhead); }
  </style>
  <defs>
    <marker id="arrowhead" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L6,3 L0,6 Z" fill="#333"/>
    </marker>
  </defs>

  <rect class="box" x="10" y="20" width="100" height="36" rx="4"/>
  <text class="lbl" x="60" y="43">Client 1</text>
  <rect class="box" x="10" y="112" width="100" height="36" rx="4"/>
  <text class="lbl" x="60" y="135">Client 2</text>
  <rect class="box" x="10" y="204" width="100" height="36" rx="4"/>
  <text class="lbl" x="60" y="227">Client 3</text>

  <rect class="box" x="190" y="112" width="110" height="36" rx="4"/>
  <text class="lbl" x="245" y="135">Load Balancer</text>

  <rect class="box" x="370" y="20" width="100" height="36" rx="4"/>
  <text class="lbl" x="420" y="43">Server 1</text>
  <rect class="box" x="370" y="112" width="100" height="36" rx="4"/>
  <text class="lbl" x="420" y="135">Server 2</text>
  <rect class="box" x="370" y="204" width="100" height="36" rx="4"/>
  <text class="lbl" x="420" y="227">Server 3</text>

  <path class="arrow" d="M110,38 C160,38 160,120 188,128"/>
  <path class="arrow" d="M110,130 L188,130"/>
  <path class="arrow" d="M110,222 C160,222 160,140 188,132"/>

  <path class="arrow" d="M300,128 C340,120 340,50 368,40"/>
  <path class="arrow" d="M300,130 L368,130"/>
  <path class="arrow" d="M300,132 C340,140 340,212 368,222"/>
</svg>
</div>

## Consistent Hashing

It is a technique to distribute requests efficiently and evenly across servers.

### Why we need consistent hashing: the rehashing problem

If you have N servers, a common way to balance the load is to use the following hash method:

```
server = h(request ID) % N
```

- **N** → size of server pool, i.e. number of servers
- **h** → hash function

**Example:** request IDs go from `0` to `m - 1`, `h(id) → id % m`

```
h(10) → 3   ⇒ 3 % 4 = 3
h(20) → 15  ⇒ 15 % 4 = 3
h(35) → 12  ⇒ 12 % 4 = 0
```

This approach works well when the size of the server pool is fixed. However, a problem arises when new servers are added or existing servers are removed.

**Example:** if server 1 goes offline, the size of the server pool becomes 3. Using the same hash function, we get the same hash value for a request ID, but applying the modulo operation gives us different server indexes, because the number of servers is reduced by 1.

This means that when server 1 goes offline, most clients will connect to the wrong server to fetch data. This causes a storm.

That's why consistent hashing is an effective technique to mitigate this problem.

### Consistent hashing, explained

Consistent hashing evenly distributes requests across servers by mapping both keys and servers onto a virtual "hash ring."

When servers are added or removed, it minimizes data movement, requiring only a small fraction of request IDs to be remapped rather than globally reshuffling the entire dataset.

**Hash servers:** using the same hash function, we map servers based on server IP or name onto the ring.

<div class="diagram">
<svg viewBox="0 0 300 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Basic hash ring with servers S0 to S3 and keys K0 to K3">
  <style>
    .ring { fill: none; stroke: #333; stroke-width: 2; }
    .node { fill: #d6e6ff; stroke: #333; stroke-width: 1.5; }
    .keynode { fill: #ffe6b3; stroke: #333; stroke-width: 1.5; }
    .lbl { font: 13px sans-serif; fill: #222; text-anchor: middle; }
    .arrow { stroke: #333; stroke-width: 1.5; fill: none; marker-end: url(#arrowhead2); }
  </style>
  <defs>
    <marker id="arrowhead2" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L6,3 L0,6 Z" fill="#333"/>
    </marker>
  </defs>

  <circle class="ring" cx="150" cy="150" r="95"/>

  <circle class="keynode" cx="150" cy="40" r="12"/>
  <text class="lbl" x="150" y="25">K0</text>

  <circle class="node" cx="227" cy="72" r="14"/>
  <text class="lbl" x="248" y="60">S0</text>

  <circle class="keynode" cx="260" cy="150" r="12"/>
  <text class="lbl" x="278" y="154">K1</text>

  <circle class="node" cx="227" cy="228" r="14"/>
  <text class="lbl" x="248" y="245">S1</text>

  <circle class="keynode" cx="150" cy="260" r="12"/>
  <text class="lbl" x="150" y="280">K2</text>

  <circle class="node" cx="72" cy="228" r="14"/>
  <text class="lbl" x="50" y="245">S2</text>

  <circle class="keynode" cx="40" cy="150" r="12"/>
  <text class="lbl" x="20" y="154">K3</text>

  <circle class="node" cx="72" cy="72" r="14"/>
  <text class="lbl" x="50" y="60">S3</text>

  <path class="arrow" d="M156,42 A100,100 0 0 1 222,68"/>
</svg>
</div>

**To determine which server a key is stored on, we go clockwise from the key's position on the ring until a server is found.**

### Add a server

Using the logic described above, adding a new server will only require redistribution of a fraction of keys.

<div class="diagram">
<svg viewBox="0 0 300 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Hash ring after adding server S4: only K0 gets remapped, from S3 to S4">
  <style>
    .ring { fill: none; stroke: #333; stroke-width: 2; }
    .node { fill: #d6e6ff; stroke: #333; stroke-width: 1.5; }
    .newnode { fill: #c8f7c5; stroke: #333; stroke-width: 1.5; }
    .keynode { fill: #ffe6b3; stroke: #333; stroke-width: 1.5; }
    .lbl { font: 13px sans-serif; fill: #222; text-anchor: middle; }
    .arrow { stroke: #b00020; stroke-width: 1.5; fill: none; stroke-dasharray: 4 3; marker-end: url(#arrowhead3); }
  </style>
  <defs>
    <marker id="arrowhead3" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L6,3 L0,6 Z" fill="#b00020"/>
    </marker>
  </defs>

  <circle class="ring" cx="150" cy="150" r="95"/>

  <circle class="keynode" cx="150" cy="40" r="12"/>
  <text class="lbl" x="150" y="25">K0</text>

  <circle class="newnode" cx="200" cy="50" r="14"/>
  <text class="lbl" x="215" y="35">S4 (new)</text>

  <circle class="node" cx="227" cy="72" r="14"/>
  <text class="lbl" x="255" y="72">S0</text>

  <circle class="keynode" cx="260" cy="150" r="12"/>
  <text class="lbl" x="278" y="154">K1</text>

  <circle class="node" cx="227" cy="228" r="14"/>
  <text class="lbl" x="248" y="245">S1</text>

  <circle class="keynode" cx="150" cy="260" r="12"/>
  <text class="lbl" x="150" y="280">K2</text>

  <circle class="node" cx="72" cy="228" r="14"/>
  <text class="lbl" x="50" y="245">S2</text>

  <circle class="keynode" cx="40" cy="150" r="12"/>
  <text class="lbl" x="20" y="154">K3</text>

  <circle class="node" cx="72" cy="72" r="14"/>
  <text class="lbl" x="50" y="60">S3</text>

  <path class="arrow" d="M150,150 L150,40"/>
  <text x="150" y="120" class="lbl" fill="#b00020">K0 remapped</text>
</svg>
</div>

After a new server S4 is added, only key K0 needs to be redistributed. K1, K2, and K3 remain on the same server.

### Remove a server

When a server is removed, only a small fraction of keys require redistribution — this is the benefit of consistent hashing.

<div class="diagram">
<svg viewBox="0 0 300 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Hash ring after removing server S1: only K1 gets remapped to S2">
  <style>
    .ring { fill: none; stroke: #333; stroke-width: 2; }
    .node { fill: #d6e6ff; stroke: #333; stroke-width: 1.5; }
    .removednode { fill: #f7c5c5; stroke: #333; stroke-width: 1.5; stroke-dasharray: 3 2; }
    .keynode { fill: #ffe6b3; stroke: #333; stroke-width: 1.5; }
    .lbl { font: 13px sans-serif; fill: #222; text-anchor: middle; }
    .arrow { stroke: #b00020; stroke-width: 1.5; fill: none; stroke-dasharray: 4 3; marker-end: url(#arrowhead4); }
  </style>
  <defs>
    <marker id="arrowhead4" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L6,3 L0,6 Z" fill="#b00020"/>
    </marker>
  </defs>

  <circle class="ring" cx="150" cy="150" r="95"/>

  <circle class="keynode" cx="150" cy="40" r="12"/>
  <text class="lbl" x="150" y="25">K0</text>

  <circle class="node" cx="227" cy="72" r="14"/>
  <text class="lbl" x="248" y="60">S0</text>

  <circle class="keynode" cx="260" cy="150" r="12"/>
  <text class="lbl" x="278" y="154">K1</text>

  <circle class="removednode" cx="227" cy="228" r="14"/>
  <text class="lbl" x="248" y="245">S1 (removed)</text>

  <circle class="keynode" cx="150" cy="260" r="12"/>
  <text class="lbl" x="150" y="280">K2</text>

  <circle class="node" cx="72" cy="228" r="14"/>
  <text class="lbl" x="50" y="245">S2</text>

  <circle class="keynode" cx="40" cy="150" r="12"/>
  <text class="lbl" x="20" y="154">K3</text>

  <circle class="node" cx="72" cy="72" r="14"/>
  <text class="lbl" x="50" y="60">S3</text>

  <path class="arrow" d="M260,150 A100,100 0 0 1 80,225"/>
  <text x="150" y="200" class="lbl" fill="#b00020">K1 remapped to S2</text>
</svg>
</div>

When server S1 is removed, only key K1 must be remapped to server S2. The rest of the keys are unaffected.

### Two problems with this basic approach

1. **It is impossible to keep the same size of partitions on the ring for all servers**, considering a server can be added or removed.
2. **It is possible to have a non-uniform key distribution on the ring.** For example, if most of the keys are stored on server 2, however, server 1 and server 3 have no data.

To solve this problem, we use the **virtual nodes** technique.

### Virtual nodes

Instead of mapping a physical server to a single spot on the ring, a single server is represented by multiple "virtual" nodes (e.g. Server 0 → S0-0, S0-1, S0-2).

This evenly slices the ring into smaller, more randomized intervals, ensuring an even distribution of data and traffic across all physical machines.

<div class="diagram">
<svg viewBox="0 0 300 300" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Hash ring with virtual nodes: server 0 and server 1 each split into three virtual nodes spread around the ring">
  <style>
    .ring { fill: none; stroke: #333; stroke-width: 2; }
    .node0 { fill: #d6e6ff; stroke: #333; stroke-width: 1.5; }
    .node1 { fill: #ffd6e6; stroke: #333; stroke-width: 1.5; }
    .lbl { font: 12px sans-serif; fill: #222; text-anchor: middle; }
  </style>

  <circle class="ring" cx="150" cy="150" r="95"/>

  <circle class="node0" cx="150" cy="40" r="12"/>
  <text class="lbl" x="150" y="25">S0-2</text>

  <circle class="node1" cx="227" cy="72" r="12"/>
  <text class="lbl" x="255" y="65">S1-0</text>

  <circle class="node0" cx="260" cy="150" r="12"/>
  <text class="lbl" x="278" y="154">S0-0</text>

  <circle class="node1" cx="227" cy="228" r="12"/>
  <text class="lbl" x="255" y="245">S1-1</text>

  <circle class="node0" cx="150" cy="260" r="12"/>
  <text class="lbl" x="150" y="280">S0-1</text>

  <circle class="node1" cx="72" cy="228" r="12"/>
  <text class="lbl" x="45" y="245">S1-2</text>

  <circle class="node0" cx="40" cy="150" r="12"/>
  <text class="lbl" x="18" y="154">S0-?</text>

  <circle class="node1" cx="72" cy="72" r="12"/>
  <text class="lbl" x="45" y="60">S1-?</text>

  <text class="lbl" x="150" y="150" font-size="13">Server 0 (blue) / Server 1 (pink)</text>
</svg>
</div>

## Message Queues

Message queuing makes it possible for applications to communicate asynchronously by sending messages to each other via a queue.

- **Asynchronous processing** allows a task to call a service and move on to the next task while the service processes the request at its own pace.
- **Queue** — a line of things waiting to be handled, in sequential order, starting at the beginning of the line.
- **Message** — the data transported between the sender and the receiver application.

The basic architecture of a message queue is simple: there are client applications called **producers** that create messages and deliver them to the message queue. Another application, called a **consumer**, connects to the queue and gets the messages to be processed. Messages placed onto the queue are stored until the consumer retrieves them.

**Examples of queues:** Kafka, Amazon SQS, etc.

> **Note:** Message queuing fulfills this purpose by providing a means for services to push messages to a queue asynchronously and ensure that they get delivered to the correct destination.

To implement a message queue between services, you need a **message broker** — think of it as a mailman who takes mail from a sender and delivers it to the correct destination. Example: **RabbitMQ**.
