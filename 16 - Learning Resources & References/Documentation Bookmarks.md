# Documentation Bookmarks

Essential documentation and reference materials organized for quick access during development. These are the authoritative sources you'll reference constantly throughout your coding journey.

---

## 🌐 Core Web Technologies

### HTML & CSS

```js
const htmlCssResources = {
  // Primary references
  authoritative: [
    {
      name: "MDN Web Docs - HTML",
      url: "https://developer.mozilla.org/en-US/docs/Web/HTML",
      description: "Complete HTML reference and tutorials",
      useFor: ["Element references", "Semantic markup", "Accessibility"],
      bookmark: "Essential for all web developers",
      quickAccess: "MDN HTML elements list"
    },
    {
      name: "MDN Web Docs - CSS",
      url: "https://developer.mozilla.org/en-US/docs/Web/CSS",
      description: "Comprehensive CSS documentation",
      useFor: ["Property references", "Layout techniques", "Browser compatibility"],
      bookmark: "Daily reference for CSS development",
      quickAccess: "CSS properties alphabetical list"
    },
    {
      name: "CSS-Tricks",
      url: "https://css-tricks.com/",
      description: "Practical CSS techniques and solutions",
      useFor: ["CSS techniques", "Layout examples", "Performance tips"],
      bookmark: "Great for learning modern CSS patterns",
      highlights: ["Complete Guide to Flexbox", "Complete Guide to Grid"]
    }
  ],
  
  // Specialized resources
  specialized: [
    {
      name: "Can I Use",
      url: "https://caniuse.com/",
      description: "Browser compatibility tables",
      useFor: ["Feature support checking", "Progressive enhancement decisions"],
      bookmark: "Check before using new CSS features"
    },
    {
      name: "CSS Grid Generator",
      url: "https://cssgrid-generator.netlify.app/",
      description: "Visual CSS Grid layout tool",
      useFor: ["Quick grid layouts", "Learning grid syntax"],
      bookmark: "Handy for rapid prototyping"
    },
    {
      name: "Flexbox Froggy",
      url: "https://flexboxfroggy.com/",
      description: "Interactive Flexbox learning game",
      useFor: ["Learning flexbox interactively", "Quick flexbox reference"],
      bookmark: "Great for flexbox mastery"
    }
  ]
};
```

### JavaScript

```js
const javascriptResources = {
  // Core language documentation
  coreLanguage: [
    {
      name: "MDN JavaScript Reference",
      url: "https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference",
      description: "Complete JavaScript language reference",
      useFor: ["Method lookups", "Syntax reference", "Browser APIs"],
      bookmark: "Primary JavaScript reference",
      sections: {
        objects: "Built-in objects (Array, Object, String, etc.)",
        operators: "All JavaScript operators explained",
        statements: "Control flow and declarations",
        functions: "Function syntax and features"
      }
    },
    {
      name: "JavaScript.info",
      url: "https://javascript.info/",
      description: "Modern JavaScript tutorial and reference",
      useFor: ["Learning concepts", "Advanced topics", "Practical examples"],
      bookmark: "Best JavaScript learning resource",
      highlights: ["Closures explained", "Async/await tutorial", "Prototype chain"]
    },
    {
      name: "ECMAScript Specification",
      url: "https://tc39.es/ecma262/",
      description: "Official JavaScript language specification",
      useFor: ["Understanding language behavior", "Edge cases", "Proposal tracking"],
      bookmark: "When you need authoritative answers",
      note: "Advanced reference for deep understanding"
    }
  ],
  
  // Modern JavaScript features
  modernFeatures: [
    {
      name: "TC39 Proposals",
      url: "https://github.com/tc39/proposals",
      description: "Upcoming JavaScript features",
      useFor: ["Future language features", "Experimental syntax"],
      bookmark: "Stay ahead of JavaScript evolution"
    },
    {
      name: "Babel REPL",
      url: "https://babeljs.io/repl",
      description: "Try new JavaScript features online",
      useFor: ["Testing ES6+ syntax", "Understanding transpilation"],
      bookmark: "Quick experimentation tool"
    }
  ]
};
```

