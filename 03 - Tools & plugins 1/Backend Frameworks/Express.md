# Express.js

**Express.js** is a minimal and flexible Node.js web application framework that provides a robust set of features for building web and API servers.

---

## 🚀 Why Use Express?

- Simple, lightweight syntax
- Large ecosystem
- Great for REST APIs and middleware
- Works seamlessly with tools like MongoDB, PostgreSQL, Prisma

---

## 📦 Installation

```bash
npm install express
```

---

## 🧪 Example

```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello from Express');
});

app.listen(3000);
```

---

## 🔧 Common Middleware

- `express.json()` – Parse JSON body
- `cors` – Enable CORS
- `helmet` – Secure headers
- `morgan` – Logging

---

## 📚 Resources

- https://expressjs.com
