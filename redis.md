# Redis Overview

Redis is an open-source, in-memory data structure store that can be used as a cache, message broker, and database. It is known for its performance, simplicity, and support for various data structures. This document provides an overview of Redis and covers some common commands.

## Introduction

### What is Redis?

Redis (Remote Dictionary Server) is an advanced key-value store that supports a wide range of data structures such as strings, hashes, lists, sets, and more. It is often used to build scalable and high-performance applications due to its in-memory nature, low-latency operations, and support for advanced features like transactions and pub/sub.

### Key Features

- **In-Memory Storage:** Redis stores data in memory, enabling fast read and write operations.
- **Data Structures:** Redis supports various data structures, making it versatile for different use cases.
- **Persistence:** While primarily an in-memory database, Redis can be configured for data persistence.
- **Pub/Sub:** Redis provides a publish/subscribe mechanism for building real-time applications.
- **Atomic Operations:** Redis supports atomic operations on these data structures, ensuring data consistency.

## Installation

### Docker

```sh
docker run --name redis -d -p 6379:6379 redis
```

## Redis Commands

### Setting and Retrieving Values

```bash
SET key value
GET key
DEL key [key ...]
```

### Working with Strings

```sh
APPEND key value
STRLEN key
```

### Working with Lists

```sh
LPUSH key value [value ...]
RPUSH key value [value ...]
LRANGE key start stop
```

### Working with Sets
```sh
SADD key member [member ...]
SMEMBERS key
```

### Expiring Keys
```sh
EXPIRE key seconds
TTL key
```

### Publish/Subcribe
```sh
PUBLISH channel message
SUBSCRIBE channel [channel ...]
```

## Code Example

### NodeJS

Install `ioredis` library:

```sh
yarn add ioredis
```

Create `RedisService`:
```typescript
import { Redis } from 'ioredis';

// Assuming you've set the appropriate environment variables (REDIS_HOST, REDIS_PORT)

class RedisService {
  private client: Redis;

  constructor() {
    this.client = new Redis({
      host: process.env.REDIS_HOST,
      port: Number(process.env.REDIS_PORT),
    });
  }

  getClient(): Redis {
    return this.client;
  }
}
```

Example usage:

```typescript
// Example usage

async function example() {
  const redisService = new RedisService();
  const redisClient = redisService.getClient();

  // Set a key-value pair
  await redisClient.set('mykey', 'Hello, Redis!');

  // Get the value for the key
  const result = await redisClient.get('mykey');
  console.log('Value:', result);

  // Close the Redis connection when done
  await redisClient.quit();
}

// Run the example
example().catch(error => console.error('Error:', error));
```
