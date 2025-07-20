
Security isn't optional—it's essential for protecting user data, preventing attacks, and maintaining trust. Every developer needs to understand core security principles.

---

## 🛡️ Core Security Principles

**Defense in Depth**: Multiple layers of security  
**Least Privilege**: Give minimum necessary access  
**Fail Secure**: Fail to a secure state, not open  
**Never Trust User Input**: Always validate and sanitize  
**Keep It Simple**: Complex systems have more vulnerabilities

---

## 🎯 Common Web Vulnerabilities (OWASP Top 10)

### 1. Injection Attacks

#### SQL Injection

```js
// ❌ Vulnerable to SQL injection
app.get('/users', (req, res) => {
  const query = `SELECT * FROM users WHERE name = '${req.query.name}'`;
  db.query(query); // Attacker could input: '; DROP TABLE users; --
});

// ✅ Safe with parameterized queries
app.get('/users', (req, res) => {
  const query = 'SELECT * FROM users WHERE name = ?';
  db.query(query, [req.query.name]);
});
```

#### NoSQL Injection

```js
// ❌ Vulnerable
const user = await User.findOne({
  email: req.body.email, // Could be: { $gt: "" }
  password: req.body.password
});

// ✅ Safe with validation
const { email, password } = req.body;
if (typeof email !== 'string' || typeof password !== 'string') {
  return res.status(400).json({ error: 'Invalid input' });
}

const user = await User.findOne({ email, password });
```

### 2. Cross-Site Scripting (XSS)

#### Stored XSS

```js
// ❌ Dangerous - executes user scripts
function displayComment(comment) {
  document.getElementById('comments').innerHTML += `
    <div>${comment.text}</div>
  `; // Attacker input: <script>steal_cookies()</script>
}

// ✅ Safe - escape HTML
function displayComment(comment) {
  const div = document.createElement('div');
  div.textContent = comment.text; // Automatically escapes
  document.getElementById('comments').appendChild(div);
}
```

#### React XSS Protection

```jsx
// ✅ React automatically escapes by default
function Comment({ text }) {
  return <div>{text}</div>; // Safe - text is escaped
}

// ❌ Dangerous - bypasses React's protection
function Comment({ html }) {
  return <div dangerouslySetInnerHTML={{ __html: html }} />; // Only if you trust the HTML!
}

// ✅ Safe HTML rendering with sanitization
import DOMPurify from 'dompurify';

function Comment({ html }) {
  const sanitized = DOMPurify.sanitize(html);
  return <div dangerouslySetInnerHTML={{ __html: sanitized }} />;
}
```

### 3. Cross-Site Request Forgery (CSRF)

```js
// ✅ CSRF protection with tokens
const csrf = require('csurf');
const csrfProtection = csrf({ cookie: true });

app.use(csrfProtection);

app.get('/form', (req, res) => {
  res.render('form', { csrfToken: req.csrfToken() });
});

app.post('/transfer-money', (req, res) => {
  // CSRF token automatically validated
  transferMoney(req.body.amount, req.body.to);
});
```

### 4. Insecure Direct Object References

```js
// ❌ Vulnerable - no authorization check
app.get('/documents/:id', (req, res) => {
  const doc = Document.findById(req.params.id);
  res.json(doc); // Any user can access any document!
});

// ✅ Safe - verify ownership
app.get('/documents/:id', authenticateUser, (req, res) => {
  const doc = Document.findOne({
    _id: req.params.id,
    userId: req.user.id // Only return user's own documents
  });
  
  if (!doc) {
    return res.status(404).json({ error: 'Document not found' });
  }
  
  res.json(doc);
});
```

---

## 🔐 Authentication & Authorization

### Password Security

#### Hashing Passwords

```js
const bcrypt = require('bcryptjs');

// ✅ Hash passwords before storing
async function createUser(email, password) {
  const saltRounds = 12;
  const hashedPassword = await bcrypt.hash(password, saltRounds);
  
  return User.create({
    email,
    password: hashedPassword // Never store plain text!
  });
}

// ✅ Verify passwords
async function verifyPassword(email, password) {
  const user = await User.findOne({ email });
  if (!user) return false;
  
  return bcrypt.compare(password, user.password);
}
```