---

## ⚛️ React Ecosystem

### React Documentation

```js
const reactResources = {
  // Official React docs
  official: [
    {
      name: "React Documentation",
      url: "https://react.dev/",
      description: "Official React documentation (new 2023 version)",
      useFor: ["API reference", "Concepts", "Best practices"],
      bookmark: "Primary React reference",
      sections: {
        learn: "Interactive tutorials and concepts",
        reference: "Complete API documentation",
        community: "Resources and community links"
      }
    },
    {
      name: "React Legacy Docs",
      url: "https://legacy.reactjs.org/",
      description: "Previous version of React docs",
      useFor: ["Class component reference", "Legacy patterns"],
      bookmark: "When working with older React codebases",
      note: "Keep for class component projects"
    }
  ],
  
  // React ecosystem tools
  ecosystem: [
    {
      name: "Create React App",
      url: "https://create-react-app.dev/",
      description: "Official React project generator",
      useFor: ["Project setup", "Build configuration", "Deployment"],
      bookmark: "Quick project initialization"
    },
    {
      name: "React Router",
      url: "https://reactrouter.com/",
      description: "Declarative routing for React",
      useFor: ["Client-side routing", "Navigation", "Route protection"],
      bookmark: "Essential for SPA development"
    },
    {
      name: "React Testing Library",
      url: "https://testing-library.com/docs/react-testing-library/intro/",
      description: "Testing utilities for React components",
      useFor: ["Component testing", "User interaction testing", "Best practices"],
      bookmark: "Modern React testing approach"
    }
  ],
  
  // State management
  stateManagement: [
    {
      name: "Redux Toolkit",
      url: "https://redux-toolkit.js.org/",
      description: "Modern Redux development tools",
      useFor: ["Complex state management", "Time travel debugging", "Middleware"],
      bookmark: "When React Context isn't enough"
    },
    {
      name: "Zustand",
      url: "https://github.com/pmndrs/zustand",
      description: "Lightweight state management",
      useFor: ["Simple global state", "TypeScript support", "Minimal boilerplate"],
      bookmark: "Redux alternative for smaller apps"
    },
    {
      name: "React Query (TanStack Query)",
      url: "https://tanstack.com/query/latest",
      description: "Data fetching and caching library",
      useFor: ["Server state management", "Caching", "Background updates"],
      bookmark: "Essential for API-heavy applications"
    }
  ]
};
```

### Next.js

```js
const nextjsResources = {
  // Core Next.js documentation
  core: [
    {
      name: "Next.js Documentation",
      url: "https://nextjs.org/docs",
      description: "Official Next.js documentation",
      useFor: ["Framework features", "API routes", "Deployment"],
      bookmark: "Complete Next.js reference",
      keyPages: {
        gettingStarted: "Installation and basic concepts",
        appRouter: "New App Router documentation (Next.js 13+)",
        apiRoutes: "Backend API development",
        deployment: "Vercel and other deployment options"
      }
    },
    {
      name: "Next.js Examples",
      url: "https://github.com/vercel/next.js/tree/canary/examples",
      description: "Official Next.js example projects",
      useFor: ["Project templates", "Integration examples", "Best practices"],
      bookmark: "Quick project starters",
      popular: ["with-typescript", "with-tailwindcss", "api-routes"]
    }
  ],
  
  // Deployment and hosting
  deployment: [
    {
      name: "Vercel Documentation",
      url: "https://vercel.com/docs",
      description: "Vercel platform documentation",
      useFor: ["Deployment", "Environment variables", "Analytics"],
      bookmark: "Primary deployment platform for Next.js"
    },
    {
      name: "Netlify Docs",
      url: "https://docs.netlify.com/",
      description: "Netlify platform documentation", 
      useFor: ["Static site deployment", "Functions", "Forms"],
      bookmark: "Alternative deployment option"
    }
  ]
};
```

