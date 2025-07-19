# SQLite

**SQLite** is a lightweight, file-based relational database that requires no server and works well for local development or embedded apps.

---

## 🔧 Why Use SQLite?

- Simple: one file, zero config
- Great for prototyping or local dev
- Compatible with Prisma, TypeORM, etc.
- Can be versioned with your app

---

## 🛠 Tools

- DB Browser for SQLite (GUI)
- Prisma ORM for schema + access
- SQLite3 CLI

---

## 📦 Example (with Prisma)

In `schema.prisma`:

```prisma
datasource db {
  provider = "sqlite"
  url      = "file:./dev.db"
}
```

Then run:

```bash
npx prisma migrate dev --name init
```

---

## 📚 Docs

- https://sqlite.org