#### Password Requirements

```js
function validatePassword(password) {
  const requirements = {
    minLength: password.length >= 12,
    hasUppercase: /[A-Z]/.test(password),
    hasLowercase: /[a-z]/.test(password),
    hasNumbers: /\d/.test(password),
    hasSpecialChars: /[!@#$%^&*(),.?":{}|<>]/.test(password),
    notCommon: !COMMON_PASSWORDS.includes(password.toLowerCase())
  };
  
  const isValid = Object.values(requirements).every(req => req);
  
  return {
    isValid,
    requirements,
    score: Object.values(requirements).filter(Boolean).length
  };
}
```

### JWT Security

```js
// ✅ Secure JWT implementation
const jwt = require('jsonwebtoken');

function generateToken(user) {
  const payload = {
    id: user.id,
    email: user.email
    // Don't include sensitive data like passwords!
  };
  
  return jwt.sign(payload, process.env.JWT_SECRET, {
    expiresIn: '15m', // Short expiration
    issuer: 'your-app',
    audience: 'your-users'
  });
}

function verifyToken(token) {
  try {
    return jwt.verify(token, process.env.JWT_SECRET, {
      issuer: 'your-app',
      audience: 'your-users'
    });
  } catch (error) {
    return null;
  }
}

// ✅ Secure cookie settings
app.use(session({
  secret: process.env.SESSION_SECRET,
  cookie: {
    httpOnly: true,    // Prevent XSS access
    secure: true,      // HTTPS only
    sameSite: 'strict', // CSRF protection
    maxAge: 900000     // 15 minutes
  }
}));
```

---

## 🌐 Client-Side Security

### Content Security Policy (CSP)

```js
// ✅ Implement CSP headers
app.use((req, res, next) => {
  res.setHeader('Content-Security-Policy', 
    "default-src 'self'; " +
    "script-src 'self' 'unsafe-inline' https://cdnjs.cloudflare.com; " +
    "style-src 'self' 'unsafe-inline'; " +
    "img-src 'self' data: https:; " +
    "font-src 'self' https://fonts.gstatic.com; " +
    "connect-src 'self' https://api.yourapp.com"
  );
  next();
});
```

### Secure Headers

```js
const helmet = require('helmet');

app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"]
    }
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true
  }
}));
```

### Input Validation & Sanitization

```js
const validator = require('validator');
const DOMPurify = require('dompurify');

function validateAndSanitizeInput(req, res, next) {
  const { email, name, bio } = req.body;
  
  // Validate email
  if (!validator.isEmail(email)) {
    return res.status(400).json({ error: 'Invalid email format' });
  }
  
  // Sanitize name (remove HTML)
  req.body.name = validator.escape(name);
  
  // Sanitize bio (allow some HTML but remove dangerous parts)
  req.body.bio = DOMPurify.sanitize(bio, {
    ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'p'],
    ALLOWED_ATTR: []
  });
  
  next();
}
```

---

## 🔒 Data Protection

### Encryption at Rest

```js
const crypto = require('crypto');

class DataEncryption {
  constructor() {
    this.algorithm = 'aes-256-gcm';
    this.secretKey = crypto.scryptSync(process.env.ENCRYPTION_PASSWORD, 'salt', 32);
  }
  
  encrypt(text) {
    const iv = crypto.randomBytes(16);
    const cipher = crypto.createCipher(this.algorithm, this.secretKey, iv);
    
    let encrypted = cipher.update(text, 'utf8', 'hex');
    encrypted += cipher.final('hex');
    
    const authTag = cipher.getAuthTag();
    
    return {
      encrypted,
      iv: iv.toString('hex'),
      authTag: authTag.toString('hex')
    };
  }
  
  decrypt(encryptedData) {
    const decipher = crypto.createDecipher(
      this.algorithm, 
      this.secretKey, 
      Buffer.from(encryptedData.iv, 'hex')
    );
    
    decipher.setAuthTag(Buffer.from(encryptedData.authTag, 'hex'));
    
    let decrypted = decipher.update(encryptedData.encrypted, 'hex', 'utf8');
    decrypted += decipher.final('utf8');
    
    return decrypted;
  }
}

// Usage
const encryption = new DataEncryption();
const sensitiveData = "User's credit card number";
const encrypted = encryption.encrypt(sensitiveData);
// Store encrypted data in database
```

