# Vitest

**Vitest** is a Vite-native test framework that is fast, lightweight, and designed for modern frontend projects.

---

## 🌟 Why Use Vitest?

- Lightning-fast test runs
- Uses Vite for transformations
- Built-in TypeScript, JSX support
- Jest-compatible API

---

## 📦 Installation

```bash
npm install -D vitest
```

In `package.json`:

```json
"scripts": {
  "test": "vitest"
}
```

Create a test:

```js
import { describe, it, expect } from 'vitest';

describe('math', () => {
  it('adds numbers', () => {
    expect(1 + 2).toBe(3);
  });
});
```

---

## ✅ Features

- Snapshot testing
- Built-in coverage with `c8`
- Works great with React, Vue, Svelte
- Can run in watch mode (`vitest --watch`)

---

## 📚 Resources

- https://vitest.dev
