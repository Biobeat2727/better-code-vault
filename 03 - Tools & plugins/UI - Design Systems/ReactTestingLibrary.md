# React Testing Library

**React Testing Library (RTL)** is a lightweight testing library for React components that encourages testing from the user’s perspective rather than the internal implementation.

---

## 🎯 Philosophy

> "The more your tests resemble the way your software is used, the more confidence they can give you."

RTL promotes:
- Minimal mocking
- Testing real DOM output
- Avoiding implementation details

---

## 📦 Installation

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

Set up `jest.setup.js`:

```js
import '@testing-library/jest-dom';
```

Add in `jest.config.js`:

```js
setupFilesAfterEnv: ['<rootDir>/jest.setup.js']
```

---

## 🧪 Basic Example

```jsx
// Greeting.jsx
export default function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// Greeting.test.js
import { render, screen } from '@testing-library/react';
import Greeting from './Greeting';

test('renders greeting with name', () => {
  render(<Greeting name="Davey" />);
  expect(screen.getByText('Hello, Davey!')).toBeInTheDocument();
});
```

---

## 🔍 Common Queries

| Function | Use |
|----------|-----|
| `getByText()` | Find element by visible text |
| `getByRole()` | By accessibility role (`button`, `heading`) |
| `getByLabelText()` | For forms |
| `getByTestId()` | If nothing else applies (fallback) |

Also includes `queryBy`, `findBy`, and `getAllBy` versions.

---

## 🧠 Useful Matchers (from jest-dom)

- `toBeInTheDocument()`
- `toHaveTextContent()`
- `toHaveClass()`
- `toBeDisabled()`
- `toBeVisible()`

---

## 🧑‍🔧 Tips

- Test **behavior**, not **implementation**
- Simulate events with `user-event`:

```bash
npm install --save-dev @testing-library/user-event
```

```js
import userEvent from '@testing-library/user-event';

userEvent.click(screen.getByText('Submit'));
```

---

## ✅ Learning Checklist

- [ ] Render components and check visible output
- [ ] Use `screen` queries to select elements
- [ ] Simulate user events with `user-event`
- [ ] Use jest-dom matchers like `toBeInTheDocument`
- [ ] Avoid relying on implementation details (class names, etc.)

---

## 📚 Resources

- [https://testing-library.com/docs/react-testing-library/intro](https://testing-library.com/docs/react-testing-library/intro)
- [https://kentcdodds.com](https://kentcdodds.com) (creator of RTL, lots of great content)