### Environment Variables Security

```js
// ✅ Secure environment setup
// .env file (never commit this!)
/*
JWT_SECRET=your-super-long-random-string-here-minimum-32-characters
DB_PASSWORD=complex-database-password
ENCRYPTION_KEY=another-long-random-string-for-encryption
API_KEY=third-party-service-api-key
*/

// ✅ Validate required environment variables
function validateEnvironment() {
  const required = [
    'JWT_SECRET',
    'DB_PASSWORD', 
    'ENCRYPTION_KEY'
  ];
  
  const missing = required.filter(key => !process.env[key]);
  
  if (missing.length > 0) {
    throw new Error(`Missing required environment variables: ${missing.join(', ')}`);
  }
  
  // Validate minimum security requirements
  if (process.env.JWT_SECRET.length < 32) {
    throw new Error('JWT_SECRET must be at least 32 characters');
  }
}

// Call on app startup
validateEnvironment();
```

---

## 🚪 API Security

### Rate Limiting

```js
const rateLimit = require('express-rate-limit');

// ✅ Basic rate limiting
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP',
  standardHeaders: true,
  legacyHeaders: false
});

// ✅ Stricter rate limiting for auth endpoints
const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5, // Only 5 login attempts per 15 minutes
  skipSuccessfulRequests: true
});

app.use('/api/', limiter);
app.use('/auth/login', authLimiter);
```

### API Authentication

```js
// ✅ API key authentication
function validateApiKey(req, res, next) {
  const apiKey = req.headers['x-api-key'];
  
  if (!apiKey) {
    return res.status(401).json({ error: 'API key required' });
  }
  
  // Hash the provided key and compare with stored hash
  const hashedKey = crypto.createHash('sha256').update(apiKey).digest('hex');
  
  if (!VALID_API_KEYS.includes(hashedKey)) {
    return res.status(401).json({ error: 'Invalid API key' });
  }
  
  next();
}

// ✅ Bearer token authentication
function validateBearerToken(req, res, next) {
  const authHeader = req.headers.authorization;
  
  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return res.status(401).json({ error: 'Bearer token required' });
  }
  
  const token = authHeader.split(' ')[1];
  const decoded = verifyToken(token);
  
  if (!decoded) {
    return res.status(401).json({ error: 'Invalid token' });
  }
  
  req.user = decoded;
  next();
}
```

### CORS Security

```js
const cors = require('cors');

// ✅ Secure CORS configuration
const corsOptions = {
  origin: function (origin, callback) {
    const allowedOrigins = [
      'https://yourapp.com',
      'https://www.yourapp.com',
      'https://app.yourapp.com'
    ];
    
    if (!origin || allowedOrigins.includes(origin)) {
      callback(null, true);
    } else {
      callback(new Error('Not allowed by CORS'));
    }
  },
  credentials: true, // Allow cookies
  optionsSuccessStatus: 200
};

app.use(cors(corsOptions));
```

---

## 🕵️ Security Monitoring & Logging

### Security Event Logging

```js
const winston = require('winston');

const securityLogger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: 'security.log' }),
    new winston.transports.Console()
  ]
});

function logSecurityEvent(event, req, additional = {}) {
  securityLogger.warn({
    event,
    ip: req.ip,
    userAgent: req.get('User-Agent'),
    url: req.originalUrl,
    timestamp: new Date().toISOString(),
    ...additional
  });
}

// Usage in middleware
function detectSuspiciousActivity(req, res, next) {
  // Log failed login attempts
  if (req.path === '/auth/login' && req.method === 'POST') {
    req.on('response', () => {
      if (res.statusCode === 401) {
        logSecurityEvent('FAILED_LOGIN', req, {
          email: req.body.email
        });
      }
    });
  }
  
  // Log admin access
  if (req.path.startsWith('/admin')) {
    logSecurityEvent('ADMIN_ACCESS', req, {
      userId: req.user?.id
    });
  }
  
  next();
}
```

