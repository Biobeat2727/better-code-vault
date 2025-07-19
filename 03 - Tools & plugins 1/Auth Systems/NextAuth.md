# NextAuth.js

**NextAuth.js** is an open-source authentication library for Next.js applications. It supports email/password, OAuth, JWT, and third-party providers.

---

## 🚀 Why Use It?

- Native support for Next.js API routes
- Built-in OAuth (Google, GitHub, etc.)
- Session management with JWT or database
- Secure by default

---

## 📦 Installation

```bash
npm install next-auth
```

---

## 🧪 Basic Example

```js
// pages/api/auth/[...nextauth].js

import NextAuth from "next-auth";
import GitHubProvider from "next-auth/providers/github";

export default NextAuth({
  providers: [
    GitHubProvider({
      clientId: process.env.GITHUB_ID,
      clientSecret: process.env.GITHUB_SECRET,
    }),
  ],
});
```

---

## 📚 Resources

- https://next-auth.js.org
