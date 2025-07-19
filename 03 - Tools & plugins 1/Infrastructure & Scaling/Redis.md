# Redis (Caching)

**Redis** is an in-memory data structure store used as a database, cache, and message broker. It's extremely fast and ideal for session storage and caching.

---

## 🚀 Why Use Redis?

- Lightning-fast read/write
- Store session data, tokens, API results
- TTL (Time to Live) support for expiring keys
- Pub/Sub and stream support

---

## 📦 Installation

```bash
npm install redis
```

```js
import { createClient } from 'redis';

const redis = createClient();
await redis.connect();

await redis.set('key', 'value');
const val = await redis.get('key');
```

---

## 🔧 Common Use Cases

- Cache database queries
- Store JWT sessions
- Rate limiting
- Pub/Sub notifications

---

## 📚 Docs

- https://redis.io/docs
