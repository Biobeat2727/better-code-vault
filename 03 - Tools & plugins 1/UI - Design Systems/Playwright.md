# Playwright

**Playwright** is an end-to-end testing framework developed by Microsoft. It supports Chromium, Firefox, and WebKit browsers, and is built for fast, reliable, and cross-browser testing.

---

## 🚀 Features

- Cross-browser (Chromium, Firefox, WebKit)
- Headless and headed testing
- Network interception and mocking
- Built-in tracing and debugging tools
- Parallel test execution

---

## 📦 Installation

```bash
npm install -D @playwright/test
npx playwright install
```

Create a test:

```js
// tests/example.spec.js
const { test, expect } = require('@playwright/test');

test('homepage has title', async ({ page }) => {
  await page.goto('https://example.com');
  await expect(page).toHaveTitle(/Example Domain/);
});
```

---

## 🧠 Common Commands

- `page.goto(url)`
- `page.click(selector)`
- `page.fill(selector, value)`
- `page.locator()`
- `page.waitForSelector()`

---

## 📚 Resources

- https://playwright.dev
