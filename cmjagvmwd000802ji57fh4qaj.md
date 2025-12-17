---
title: "Understanding Batch Processing: A Beginner's Guide"
seoTitle: "Batch Processing Basics Explained"
seoDescription: "Learn about batch processing and its role in managing high-frequency database writes efficiently using Redis and worker-based batching"
datePublished: Wed Dec 17 2025 20:31:26 GMT+0000 (Coordinated Universal Time)
cuid: cmjagvmwd000802ji57fh4qaj
slug: understanding-batch-processing-a-beginners-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1765282866321/062aa5d8-c016-4618-be22-cc685cafb925.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1766003448311/225a695e-8781-474a-b052-c12253297470.png
tags: backend-developments, batch-processing

---

Imagine being a celebrity with a massive following. Every time you post, you receive an overwhelming number of likes. Now, picture getting 100,000 likes within minutes. Would Instagram update its database 100,000 times in one minute to reflect this?

## The Reality of Database Updates

When a celebrity posts a photo and receives 100,000 likes in minutes, a critical question arises:

> Would Instagram really hit its database 100,000 times in one minute just to update a like counter?

The answer is simple: **no — and it shouldn’t.**

This is where **batch processing** becomes essential.

In this guide, we’ll explore:

* The pitfalls of naïve database updates at scale
    
* How large systems manage massive write traffic
    
* A practical batch-processing architecture
    
* Implementation using **Redis and worker-based batching**
    

This is not just theory; it’s how production systems operate.

## The Naïve Approach and Its Limitations

A straightforward method to handle likes involves:

1. User clicks Like
    
2. Backend increments `likes_count` in the database
    
3. Immediate database write
    

### Challenges at Scale

If 100,000 users like a post within a minute, this results in:

* 100,000 database write operations
    
* Heavy lock contention on the same row
    
* Increased latency for all users
    
* Risk of database throttling or outages
    

Relational databases are not designed for extremely high-frequency writes on the same record. If Instagram followed this approach, their database would struggle to cope.

## How Large Systems Address the Problem

Big systems adhere to a key principle:

> **User experience must be fast; database writes can be delayed.**

A like doesn’t need to be immediately stored in permanent storage. A short delay is acceptable and invisible to users.

The strategy involves:

* Quickly accepting likes
    
* Storing them in a fast in-memory system
    
* Persisting them to the database in batches
    

## Understanding Batch Processing

**Batch processing** involves:

* Collecting multiple events over time
    
* Processing them together as a group
    
* Dramatically reducing system load
    

Instead of:

> 100,000 likes → 100,000 database writes

We achieve:

> 100,000 likes → 1 Redis counter → 1 batched database write

This results in a **100,000x improvement** in write efficiency.

## Architectural Overview

Here’s the high-level architecture:

1. User clicks Like
    
2. Backend updates Redis (fast, in-memory)
    
3. A background worker runs periodically
    
4. Worker reads accumulated likes from Redis
    
5. Worker updates the database in batches
    

## Why Choose Redis?

Redis is ideal for this scenario because:

* It’s in-memory, making it extremely fast
    
* Supports atomic operations (`INCR`)
    
* Can handle millions of operations per second
    
* Temporary data storage is acceptable
    

A database ensures **durability**, while Redis provides **speed**.

## Storing Likes in Redis

When a user likes a post:

```typescript
redis.incr(`post:likes:${postId}`)
```

Benefits include:

* O(1) operation
    
* No database lock
    
* Immediate user response
    

At this stage:

* The UI can display the updated count
    
* The database remains untouched
    

## The Role of the Batch Worker

A background worker runs every few seconds or minutes.

### Worker Responsibilities

1. Fetch all like counters from Redis
    
2. Aggregate them
    
3. Write updates to the database
    
4. Reset Redis counters
    

Pseudo-flow:

```apache
for each postId in redisKeys:
  likes = redis.get(postId)
  UPDATE posts SET likes_count = likes_count + likes
  redis.del(postId)
```

This reduces thousands of updates to **one update per post per interval**.

## Determining Batch Size and Frequency

This is a design decision:

* Every 5 seconds → more real-time, more database writes
    
* Every 1 minute → fewer database writes, slight delay
    

Production systems adjust based on:

* Traffic
    
* Database capacity
    
* Acceptable data freshness
    

Instagram doesn’t require millisecond-accurate likes, nor do most apps.

## Handling Edge Cases

### Redis Crash

If Redis crashes, likes in memory may be lost.

Mitigations:

* Enable Redis persistence (AOF/RDB)
    
* Accept minor data loss for non-critical metrics
    

Likes are **eventually consistent**, not financial transactions.

### Worker Failure

If a worker crashes mid-batch:

* Redis data remains intact
    
* The next worker run continues processing
    

This ensures the system is **fault-tolerant**.

### Duplicate Updates

Workers must be:

* Idempotent
    
* Or carefully delete Redis keys only after successful database writes
    

This prevents double-counting likes.

## Why This Pattern is Industry Standard

This approach is used for:

* Like counters
    
* View counts
    
* Follower counts
    
* Analytics events
    
* Notifications
    

Any system with **high write frequency** employs batching. Similar patterns are found in:

* Instagram
    
* Twitter
    
* YouTube
    
* Netflix analytics
    

## Key Takeaways

* Databases shouldn’t handle extremely high-frequency writes
    
* Redis absorbs traffic spikes
    
* Batch workers ensure system stability
    
* Eventual consistency is acceptable for metrics
    

For any application that might go viral, **batch processing is essential**.

## Final Thought

Next time you see a post jump from 10K to 100K likes instantly, remember:

Behind the scenes, no database is being overwhelmed. A smart batching system is efficiently managing the load.

For students preparing for backend interviews or system design rounds, this pattern is invaluable. Understand it, implement it, and discuss it with confidence.

### Implementation

%[https://youtu.be/SeOu8Lc0Z_M]