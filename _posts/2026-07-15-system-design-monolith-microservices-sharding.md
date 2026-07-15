---
layout: post
title: "System Design Notes: Monolithic vs. Microservices Architecture & Database Sharding"
---

## Monolithic Architecture

It is a single deployable unit containing all components, sharing a single database.

<div class="diagram">
<svg viewBox="0 0 560 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Client connects through a load balancer to a monolithic application containing the user interface, business logic, and data access layer, which talks to a single database">
  <style>
    .box { fill: #f5f5f5; stroke: #333; stroke-width: 1.5; }
    .group { fill: none; stroke: #999; stroke-width: 1.2; stroke-dasharray: 4 3; }
    .lbl { font: 14px sans-serif; fill: #222; text-anchor: middle; }
    .sublbl { font: 12px sans-serif; fill: #222; text-anchor: middle; }
    .cap { font: 12px sans-serif; fill: #555; text-anchor: middle; }
    .arrow { stroke: #333; stroke-width: 1.5; fill: none; marker-end: url(#arrowhead); }
  </style>
  <defs>
    <marker id="arrowhead" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L6,3 L0,6 Z" fill="#333"/>
    </marker>
  </defs>

  <rect class="box" x="10" y="100" width="90" height="36" rx="4"/>
  <text class="lbl" x="55" y="123">Client</text>

  <rect class="box" x="140" y="100" width="100" height="36" rx="4"/>
  <text class="lbl" x="190" y="123">Load Balancer</text>

  <rect class="group" x="290" y="20" width="150" height="200"/>
  <text class="cap" x="365" y="14">Monolithic Application</text>
  <rect class="box" x="305" y="35" width="120" height="30" rx="4"/>
  <text class="sublbl" x="365" y="55">User Interface</text>
  <rect class="box" x="305" y="90" width="120" height="30" rx="4"/>
  <text class="sublbl" x="365" y="110">Business Logic</text>
  <rect class="box" x="305" y="145" width="120" height="30" rx="4"/>
  <text class="sublbl" x="365" y="165">Data Access Layer</text>

  <rect class="box" x="480" y="100" width="70" height="50" rx="4"/>
  <text class="sublbl" x="515" y="122">Single</text>
  <text class="sublbl" x="515" y="138">Database</text>

  <path class="arrow" d="M100,118 L138,118"/>
  <path class="arrow" d="M240,118 L288,118"/>
  <path class="arrow" d="M425,165 C450,180 460,150 478,130"/>
</svg>
</div>

- It is a single deployable unit containing all components and sharing a single database.
- Communication between components is direct and internal (all in one process).
- Simpler architecture, but less flexible.

### Advantages vs. disadvantages

| # | Advantages | Disadvantages |
|---|-----------|----------------|
| 1 | Simplicity — because of a single codebase | Complex codebase as the codebase grows |
| 2 | Communication is fast, because everything runs in a single process | Harder to scale parts of the application independently — scaling a monolith often means scaling the entire application |
| 3 | New developers can onboard more easily, since they only need to understand one codebase | A small change in one part can require redeployment of the entire application |
| 4 | Initial cost is low | Limited in adopting different technologies, because of the single codebase |

## Microservices Architecture

Multiple independent services (e.g. profiles, order, chat), each with its own database.

<div class="diagram">
<svg viewBox="0 0 480 240" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Client connects through a gateway to three independent services -- profiles, order, and chat -- each with its own database">
  <style>
    .box { fill: #f5f5f5; stroke: #333; stroke-width: 1.5; }
    .lbl { font: 14px sans-serif; fill: #222; text-anchor: middle; }
    .arrow { stroke: #333; stroke-width: 1.5; fill: none; marker-end: url(#arrowhead5); }
  </style>
  <defs>
    <marker id="arrowhead5" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L6,3 L0,6 Z" fill="#333"/>
    </marker>
  </defs>

  <rect class="box" x="10" y="102" width="80" height="36" rx="4"/>
  <text class="lbl" x="50" y="125">Client</text>

  <rect class="box" x="140" y="20" width="50" height="200" rx="4"/>
  <text class="lbl" x="165" y="125" transform="rotate(-90 165 125)">Gateway</text>

  <rect class="box" x="260" y="20" width="100" height="36" rx="4"/>
  <text class="lbl" x="310" y="43">Profiles</text>
  <rect class="box" x="260" y="102" width="100" height="36" rx="4"/>
  <text class="lbl" x="310" y="125">Order</text>
  <rect class="box" x="260" y="184" width="100" height="36" rx="4"/>
  <text class="lbl" x="310" y="207">Chat</text>

  <circle cx="430" cy="38" r="16" fill="#f5f5f5" stroke="#333" stroke-width="1.5"/>
  <circle cx="430" cy="120" r="16" fill="#f5f5f5" stroke="#333" stroke-width="1.5"/>
  <circle cx="430" cy="202" r="16" fill="#f5f5f5" stroke="#333" stroke-width="1.5"/>

  <path class="arrow" d="M90,120 L138,120"/>
  <path class="arrow" d="M190,60 C220,60 220,38 258,38"/>
  <path class="arrow" d="M190,120 L258,120"/>
  <path class="arrow" d="M190,180 C220,180 220,202 258,202"/>

  <path class="arrow" d="M360,38 L412,38"/>
  <path class="arrow" d="M360,120 L412,120"/>
  <path class="arrow" d="M360,202 L412,202"/>
</svg>
</div>

- Multiple independent services (profiles, order, chat). Each service has its own database.
- Services communicate via an API gateway, and services can interact with each other when needed.
- More complex, but highly scalable and maintainable.

### Advantages

1. **Scalability** — each service can be scaled independently based on its own resource needs.
2. **Flexibility** — different services can be developed using different tech stacks.
3. **Fault isolation** — if one service fails, it does not affect the rest of the system.
4. **Continuous deployment** — you can update a particular service without impacting other services.

### Disadvantages

1. **Complexity in management** — managing multiple services introduces complexity.
2. **Data consistency** — maintaining data consistency across services can be tricky.
3. **Deployment overhead** — each service needs its own pipeline and configuration.

### When are monoliths favorable?

Microservices are at a disadvantage compared to monoliths in some cases. Monoliths are favorable when:

1. The technical/developer team is very small.
2. The service is simple to think of as a whole.
3. The service requires very high efficiency, where network calls are avoided as much as possible.
4. All developers must have context of all services.

## Database Sharding

When a single database can't keep up anymore, you have only one real option: split your data across multiple machines. This is called **sharding**, and it is a necessity at scale.

### First, what is partitioning?

Partitioning means splitting a large table into smaller pieces inside a single database.

Two common types:

1. **Horizontal partitioning** — split rows across partitions.
2. **Vertical partitioning** — split columns across partitions.

### What is sharding?

Sharding is horizontal partitioning, where we spread the load across many *independent* databases.

**Example:** if we partitioned our order data by `id`, we might end up with something like this:

<div class="diagram">
<svg viewBox="0 0 420 220" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Original database at capacity splits into three shards, each holding a range of ids">
  <style>
    .box { fill: #f5f5f5; stroke: #333; stroke-width: 1.5; }
    .lbl { font: 13px sans-serif; fill: #222; text-anchor: middle; }
    .cap { font: 11px sans-serif; fill: #555; text-anchor: middle; }
    .arrow { stroke: #333; stroke-width: 1.5; fill: none; marker-end: url(#arrowhead6); }
  </style>
  <defs>
    <marker id="arrowhead6" markerWidth="8" markerHeight="8" refX="6" refY="3" orient="auto">
      <path d="M0,0 L6,3 L0,6 Z" fill="#333"/>
    </marker>
  </defs>

  <rect class="box" x="130" y="10" width="160" height="50" rx="6"/>
  <text class="lbl" x="210" y="30">Original Database</text>
  <text class="cap" x="210" y="46">(at capacity)</text>

  <rect class="box" x="10" y="150" width="110" height="44" rx="6"/>
  <text class="lbl" x="65" y="172">Shard 1</text>
  <text class="cap" x="65" y="186">id: 0–10M</text>

  <rect class="box" x="155" y="150" width="110" height="44" rx="6"/>
  <text class="lbl" x="210" y="172">Shard 2</text>
  <text class="cap" x="210" y="186">id: 10M–20M</text>

  <rect class="box" x="300" y="150" width="110" height="44" rx="6"/>
  <text class="lbl" x="355" y="172">Shard 3</text>
  <text class="cap" x="355" y="186">id: 30M–40M</text>

  <path class="arrow" d="M180,60 C120,90 90,110 65,148"/>
  <path class="arrow" d="M210,60 L210,148"/>
  <path class="arrow" d="M240,60 C300,90 330,110 355,148"/>
</svg>
</div>

Each shard is a standalone database with its own CPU, memory, storage, and connection pool. No single machine holds all the data or handles all the traffic.

### Choosing your shard key

A common statement is "I'm going to shard by [field]." The key is knowing what field to use as your shard key, and why.

What makes a good shard key:

1. The key should have many unique values — like sharding by `user_id`.
2. Values should spread evenly across shards.

**Good shard keys:** `user_id`, `order_id`

**Bad shard keys:**
- `is_premium` (boolean — only two values)
- `created_at` (for a growing table — see the note below)

### Sharding strategies

There are three main strategies, each with different trade-offs.

**1. Range-based sharding**

The most straightforward. It groups records by a continuous range of values. You pick a shard key like `user_id` or `created_at`, then assign value ranges to shards.

```
Shard 1 → user IDs 1     – 1M
Shard 2 → user IDs 1M    – 2M
Shard 3 → user IDs 2M    – 3M
```

> **Note:** if you shard orders by `created_at`, almost all your traffic hits the most recent shard, because users care about recent orders. New writes only go to the latest shard — old shards sit mostly idle.

**2. Hash-based sharding (default)**

Uses a hash function to evenly distribute records across shards. You take a shard key like `user_id` and hash it.

```
shard = hash(user_id) % 4

user 42  → hash(42)  % 4 = shard 2
user 99  → hash(99)  % 4 = shard 3
user 123 → hash(123) % 4 = shard 1
```

**3. Directory-based sharding**

Uses a lookup table to decide where each record lives. Rarely asked about in system design interviews.

### Challenges of sharding

**1. Hot spots and load imbalance**

Some shards can end up handling way more traffic than others — this is called a **hot spot**.

The most common cause is the **celebrity problem**: if you shard users by `user_id`, a celebrity's shard could handle 1000x more traffic than a normal user's shard. Hash-based distribution doesn't help here, because the issue isn't the distribution strategy — it's that some keys are inherently more active than others.

**How to handle it:**

- **Isolate hot keys to a dedicated shard.** If a celebrity account generates too much traffic, move it to a dedicated shard that only handles celebrity accounts. This is why directory-based sharding can be useful for specific cases.
- **Use compound shard keys.** Instead of sharding just by `user_id`, combine it with another dimension, like `hash(user_id + date)`.

**2. Cross-shard operations**

When your data lives on multiple machines, any query that needs data from more than one shard becomes expensive, because you have to wait for all of them to respond and aggregate the results yourself.

**Example:** "Get the top 10 most popular posts globally" has to check every shard, because posts are scattered across all user shards.

**How to handle it:**

- **Design to avoid cross-shard transactions.** This is the best solution. If you shard users by `user_id`, keep all of a user's data on their shard — account balance, transaction history, profile information all on one shard. Now all your transactions are single-shard transactions, which are fast and reliable.
- **Use sagas for multi-shard operations.** When you absolutely need to coordinate across shards, use the saga pattern: break the operation into a sequence of independent steps, each with a compensating action. If step 3 fails, you run the compensating actions for steps 2 and 1 to undo the work.

  **Example:** transferring money between users on different shards:
  1. Deduct money from user A's account (shard 1).
  2. Add money to user B's account (shard 2).
  3. If step 2 fails, refund user A (compensating action).
</content>
