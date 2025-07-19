# JSON Web Tokens (JWT)

**JWTs** are a compact, URL-safe way to represent claims securely between two parties. They're used for authentication, session management, and authorization.

---

## 🧠 Anatomy of a JWT

```txt
HEADER.PAYLOAD.SIGNATURE
```

- Header: algorithm + type
- Payload: user data
- Signature: verifies token integrity

---

## 🔐 Use Cases

- API authentication (bearer tokens)
- Stateless session handling
- Token-based permissions

---

## 🧪 Example

```js
import jwt from 'jsonwebtoken';

const token = jwt.sign({ userId: 123 }, 'secret', { expiresIn: '1h' });
const decoded = jwt.verify(token, 'secret');
```

---

## 🛡️ Best Practices

- Never store JWTs in localStorage (use cookies + httpOnly)
- Set expiration times
- Use strong secrets or RSA key pairs

---

## 📚 Resources

- https://jwt.io
- https://auth0.com/docs/secure/tokens/json-web-tokens
