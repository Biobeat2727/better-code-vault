# Cypress

**Cypress** is a powerful end-to-end (E2E) testing framework that runs directly in the browser. It's used to simulate real user behavior, test full app flows, and verify that everything works across pages, routes, and network requests.

---

## 🎯 Why Use Cypress?

- Tests run in a real browser (Chrome, Edge, Electron)
- Built-in time travel and visual debugger
- Automatically waits for elements (no manual sleeps)
- Full access to network requests and DOM
- Great DX with real-time test runner

---

## 📦 Installation

```bash
npm install --save-dev cypress
```

Then open the Cypress app:
```bash
npx cypress open
```

This creates a `/cypress/` folder in your project with test examples.

---

## 📁 Folder Structure

```
/cypress
  /e2e        # Your tests
  /support    # Reusable commands, hooks
  /fixtures   # Mock data
```

---

## 🧪 Basic Example

```js
// cypress/e2e/home.cy.js

describe('Home Page', () => {
  it('loads and displays title', () => {
    cy.visit('/');
    cy.contains('Welcome to Sandpoint.Events');
  });

  it('navigates to About page', () => {
    cy.get('a[href="/about"]').click();
    cy.url().should('include', '/about');
  });
});
```

---

## 📦 Useful Commands

| Command             | Description |
|---------------------|-------------|
| `cy.visit()`         | Go to a page |
| `cy.get()`           | Select an element |
| `cy.contains()`      | Find by text |
| `cy.click()`         | Click elements |
| `cy.type()`          | Fill inputs |
| `cy.intercept()`     | Mock/fake network requests |
| `cy.url()` / `cy.location()` | Assert URL/location info |

---

## 🧪 Assertions

```js
cy.get('h1').should('have.text', 'Hello')
cy.get('button').should('be.visible')
cy.url().should('include', '/dashboard')
```

---

## 🧑‍🔧 Developer Tips

- Use `data-cy="some-id"` attributes for targeting (safer than classes)
- Run headless mode in CI with `cypress run`
- Use `cy.intercept()` to test network failures or mock APIs

---

## ✅ Learning Checklist

- [ ] Install and configure Cypress
- [ ] Write basic E2E flow tests
- [ ] Use assertions and selectors properly
- [ ] Simulate navigation, input, and user flows
- [ ] Run Cypress in CI/CD pipelines

---

## 📚 Resources

- [https://docs.cypress.io](https://docs.cypress.io)
- [https://github.com/cypress-io/cypress-realworld-app](https://github.com/cypress-io/cypress-realworld-app)
