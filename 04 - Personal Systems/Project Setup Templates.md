
Standardized project templates save time, enforce best practices, and ensure consistency across projects. Use these templates to start new projects quickly with proper tooling and structure.

---

## 🎯 Template Philosophy

**Start with the End in Mind**: Include deployment, testing, and monitoring from day one  
**Enforce Best Practices**: Bake good practices into the template structure  
**Minimize Setup Time**: Get from idea to first commit in under 10 minutes  
**Stay Flexible**: Templates should guide, not constrain creativity  
**Document Everything**: Include clear README and getting started guide

---

## 🏗️ Frontend Project Template

### React + TypeScript + Vite Template

```bash
# Quick setup command
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install

# Additional dependencies
npm install -D \
  @types/node \
  eslint \
  @typescript-eslint/eslint-plugin \
  @typescript-eslint/parser \
  prettier \
  husky \
  lint-staged \
  @testing-library/react \
  @testing-library/jest-dom \
  @testing-library/user-event \
  vitest \
  jsdom
```

### Folder Structure

```
my-react-app/
├── public/
│   ├── favicon.ico
│   └── index.html
├── src/
│   ├── components/
│   │   ├── common/
│   │   │   ├── Button/
│   │   │   │   ├── Button.tsx
│   │   │   │   ├── Button.test.tsx
│   │   │   │   └── index.ts
│   │   │   └── index.ts
│   │   └── pages/
│   │       ├── Home/
│   │       └── About/
│   ├── hooks/
│   │   ├── useApi.ts
│   │   └── useLocalStorage.ts
│   ├── services/
│   │   ├── api.ts
│   │   └── auth.ts
│   ├── types/
│   │   ├── api.ts
│   │   └── user.ts
│   ├── utils/
│   │   ├── constants.ts
│   │   ├── helpers.ts
│   │   └── validators.ts
│   ├── styles/
│   │   ├── globals.css
│   │   └── components.css
│   ├── App.tsx
│   ├── App.test.tsx
│   ├── main.tsx
│   └── vite-env.d.ts
├── tests/
│   ├── setup.ts
│   └── __mocks__/
├── .env.example
├── .gitignore
├── .eslintrc.js
├── .prettierrc
├── package.json
├── tsconfig.json
├── vite.config.ts
└── README.md
```

### Configuration Files

```json
// package.json scripts
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "test:coverage": "vitest --coverage",
    "lint": "eslint src --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
    "lint:fix": "eslint src --ext ts,tsx --fix",
    "format": "prettier --write src/",
    "type-check": "tsc --noEmit",
    "prepare": "husky install"
  }
}
```

```javascript
// vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@components': path.resolve(__dirname, './src/components'),
      '@hooks': path.resolve(__dirname, './src/hooks'),
      '@services': path.resolve(__dirname, './src/services'),
      '@utils': path.resolve(__dirname, './src/utils'),
      '@types': path.resolve(__dirname, './src/types')
    }
  },
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: './tests/setup.ts'
  }
})
```

---

## 🖥️ Backend Project Template

### Node.js + Express + TypeScript Template

```bash
# Initialize project
mkdir my-api && cd my-api
npm init -y

# Core dependencies
npm install express cors helmet morgan dotenv
npm install express-rate-limit express-validator

# Development dependencies
npm install -D \
  typescript \
  @types/node \
  @types/express \
  @types/cors \
  @types/morgan \
  ts-node \
  nodemon \
  eslint \
  @typescript-eslint/eslint-plugin \
  @typescript-eslint/parser \
  prettier \
  jest \
  @types/jest \
  supertest \
  @types/supertest
```

### Backend Folder Structure

```
my-api/
├── src/
│   ├── controllers/
│   │   ├── authController.ts
│   │   ├── userController.ts
│   │   └── index.ts
│   ├── middleware/
│   │   ├── auth.ts
│   │   ├── errorHandler.ts
│   │   ├── validation.ts
│   │   └── index.ts
│   ├── models/
│   │   ├── User.ts
│   │   └── index.ts
│   ├── routes/
│   │   ├── auth.ts
│   │   ├── users.ts
│   │   └── index.ts
│   ├── services/
│   │   ├── authService.ts
│   │   ├── emailService.ts
│   │   └── index.ts
│   ├── types/
│   │   ├── auth.ts
│   │   ├── user.ts
│   │   └── index.ts
│   ├── utils/
│   │   ├── constants.ts
│   │   ├── helpers.ts
│   │   ├── logger.ts
│   │   └── validators.ts
│   ├── config/
│   │   ├── database.ts
│   │   └── index.ts
│   ├── app.ts
│   └── server.ts
├── tests/
│   ├── setup.ts
│   ├── controllers/
│   ├── services/
│   └── __mocks__/
├── .env.example
├── .gitignore
├── jest.config.js
├── tsconfig.json
├── package.json
└── README.md
```