### Intrusion Detection

```js
class SecurityMonitor {
  constructor() {
    this.suspiciousIPs = new Map();
    this.rateLimits = new Map();
  }
  
  checkRequest(req) {
    const ip = req.ip;
    const now = Date.now();
    
    // Track request patterns
    if (!this.rateLimits.has(ip)) {
      this.rateLimits.set(ip, []);
    }
    
    const requests = this.rateLimits.get(ip);
    requests.push(now);
    
    // Remove old requests (older than 1 minute)
    const recentRequests = requests.filter(time => now - time < 60000);
    this.rateLimits.set(ip, recentRequests);
    
    // Detect suspicious patterns
    if (recentRequests.length > 100) {
      this.markSuspicious(ip, 'HIGH_REQUEST_RATE');
      return false;
    }
    
    // Check for SQL injection patterns
    const body = JSON.stringify(req.body);
    const suspicious = [
      /(\bUNION\b|\bSELECT\b|\bINSERT\b|\bDELETE\b|\bDROP\b)/i,
      /<script[^>]*>.*?<\/script>/gi,
      /javascript:/gi
    ];
    
    if (suspicious.some(pattern => pattern.test(body))) {
      this.markSuspicious(ip, 'INJECTION_ATTEMPT');
      return false;
    }
    
    return true;
  }
  
  markSuspicious(ip, reason) {
    logSecurityEvent('SUSPICIOUS_ACTIVITY', { ip }, { reason });
    this.suspiciousIPs.set(ip, {
      reason,
      timestamp: Date.now(),
      blocked: true
    });
  }
}

const monitor = new SecurityMonitor();

app.use((req, res, next) => {
  if (!monitor.checkRequest(req)) {
    return res.status(429).json({ error: 'Request blocked' });
  }
  next();
});
```

---

## 🔍 Security Testing

### Automated Security Scanning

```js
// package.json
{
  "scripts": {
    "security-audit": "npm audit",
    "security-check": "npx audit-ci --moderate",
    "dependency-check": "npx better-npm-audit audit"
  }
}
```

### Security Test Cases

```js
// Security-focused tests
describe('Authentication Security', () => {
  test('should reject weak passwords', async () => {
    const weakPasswords = ['123456', 'password', 'qwerty'];
    
    for (const password of weakPasswords) {
      const response = await request(app)
        .post('/auth/register')
        .send({
          email: 'test@example.com',
          password: password
        });
      
      expect(response.status).toBe(400);
      expect(response.body.error).toContain('password');
    }
  });
  
  test('should prevent brute force attacks', async () => {
    const attempts = [];
    
    // Make multiple failed login attempts
    for (let i = 0; i < 10; i++) {
      attempts.push(
        request(app)
          .post('/auth/login')
          .send({
            email: 'test@example.com',
            password: 'wrongpassword'
          })
      );
    }
    
    const responses = await Promise.all(attempts);
    const lastResponse = responses[responses.length - 1];
    
    expect(lastResponse.status).toBe(429); // Rate limited
  });
  
  test('should sanitize user input', async () => {
    const maliciousInput = '<script>alert("xss")</script>';
    
    const response = await request(app)
      .post('/api/comments')
      .send({ text: maliciousInput })
      .set('Authorization', `Bearer ${validToken}`);
    
    expect(response.body.text).not.toContain('<script>');
  });
});
```

---

## 🚨 Incident Response

### Security Incident Handling

