# Redis Interview Questions (Basic Level)

## What is Redis and why is it used?
**A:** Redis is an in-memory key-value data store, often used for caching, session management, and fast data retrieval. It helps improve application performance by storing frequently accessed data in memory.

## How did you use Redis in your project?
**A:** In my project, Redis was mainly used as a caching layer to reduce database load and improve response times. The DevOps team handled most of the setup, but I wrote code to interact with Redis for storing and retrieving cached data.

## What are some common use cases for Redis?
**A:** Redis is commonly used for caching, session storage, pub/sub messaging, and storing temporary data like tokens or counters.

## Did you face any issues while working with Redis?
**A:** Sometimes, cache invalidation or stale data could be a challenge. I worked with the team to ensure proper cache expiry and handled fallback to the database if data was missing in Redis.

## How do you connect your application to Redis?
**A:** I used a Redis client library for my programming language (like Jedis for Java or ioredis for Node.js). Connection details like host and port were managed through configuration files or environment variables.

## What is cache expiry in Redis?
**A:** Cache expiry means setting a time-to-live (TTL) for cached data so that it is automatically removed after a certain period. This helps prevent stale data and keeps the cache up to date.

## What is the difference between Redis and a traditional database?
**A:** Redis stores data in memory for fast access, while traditional databases store data on disk. Redis is ideal for temporary, fast-access data, whereas databases are used for persistent storage.

## What data structures does Redis support?
**A:** Redis supports strings, lists, sets, hashes, sorted sets, bitmaps, and hyperloglogs, allowing flexible ways to store and manipulate data.

## How do you handle data persistence in Redis?
**A:** Redis can persist data using snapshots (RDB) or append-only files (AOF). In my project, persistence was managed by DevOps, but I ensured my code could handle restarts gracefully.

## What is a Redis pub/sub system?
**A:** Redis pub/sub allows applications to send (publish) and receive (subscribe) messages in real time, useful for notifications and event-driven systems.

## How do you monitor Redis performance?
**A:** Monitoring was mainly handled by DevOps, but I added logging and metrics in my application to track cache hits, misses, and latency.

## What is a Redis cluster?
**A:** A Redis cluster is a way to run Redis in a distributed manner, allowing data to be sharded across multiple nodes for scalability and high availability.

## How do you secure Redis?
**A:** Security is managed by DevOps, but I ensured sensitive data was not stored in Redis and used authentication if required. Network-level security and access controls are also important.

## What is the maximum size of a value in Redis?
**A:** The maximum size of a value in Redis is 512 MB.

## Can Redis be used for session management?
**A:** Yes, Redis is commonly used to store user sessions due to its fast read/write capabilities.

## What is a cache miss in Redis?
**A:** A cache miss occurs when the requested data is not found in Redis, so the application fetches it from the database and may store it in Redis for future requests.

## How do you handle cache invalidation in Redis?
**A:** Cache invalidation can be handled by setting TTLs, deleting keys when data changes, or using cache busting strategies. I worked with the team to ensure proper invalidation logic was in place.

## What is the role of DevOps in managing Redis?
**A:** DevOps is responsible for setting up, configuring, monitoring, and scaling Redis. As a developer, I focused on writing code that interacts with Redis and followed best practices for caching and data management.
