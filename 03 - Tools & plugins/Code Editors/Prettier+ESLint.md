# Prettier + ESLint

**Prettier** is an opinionated code formatter.  
**ESLint** is a linter that finds and fixes problems in your JavaScript/TypeScript code.

---

## 🛠 Setup

```bash
npm install --save-dev prettier eslint
```

Create configs:

- `.prettierrc`
- `.eslintrc.json`

Add scripts to `package.json`:

```json
"scripts": {
  "format": "prettier --write .",
  "lint": "eslint . --ext .js,.jsx,.ts,.tsx"
}
```

---

## 💡 Tip

Use `eslint-plugin-prettier` to integrate Prettier with ESLint for a single linter/formatter pass.

---

## 📚 Resources

- https://prettier.io
- https://eslint.org