```js
class SecurityIncidentHandler {
  constructor() {
    this.incidents = [];
    this.alertThresholds = {
      FAILED_LOGINS: 10,
      INJECTION_ATTEMPTS: 5,
      SUSPICIOUS_IPS: 3
    };
  }
  
  reportIncident(type, details) {
    const incident = {
      id: crypto.randomUUID(),
      type,
      details,
      timestamp: new Date().toISOString(),
      severity: this.calculateSeverity(type),
      status: 'OPEN'
    };
    
    this.incidents.push(incident);
    
    // Send alerts for high severity incidents
    if (incident.severity === 'HIGH') {
      this.sendAlert(incident);
    }
    
    // Auto-block if threshold exceeded
    if (this.shouldAutoBlock(type)) {
      this.autoBlock(details.ip);
    }
    
    return incident;
  }
  
  calculateSeverity(type) {
    const severityMap = {
      'INJECTION_ATTEMPT': 'HIGH',
      'FAILED_LOGIN': 'MEDIUM',
      'SUSPICIOUS_ACTIVITY': 'MEDIUM',
      'RATE_LIMIT_EXCEEDED': 'LOW'
    };
    
    return severityMap[type] || 'LOW';
  }
  
  sendAlert(incident) {
    // Send to monitoring service, Slack, email, etc.
    console.error('SECURITY ALERT:', incident);
    
    // In production, integrate with your alerting system
    // slack.sendMessage('#security', `Security incident: ${incident.type}`);
    // email.send(securityTeam, 'Security Alert', incident);
  }
  
  autoBlock(ip) {
    // Add to firewall, update load balancer, etc.
    console.log(`Auto-blocking IP: ${ip}`);
  }
}

const incidentHandler = new SecurityIncidentHandler();
```

---

## 📋 Security Checklist

### Development

- [ ] Validate and sanitize all user inputs
- [ ] Use parameterized queries to prevent SQL injection
- [ ] Implement proper authentication and authorization
- [ ] Hash passwords with bcrypt (salt rounds ≥ 12)
- [ ] Use HTTPS everywhere
- [ ] Set secure HTTP headers (CSP, HSTS, etc.)
- [ ] Implement rate limiting on APIs
- [ ] Never expose sensitive data in client-side code

### Production

- [ ] Regular security audits and penetration testing
- [ ] Monitor for suspicious activities
- [ ] Keep dependencies up to date
- [ ] Use environment variables for secrets
- [ ] Implement proper logging and alerting
- [ ] Regular backups with encryption
- [ ] Incident response plan documented
- [ ] Security training for development team

### Code Review

- [ ] Check for hardcoded secrets or passwords
- [ ] Verify input validation on all endpoints
- [ ] Ensure proper error handling (no info leakage)
- [ ] Review authentication and authorization logic
- [ ] Check for potential XSS vulnerabilities
- [ ] Verify CSRF protection is implemented

---

## 🛠️ Security Tools & Libraries

### Node.js Security

- **helmet**: Secure HTTP headers
- **bcryptjs**: Password hashing
- **express-rate-limit**: Rate limiting
- **validator**: Input validation
- **dompurify**: HTML sanitization
- **csurf**: CSRF protection

### Frontend Security

- **DOMPurify**: XSS prevention
- **Content Security Policy**: XSS/injection prevention
- **SameSite cookies**: CSRF protection
- **Subresource Integrity**: CDN security

### Security Scanning

- **npm audit**: Dependency vulnerabilities
- **snyk**: Advanced vulnerability scanning
- **eslint-plugin-security**: Static analysis
- **OWASP ZAP**: Dynamic security testing

---

## 📚 Resources for Further Learning

- **OWASP Top 10**: Most critical web application risks
- **Mozilla Web Security**: Comprehensive security guide
- **Security Headers**: Check your site's security headers
- **Have I Been Pwned**: Check for data breaches
- **NIST Cybersecurity Framework**: Industry standards

---

## ✅ Learning Goals

- [ ] Understand common web vulnerabilities and how to prevent them
- [ ] Implement secure authentication and authorization
- [ ] Set up proper input validation and output encoding
- [ ] Configure secure HTTP headers and CORS policies
- [ ] Monitor and log security events effectively
- [ ] Know how to respond to security incidents
- [ ] Stay updated on emerging security threats and best practices