---

## 🛠️ Developer Tools

### Build Tools & Bundlers

```js
const buildTools = {
  // Modern build tools
  modern: [
    {
      name: "Vite Documentation",
      url: "https://vitejs.dev/",
      description: "Fast build tool for modern web development",
      useFor: ["Fast development server", "Build optimization", "Plugin ecosystem"],
      bookmark: "Modern alternative to Create React App",
      features: ["Hot Module Replacement", "TypeScript support", "CSS preprocessing"]
    },
    {
      name: "Webpack Documentation",
      url: "https://webpack.js.org/",
      description: "Module bundler for JavaScript applications",
      useFor: ["Complex build configurations", "Code splitting", "Asset optimization"],
      bookmark: "When you need advanced bundling control",
      note: "Complex but powerful"
    }
  ],
  
  // Package managers
  packageManagers: [
    {
      name: "npm Documentation",
      url: "https://docs.npmjs.com/",
      description: "Node.js package manager documentation",
      useFor: ["Package management", "Scripts", "Publishing"],
      bookmark: "Essential for Node.js development"
    },
    {
      name: "Yarn Documentation", 
      url: "https://yarnpkg.com/",
      description: "Fast, reliable package manager",
      useFor: ["Dependency management", "Workspaces", "Offline installs"],
      bookmark: "npm alternative with workspace support"
    },
    {
      name: "pnpm Documentation",
      url: "https://pnpm.io/",
      description: "Fast, disk space efficient package manager",
      useFor: ["Monorepos", "Strict dependency resolution", "Space saving"],
      bookmark: "Efficient package manager for large projects"
    }
  ]
};
```

### Version Control

```js
const versionControl = {
  // Git documentation
  git: [
    {
      name: "Git Documentation",
      url: "https://git-scm.com/doc",
      description: "Official Git documentation and reference",
      useFor: ["Command reference", "Workflow strategies", "Troubleshooting"],
      bookmark: "Complete Git reference",
      essentials: ["Basic commands", "Branching", "Merging", "Remote repositories"]
    },
    {
      name: "GitHub Docs",
      url: "https://docs.github.com/en",
      description: "GitHub platform documentation",
      useFor: ["Repository management", "Actions/CI-CD", "Collaboration"],
      bookmark: "GitHub feature reference",
      popular: ["GitHub Actions", "Pull requests", "Issues and projects"]
    },
    {
      name: "GitLab Docs",
      url: "https://docs.gitlab.com/",
      description: "GitLab platform documentation",
      useFor: ["DevOps workflows", "CI/CD pipelines", "Container registry"],
      bookmark: "Full DevOps platform reference"
    }
  ],
  
  // Git learning resources
  learning: [
    {
      name: "Learn Git Branching",
      url: "https://learngitbranching.js.org/",
      description: "Interactive Git tutorial",
      useFor: ["Visual Git learning", "Branching strategies", "Advanced Git"],
      bookmark: "Best interactive Git tutorial"
    },
    {
      name: "Oh Shit, Git!?!",
      url: "https://ohshitgit.com/",
      description: "Git troubleshooting guide",
      useFor: ["Common Git problems", "Recovery commands", "Quick fixes"],
      bookmark: "When Git goes wrong"
    }
  ]
};
```

---

## 🎨 CSS Frameworks & Design

### CSS Frameworks

