# REST & GraphQL APIs

Two common approaches for building APIs: **REST** (traditional) and **GraphQL** (flexible query-based).

---

## 📌 REST APIs

- Uses HTTP methods: GET, POST, PUT, DELETE
- Resource-oriented (`/users`, `/products`)
- Simpler to implement, easier caching

### Example

```http
GET /api/posts/123
```

---

## ⚛️ GraphQL APIs

- Clients request exactly the data they need
- Single endpoint: `/graphql`
- Schema defines types and relationships

### Example Query

```graphql
query {
  post(id: 123) {
    title
    author {
      name
    }
  }
}
```

---

## 🔧 Tools

- **REST**: Express, Fastify, Django, Flask
- **GraphQL**: Apollo Server, Graphene (Python), urql

---

## 🧠 Pros & Cons

| Feature     | REST              | GraphQL           |
|-------------|-------------------|--------------------|
| Simplicity  | ✅ Easy            | ❌ More setup       |
| Flexibility | ❌ Fixed shape     | ✅ Query-specific   |
| Overfetch   | ❌ Common          | ✅ Avoided          |
| Caching     | ✅ Easy (HTTP)     | ❌ Harder (manual)  |

---

## 📚 Resources

- https://graphql.org
- https://restfulapi.net