### Backend Configuration

```typescript
// src/app.ts
import express from 'express';
import cors from 'cors';
import helmet from 'helmet';
import morgan from 'morgan';
import rateLimit from 'express-rate-limit';

import { errorHandler } from './middleware/errorHandler';
import routes from './routes';

const app = express();

// Security middleware
app.use(helmet());
app.use(cors({
  origin: process.env.FRONTEND_URL || 'http://localhost:3000',
  credentials: true
}));

// Rate limiting
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});
app.use(limiter);

// Logging
app.use(morgan('combined'));

// Body parsing
app.use(express.json({ limit: '10mb' }));
app.use(express.urlencoded({ extended: true }));

// Routes
app.use('/api', routes);

// Error handling
app.use(errorHandler);

export default app;
```

---

## 📱 Full-Stack Template

### Next.js + TypeScript + Prisma Template

```bash
# Create Next.js app
npx create-next-app@latest my-fullstack-app --typescript --tailwind --eslint --app

cd my-fullstack-app

# Add additional dependencies
npm install \
  @prisma/client \
  @auth/prisma-adapter \
  next-auth \
  zod \
  react-hook-form \
  @hookform/resolvers

npm install -D \
  prisma \
  @types/bcryptjs \
  bcryptjs
```

### Full-Stack Structure

```
my-fullstack-app/
├── app/
│   ├── api/
│   │   ├── auth/
│   │   ├── users/
│   │   └── posts/
│   ├── (auth)/
│   │   ├── login/
│   │   └── register/
│   ├── dashboard/
│   ├── globals.css
│   ├── layout.tsx
│   └── page.tsx
├── components/
│   ├── ui/
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   └── Modal.tsx
│   ├── forms/
│   │   ├── LoginForm.tsx
│   │   └── UserForm.tsx
│   └── layout/
│       ├── Header.tsx
│       ├── Footer.tsx
│       └── Sidebar.tsx
├── lib/
│   ├── auth.ts
│   ├── db.ts
│   ├── validations.ts
│   └── utils.ts
├── prisma/
│   ├── schema.prisma
│   └── seed.ts
├── types/
│   ├── auth.ts
│   └── user.ts
├── .env.example
├── .env.local
├── next.config.js
├── tailwind.config.js
└── tsconfig.json
```

---

## 🔧 Development Tooling Setup

### Git Hooks with Husky

```bash
# Install husky
npm install -D husky lint-staged

# Initialize husky
npx husky install

# Add pre-commit hook
npx husky add .husky/pre-commit "npx lint-staged"

# Add commit-msg hook for conventional commits
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
```

```json
// package.json
{
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{json,css,md}": [
      "prettier --write"
    ]
  }
}
```

### ESLint Configuration

```javascript
// .eslintrc.js
module.exports = {
  extends: [
    'eslint:recommended',
    '@typescript-eslint/recommended',
    'next/core-web-vitals', // For Next.js projects
    'prettier'
  ],
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint'],
  root: true,
  env: {
    node: true,
    browser: true,
    es2021: true
  },
  rules: {
    // Custom rules
    '@typescript-eslint/no-unused-vars': 'error',
    '@typescript-eslint/no-explicit-any': 'warn',
    'prefer-const': 'error',
    'no-console': 'warn'
  }
};
```

### Prettier Configuration

```json
// .prettierrc
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false
}
```

---

## 🧪 Testing Setup

### Jest Configuration

