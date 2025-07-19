# GitHub Actions

**GitHub Actions** is a CI/CD platform built into GitHub that automates tasks like running tests, linting, and deploying apps when you push code.

---

## 🚀 Use Cases

- Run unit/integration tests on push
- Lint or format code automatically
- Deploy to Vercel, Netlify, or servers
- Build and publish Docker images

---

## 🧩 Basic Workflow File

```yaml
# .github/workflows/main.yml
name: CI

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - name: Install dependencies
        run: npm install
      - name: Run tests
        run: npm test
```

---

## 🧠 Tips

- Store secrets in GitHub → Settings → Secrets
- Use matrix strategy to test multiple Node versions
- Add caching to speed up installs

---

## 📚 Resources

- https://docs.github.com/en/actions
