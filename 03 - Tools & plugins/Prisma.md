Prisma

**Prisma** is a modern TypeScript ORM that provides type-safe database queries and schema management.

---

## 🔧 Why Prisma?

- Autocompletion in queries
- Type safety with your DB
- Schema migrations
- Great with PostgreSQL, MySQL, SQLite

---

## 📦 Installation

```bash
npm install @prisma/client
npx prisma init
```

Define schema in `prisma/schema.prisma`:

```prisma
model User {
  id    Int     @id @default(autoincrement())
  email String  @unique
  name  String?
}
```

---

## 🔍 Common Commands

- `npx prisma generate` – create client
- `npx prisma migrate dev` – run dev migration
- `npx prisma studio` – GUI for browsing your DB

---

## 🧠 Query Example

```ts
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();
const users = await prisma.user.findMany();
```

---

## 📚 Resources

- https://www.prisma.io/docs
