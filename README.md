# BullMQ Lazy Scheduler

Distributed job scheduler built on BullMQ, Redis, and MongoDB with lazy execution and durable persistence.

**Status: Alpha**
This project is under active development ⚠️. APIs and behavior may change. Not yet recommended for mission-critical production use.

---

## Why This Exists

Most schedulers force a trade-off:

* Redis/BullMQ → fast but not durable ⚡
* Mongo-based schedulers → durable but slower 💾

This project combines both:

* MongoDB as the source of truth 📦
* Redis (BullMQ) as the execution layer ⚙️

Result:

* durable job storage
* fast execution
* scalable processing 🚀

---

## Features

* Mongo-backed persistence 📦
* BullMQ execution engine ⚡
* Lazy scheduling (jobs are enqueued only when needed) 💤
* Reconciliation engine to ensure consistency 🔁
* Registry-based job handlers 🧩
* TypeScript-first API

---

## Architecture

```id="z94of9"
Client → Scheduler → MongoDB (source of truth)
                         ↓
                 Reconciliation Engine
                         ↓
                  BullMQ (Redis)
                         ↓
                       Worker
```

### Flow

1. Job is scheduled and stored in MongoDB 📦
2. Reconciliation identifies due jobs 🔁
3. Job is pushed to BullMQ ⚙️
4. Worker processes the job
5. State is updated in MongoDB

---

## Installation

```id="gnkgws"
npm install bullmq-lazy-scheduler
```

---

## Quick Example

```ts id="g5fjnc"
import { schedule } from 'bullmq-lazy-scheduler';

await schedule({
  name: 'send-email',
  data: { userId: 1 },
  delay: 5000
});
```

---

## Registering Jobs

```ts id="x2xf2c"
import { registerJob } from 'bullmq-lazy-scheduler';

registerJob('send-email', async (data) => {
  console.log('Sending email to:', data.userId);
});
```

---

## Running the Worker

```ts id="r2kvrz"
import { startWorker } from 'bullmq-lazy-scheduler';

startWorker();
```

---

## API Overview

### Scheduling

```ts id="0qeyko"
schedule({ name, data, delay })
cancel(jobId)
get(jobId)
list(filters)
```

---

## Design Principles

* MongoDB is the source of truth 📦
* Redis is an execution layer, not storage ⚙️
* Jobs are persisted before execution
* System is eventually consistent with correction (reconciliation) 🔁

---

## Limitations (Alpha)

Current gaps:

* No distributed locking (multi-instance safety) ⚠️
* No strict idempotency guarantees
* No cron / recurring jobs
* No dead letter queue
* No advanced retry strategies
* No metrics or dashboard
* No rate limiting / backpressure

---

## Roadmap

### Stability

* Distributed locking (Redis-based)
* Idempotent job enqueue
* Mongo TTL cleanup

### Reliability

* Retry strategies
* Dead letter queue
* Failure classification

### Features

* Cron / recurring jobs
* Priority queues
* Rate limiting

### Observability

* Metrics
* Dashboard
* Tracing

---

## Comparison

| Feature         | BullMQ | Agenda | This   |
| --------------- | ------ | ------ | ------ |
| Speed           | High ⚡ | Low    | High ⚡ |
| Persistence     | No     | Yes 💾 | Yes 💾 |
| Lazy Scheduling | No     | No     | Yes 💤 |

---

## When To Use

* You need durable delayed jobs 📦
* You already use BullMQ and Redis ⚙️
* You want Mongo-backed scheduling

---

## When Not To Use

* Mission-critical production systems (yet) ⚠️
* Strong consistency requirements
* Multi-region distributed systems

---

## Tech Stack

* BullMQ ⚙️
* Redis ⚡
* MongoDB 📦
* Mongoose
* Adapters ( for Persistent storage )

---

## Keywords

bullmq scheduler, redis job queue, mongodb scheduler, nodejs background jobs, distributed job scheduler

---

## License

MIT

---

## Final Note

This project is not trying to replace existing schedulers.

It aims to combine:
* Simplicity
* Redis speed ⚡
* Mongo durability 💾

into a single, scalable scheduling model 🚀
