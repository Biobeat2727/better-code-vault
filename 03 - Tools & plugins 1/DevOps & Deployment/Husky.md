# Husky (Pre-commit Hooks)

**Husky** lets you run scripts before commits, ensuring code quality standards like linting, tests, or formatting are met before pushing code.

---

## 🚀 Common Use Cases

- Run linting or formatting before commit
- Prevent commits that break tests
- Enforce commit message conventions

---

## 🛠 Setup

```bash
npm install husky --save-dev
npx husky install
```

Add to `package.json`:

```json
"scripts": {
  "prepare": "husky install"
}
```

Create a pre-commit hook:

```bash
npx husky add .husky/pre-commit "npm run lint && npm run test"
```

---

## 📚 Resources

- https://typicode.github.io/husky
