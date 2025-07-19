# Prisma (ORM)

**Prisma** is a modern TypeScript ORM that provides type-safe database access and powerful migrations.

---

## 🔧 Why Use Prisma?

- Auto-complete and type safety
- Clean, readable queries
- Migrations + schema generation
- Works with PostgreSQL, MySQL, SQLite

---

## 📦 Setup

```bash
npm install @prisma/client
npx prisma init
```

---

## 📁 Files

- `schema.prisma`: define models and DB type
- `prisma/migrations/`: stores migrations
- `prisma/client/`: generated type-safe client

---

## 🧪 Example Model

```prisma
model User {
  id    Int    @id @default(autoincrement())
  email String @unique
  name  String?
}
```

Then:

```ts
const users = await prisma.user.findMany();
```

---

## 📚 Docs

- https://www.prisma.io/docs
