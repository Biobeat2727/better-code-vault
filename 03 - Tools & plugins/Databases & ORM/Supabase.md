# Supabase

**Supabase** is an open-source Firebase alternative built on PostgreSQL. It includes a hosted database, authentication, file storage, and real-time subscriptions.

---

## 🔧 Why Use Supabase?

- Hosted PostgreSQL
- Auth + database in one
- REST & GraphQL API built in
- Realtime via WebSockets
- Great local dev + production tooling

---

## 📦 Setup

```bash
npm install @supabase/supabase-js
```

Initialize:

```js
import { createClient } from '@supabase/supabase-js';

const supabase = createClient(SUPABASE_URL, SUPABASE_KEY);
```

---

## ✅ Use Cases

- Instant backend for small apps
- User auth + session storage
- Realtime dashboards
- Easily query Postgres via JS

---

## 📚 Docs

- https://supabase.com/docs