```javascript
// jest.config.js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  roots: ['<rootDir>/src', '<rootDir>/tests'],
  testMatch: [
    '**/__tests__/**/*.+(ts|tsx|js)',
    '**/*.(test|spec).+(ts|tsx|js)'
  ],
  transform: {
    '^.+\\.(ts|tsx)$': 'ts-jest'
  },
  collectCoverageFrom: [
    'src/**/*.{ts,tsx}',
    '!src/**/*.d.ts',
    '!src/**/*.stories.tsx'
  ],
  coverageThreshold: {
    global: {
      branches: 70,
      functions: 70,
      lines: 70,
      statements: 70
    }
  },
  setupFilesAfterEnv: ['<rootDir>/tests/setup.ts']
};
```

### Vitest Configuration (for Vite projects)

```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: './tests/setup.ts',
    css: true,
    coverage: {
      reporter: ['text', 'json', 'html'],
      exclude: [
        'node_modules/',
        'tests/',
        '**/*.config.*',
        '**/*.d.ts'
      ]
    }
  }
});
```

---

## 🚀 Deployment Configuration

### Docker Setup

```dockerfile
# Dockerfile
FROM node:18-alpine AS base

# Install dependencies only when needed
FROM base AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --only=production && npm cache clean --force

# Rebuild the source code only when needed
FROM base AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production image, copy all the files and run next
FROM base AS runner
WORKDIR /app

ENV NODE_ENV=production

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs

EXPOSE 3000

ENV PORT 3000

CMD ["node", "server.js"]
```

### GitHub Actions

```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run type check
        run: npm run type-check
      
      - name: Run linting
        run: npm run lint
      
      - name: Run tests
        run: npm run test:coverage
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3

  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build application
        run: npm run build
      
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.ORG_ID }}
          vercel-project-id: ${{ secrets.PROJECT_ID }}
          vercel-args: '--prod'
```

---

## 📋 Environment Configuration

### Environment Variables Template

```bash
# .env.example

# Database
DATABASE_URL="postgresql://username:password@localhost:5432/mydb"

# Authentication
NEXTAUTH_SECRET="your-secret-key-here"
NEXTAUTH_URL="http://localhost:3000"

# OAuth Providers
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"

# API Keys
STRIPE_SECRET_KEY="sk_test_..."
STRIPE_PUBLISHABLE_KEY="pk_test_..."

# Email
SMTP_HOST="smtp.gmail.com"
SMTP_PORT=587
SMTP_USER="your-email@gmail.com"
SMTP_PASS="your-app-password"

# File Upload
CLOUDINARY_CLOUD_NAME="your-cloud-name"
CLOUDINARY_API_KEY="your-api-key"
CLOUDINARY_API_SECRET="your-api-secret"

# Redis (optional)
REDIS_URL="redis://localhost:6379"

# Analytics (optional)
GOOGLE_ANALYTICS_ID="GA_MEASUREMENT_ID"
```

---

## 📖 Documentation Templates

### README Template

````markdown
# Project Name

Brief description of what this project does and who it's for.

## 🚀 Quick Start

```bash
# Clone the repository
git clone https://github.com/username/project-name.git
cd project-name

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env.local
# Edit .env.local with your values

# Run the development server
npm run dev
````

## 📁 Project Structure

```
src/
├── components/     # Reusable UI components
├── pages/         # Next.js pages
├── lib/           # Utility functions and configurations
├── types/         # TypeScript type definitions
└── styles/        # CSS and styling files
```

## 🛠️ Tech Stack

- **Frontend**: Next.js 14, TypeScript, Tailwind CSS
- **Backend**: Next.js API Routes, Prisma
- **Database**: PostgreSQL
- **Authentication**: NextAuth.js
- **Deployment**: Vercel

## 📝 Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run start` - Start production server
- `npm run test` - Run tests
- `npm run lint` - Run ESLint
- `npm run type-check` - Run TypeScript checks

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License.

````

---

## 🔄 Template Automation

### Project Generator Script
```bash
#!/bin/bash
# create-project.sh

PROJECT_NAME=$1
TEMPLATE_TYPE=$2

if [ -z "$PROJECT_NAME" ] || [ -z "$TEMPLATE_TYPE" ]; then
  echo "Usage: ./create-project.sh <project-name> <template-type>"
  echo "Template types: react, nextjs, express, fullstack"
  exit 1
fi

case $TEMPLATE_TYPE in
  "react")
    npm create vite@latest $PROJECT_NAME -- --template react-ts
    ;;
  "nextjs")
    npx create-next-app@latest $PROJECT_NAME --typescript --tailwind --eslint --app
    ;;
  "express")
    mkdir
````