```js
const cssFrameworks = {
  // Utility-first frameworks
  utilityFirst: [
    {
      name: "Tailwind CSS Documentation",
      url: "https://tailwindcss.com/docs",
      description: "Utility-first CSS framework",
      useFor: ["Rapid UI development", "Design systems", "Responsive design"],
      bookmark: "Modern CSS framework approach",
      tools: {
        playground: "https://play.tailwindcss.com/",
        ui: "https://tailwindui.com/",
        components: "https://headlessui.com/"
      }
    },
    {
      name: "Tailwind CSS Cheat Sheet",
      url: "https://tailwindcomponents.com/cheatsheet/",
      description: "Quick reference for Tailwind classes",
      useFor: ["Class lookups", "Quick reference", "Learning utility names"],
      bookmark: "Tailwind quick reference"
    }
  ],
  
  // Component frameworks
  component: [
    {
      name: "Bootstrap Documentation",
      url: "https://getbootstrap.com/docs/",
      description: "Popular CSS component framework",
      useFor: ["Component library", "Grid system", "JavaScript components"],
      bookmark: "When you need pre-built components"
    },
    {
      name: "Bulma Documentation",
      url: "https://bulma.io/documentation/",
      description: "Modern CSS framework based on Flexbox",
      useFor: ["Flexbox layouts", "CSS-only components", "Clean design"],
      bookmark: "CSS-only alternative to Bootstrap"
    }
  ]
};
```

### Design Resources

```js
const designResources = {
  // Design systems
  designSystems: [
    {
      name: "Material Design",
      url: "https://material.io/design",
      description: "Google's design system",
      useFor: ["Design principles", "Component patterns", "Accessibility"],
      bookmark: "Comprehensive design system reference"
    },
    {
      name: "Human Interface Guidelines",
      url: "https://developer.apple.com/design/human-interface-guidelines/",
      description: "Apple's design principles",
      useFor: ["iOS/macOS design", "User experience", "Interface patterns"],
      bookmark: "Apple design standards"
    },
    {
      name: "Ant Design",
      url: "https://ant.design/",
      description: "Enterprise design language and React components",
      useFor: ["React components", "Design patterns", "Enterprise apps"],
      bookmark: "Complete React UI library"
    }
  ],
  
  // Color and typography
  visualDesign: [
    {
      name: "Adobe Color",
      url: "https://color.adobe.com/",
      description: "Color palette generator and trends",
      useFor: ["Color scheme generation", "Accessibility checking", "Color theory"],
      bookmark: "Professional color tool"
    },
    {
      name: "Google Fonts",
      url: "https://fonts.google.com/",
      description: "Free web fonts",
      useFor: ["Typography selection", "Font pairing", "Web performance"],
      bookmark: "Primary font resource"
    },
    {
      name: "Type Scale",
      url: "https://type-scale.com/",
      description: "Typography scale generator",
      useFor: ["Font size hierarchies", "Typography systems", "Design consistency"],
      bookmark: "Typography planning tool"
    }
  ]
};
```

---

## 🗄️ Backend & Databases

### Node.js & Express

```js
const backendResources = {
  // Node.js ecosystem
  nodejs: [
    {
      name: "Node.js Documentation",
      url: "https://nodejs.org/en/docs/",
      description: "Official Node.js documentation",
      useFor: ["Core modules", "API reference", "Best practices"],
      bookmark: "Essential for backend development",
      keyModules: ["fs", "http", "path", "crypto", "stream"]
    },
    {
      name: "Express.js Documentation",
      url: "https://expressjs.com/",
      description: "Fast, minimalist web framework for Node.js",
      useFor: ["API development", "Middleware", "Routing"],
      bookmark: "Most popular Node.js framework",
      guides: ["Getting started", "Routing", "Middleware", "Error handling"]
    },
    {
      name: "Fastify Documentation",
      url: "https://www.fastify.io/docs/",
      description: "Fast and low overhead web framework",
      useFor: ["High-performance APIs", "Schema validation", "Plugin ecosystem"],
      bookmark: "Express alternative focused on speed"
    }
  ],
  
  // API development
  apiDevelopment: [
    {
      name: "REST API Tutorial",
      url: "https://restfulapi.net/",
      description: "Complete guide to RESTful API design",
      useFor: ["API design principles", "HTTP methods", "Status codes"],
      bookmark: "REST API best practices"
    },
    {
      name: "JSON:API Specification",
      url: "https://jsonapi.org/",
      description: "Specification for building APIs in JSON",
      useFor: ["API standardization", "Consistent response format", "Client-server agreement"],
      bookmark: "API design standard"
    }
  ]
};
