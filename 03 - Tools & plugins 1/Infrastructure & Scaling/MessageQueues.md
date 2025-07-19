# Message Queues (BullMQ, Kafka)

Message queues decouple services and let you process tasks asynchronously, at scale.

---

## 🎯 Why Use a Message Queue?

- Handle long-running tasks outside request/response cycle
- Prevent blocking the main thread
- Scale workers independently
- Retry failed jobs

---

## 🔧 BullMQ (Node + Redis)

```bash
npm install bullmq
```

```js
import { Queue } from 'bullmq';

const queue = new Queue('emails');
await queue.add('sendWelcomeEmail', { userId: 123 });
```

BullMQ uses Redis and is great for job queues in Node.js apps.

---

## 🔧 Kafka (Enterprise-Grade)

Apache Kafka is a distributed streaming platform for high-throughput, fault-tolerant event processing.

Use cases:
- Stream processing
- Logs, telemetry
- Pub/Sub at scale

Node library: `kafkajs`

---

## 📚 Resources

- https://docs.bullmq.io
- https://kafka.apache.org
