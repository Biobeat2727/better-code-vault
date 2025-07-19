# Fastify

**Fastify** is a modern, fast web framework for Node.js focused on performance and low overhead.

---

## 🚀 Why Use Fastify?

- 10x faster than Express in some cases
- JSON Schema-based validation
- Great for high-performance APIs
- Plugin-based architecture

---

## 📦 Installation

```bash
npm install fastify
```

---

## 🧪 Example

```js
const fastify = require('fastify')();

fastify.get('/', async (request, reply) => {
  return { hello: 'world' };
});

fastify.listen({ port: 3000 });
```

---

## 🔧 Plugins

- `@fastify/cors`
- `@fastify/jwt`
- `@fastify/swagger`

---

## 📚 Resources

- https://www.fastify.io
