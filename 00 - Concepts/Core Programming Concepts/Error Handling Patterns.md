
Robust error handling prevents crashes, improves user experience, and makes debugging easier. Handle errors gracefully instead of letting them crash your application.

---

## 🎯 Core Principles

**Fail Fast**: Detect errors early and clearly  
**Fail Safe**: Graceful degradation when errors occur  
**Fail Loud**: Log errors for debugging  
**User-Friendly**: Show helpful messages to users

---

## 🔍 Types of Errors

### Syntax Errors

```js
// ❌ Caught at compile/parse time
function broken() {
  return "missing quote;
}
```

### Runtime Errors

```js
// ❌ Thrown during execution
function divide(a, b) {
  if (b === 0) {
    throw new Error('Division by zero');
  }
  return a / b;
}
```

### Logic Errors

```js
// ❌ Wrong results, no error thrown
function calculateAge(birthYear) {
  return birthYear - new Date().getFullYear(); // Should be reversed!
}
```

---

## 🛡️ JavaScript Error Handling

### Try-Catch Blocks

```js
function safeOperation() {
  try {
    const result = riskyFunction();
    return result;
  } catch (error) {
    console.error('Operation failed:', error.message);
    return null; // Fallback value
  } finally {
    // Always runs (cleanup)
    console.log('Operation completed');
  }
}
```

### Custom Error Types

```js
class ValidationError extends Error {
  constructor(field, value) {
    super(`Invalid ${field}: ${value}`);
    this.name = 'ValidationError';
    this.field = field;
    this.value = value;
  }
}

class NetworkError extends Error {
  constructor(status, url) {
    super(`Network request failed: ${status}`);
    this.name = 'NetworkError';
    this.status = status;
    this.url = url;
  }
}

// Usage
function validateEmail(email) {
  if (!email.includes('@')) {
    throw new ValidationError('email', email);
  }
}
```

---

## 🌐 Async Error Handling

### Async/Await with Try-Catch

```js
async function fetchUserData(id) {
  try {
    const response = await fetch(`/api/users/${id}`);
    
    if (!response.ok) {
      throw new NetworkError(response.status, response.url);
    }
    
    const user = await response.json();
    return user;
  } catch (error) {
    if (error instanceof NetworkError) {
      console.error('Network issue:', error.message);
      return null;
    }
    
    console.error('Unexpected error:', error);
    throw error; // Re-throw if we can't handle it
  }
}
```

### Promise Error Handling

```js
fetch('/api/data')
  .then(response => {
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('Fetch failed:', error));
```

### Unhandled Promise Rejections

```js
// Global handlers for unhandled errors
process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Promise Rejection:', reason);
  // Log to monitoring service
});

window.addEventListener('unhandledrejection', event => {
  console.error('Unhandled Promise Rejection:', event.reason);
  // Log to error tracking service
});
```

---

## 🔧 Defensive Programming

### Input Validation

```js
function calculateTotal(items) {
  // Validate inputs
  if (!Array.isArray(items)) {
    throw new ValidationError('items', 'Must be an array');
  }
  
  if (items.length === 0) {
    return 0; // Safe default
  }
  
  return items.reduce((total, item) => {
    // Validate each item
    if (typeof item.price !== 'number' || item.price < 0) {
      throw new ValidationError('price', item.price);
    }
    return total + item.price;
  }, 0);
}
```

### Null/Undefined Checks

```js
// ❌ Dangerous
function processUser(user) {
  return user.profile.name.toUpperCase();
}

// ✅ Safe with optional chaining
function processUser(user) {
  return user?.profile?.name?.toUpperCase() || 'Unknown';
}

// ✅ Safe with explicit checks
function processUser(user) {
  if (!user || !user.profile || !user.profile.name) {
    return 'Unknown';
  }
  return user.profile.name.toUpperCase();
}
```

---

## 🎨 Frontend Error Handling

### React Error Boundaries

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
    // Log to error tracking service
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div className="error-fallback">
          <h2>Something went wrong</h2>
          <button onClick={() => this.setState({ hasError: false })}>
            Try again
          </button>
        </div>
      );
    }
    
    return this.props.children;
  }
}
```

### Form Validation

```js
function validateForm(data) {
  const errors = {};
  
  if (!data.email) {
    errors.email = 'Email is required';
  } else if (!/\S+@\S+\.\S+/.test(data.email)) {
    errors.email = 'Email is invalid';
  }
  
  if (!data.password) {
    errors.password = 'Password is required';
  } else if (data.password.length < 8) {
    errors.password = 'Password must be at least 8 characters';
  }
  
  return {
    isValid: Object.keys(errors).length === 0,
    errors
  };
}
```

---

## 🖥️ Backend Error Handling

### Express.js Error Middleware

```js
// Custom error class
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = true;
  }
}

// Error handling middleware
function errorHandler(err, req, res, next) {
  let error = { ...err };
  error.message = err.message;
  
  // Log error
  console.error(err);
  
  // Mongoose bad ObjectId
  if (err.name === 'CastError') {
    const message = 'Resource not found';
    error = new AppError(message, 404);
  }
  
  // Mongoose duplicate key
  if (err.code === 11000) {
    const message = 'Duplicate field value entered';
    error = new AppError(message, 400);
  }
  
  // Mongoose validation error
  if (err.name === 'ValidationError') {
    const message = Object.values(err.errors).map(val => val.message);
    error = new AppError(message, 400);
  }
  
  res.status(error.statusCode || 500).json({
    success: false,
    error: error.message || 'Server Error'
  });
}

// Use the middleware
app.use(errorHandler);
```

### API Route Error Handling

```js
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

app.get('/api/users/:id', asyncHandler(async (req, res) => {
  const user = await User.findById(req.params.id);
  
  if (!user) {
    throw new AppError('User not found', 404);
  }
  
  res.json({ success: true, data: user });
}));
```

---

## 📊 Error Monitoring & Logging

### Structured Logging

```js
const logger = {
  error: (message, meta = {}) => {
    const logEntry = {
      level: 'error',
      message,
      timestamp: new Date().toISOString(),
      ...meta
    };
    console.error(JSON.stringify(logEntry));
  }
};

// Usage
try {
  await processPayment(paymentData);
} catch (error) {
  logger.error('Payment processing failed', {
    userId: user.id,
    amount: paymentData.amount,
    error: error.message,
    stack: error.stack
  });
}
```

### Error Tracking Services

```js
// Sentry example
import * as Sentry from '@sentry/browser';

Sentry.init({
  dsn: 'YOUR_DSN_HERE'
});

try {
  riskyOperation();
} catch (error) {
  Sentry.captureException(error, {
    user: { id: currentUser.id },
    extra: { context: 'checkout process' }
  });
}
```

---

## 🔄 Retry Patterns

### Exponential Backoff

```js
async function retryWithBackoff(fn, maxRetries = 3, baseDelay = 1000) {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error) {
      if (attempt === maxRetries) {
        throw error;
      }
      
      const delay = baseDelay * Math.pow(2, attempt - 1);
      console.log(`Attempt ${attempt} failed, retrying in ${delay}ms`);
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
}

// Usage
const data = await retryWithBackoff(() => fetch('/api/data'));
```

### Circuit Breaker Pattern

```js
class CircuitBreaker {
  constructor(threshold = 5, timeout = 60000) {
    this.threshold = threshold;
    this.timeout = timeout;
    this.failureCount = 0;
    this.lastFail
```