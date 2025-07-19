# Jest

**Jest** is a JavaScript testing framework built by Meta, designed for unit and integration testing of frontend and backend applications. It works out of the box with React and supports mocking, snapshot testing, and code coverage.

---

## ⚙️ Why Use Jest?

- Fast and simple testing syntax
- Zero config for most JS projects
- Built-in mocking and timers
- Snapshot testing support
- Integrates well with React, Next.js, and Node

---

## 📦 Installation

```bash
npm install --save-dev jest
```

For React:

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

Add script to `package.json`:

```json
"scripts": {
  "test": "jest"
}
```

---

## 🧪 Example Test

```js
// sum.js
function sum(a, b) {
  return a + b;
}
module.exports = sum;

// sum.test.js
const sum = require('./sum');

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

Run with:
```bash
npm test
```

---

## 🧠 Key Matchers

| Matcher         | Description |
|-----------------|-------------|
| `toBe()`        | Exact match |
| `toEqual()`     | Deep equality |
| `toContain()`   | Array or string includes |
| `toHaveLength()`| Check array/string length |
| `toThrow()`     | Error handling |

---

## 📚 Test Structure

```
/src
  /components
    MyComponent.jsx
    MyComponent.test.js
```

- Name test files with `.test.js` or `.spec.js`
- Keep them close to the source files

---

## 🔧 Common Config Options (jest.config.js)

```js
module.exports = {
  testEnvironment: "jsdom",
  setupFilesAfterEnv: ["@testing-library/jest-dom/extend-expect"],
  moduleNameMapper: {
    "\.(css|less)$": "<rootDir>/styleMock.js"
  }
}
```

---

## 🔍 Code Coverage

```bash
jest --coverage
```

Shows what % of your code is being tested.

---

## 🔄 Mocking

```js
jest.mock('./api');

api.fetchData.mockResolvedValue({ result: 123 });
```

Mock functions, timers, modules, and more.

---

## 🧠 Learning Checklist

- [ ] Write unit tests with Jest
- [ ] Use matchers like `toBe`, `toEqual`, `toThrow`
- [ ] Mock functions and modules
- [ ] Integrate Jest with React Testing Library
- [ ] Generate code coverage reports
