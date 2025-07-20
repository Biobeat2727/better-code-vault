
Effective code reviews improve code quality, share knowledge, and prevent bugs from reaching production. Use this systematic approach for thorough and constructive reviews.

---

## 🎯 Code Review Goals

**Find Bugs**: Catch logic errors, edge cases, and potential issues  
**Improve Quality**: Ensure readability, maintainability, and performance  
**Share Knowledge**: Learn from each other and spread best practices  
**Maintain Standards**: Enforce team coding conventions and architecture  
**Build Trust**: Create collaborative environment for improvement

---

## 🔍 Review Process & Mindset

### Before You Start

```js
const reviewPreparation = {
  // 1. Understand the context
  context: [
    "Read the PR description and requirements",
    "Understand what problem this solves", 
    "Check linked issues or tickets",
    "Note any architectural implications"
  ],
  
  // 2. Set the right mindset
  mindset: [
    "Focus on the code, not the person",
    "Assume positive intent",
    "Ask questions instead of making demands",
    "Suggest improvements, don't just criticize"
  ],
  
  // 3. Allocate sufficient time
  timeNeeded: "15-30 minutes for thorough review"
};
```

### Review Flow

```markdown
1. **High-level review** (5 minutes)
   - Overall approach and architecture
   - Does this solve the stated problem?
   - Are there any major design issues?

2. **Detailed code review** (15-20 minutes)  
   - Line-by-line examination
   - Logic, edge cases, error handling
   - Code quality and best practices

3. **Testing and documentation** (5 minutes)
   - Are tests adequate and meaningful?
   - Is documentation clear and complete?
   - Can you understand the code without explanation?
```

---

## 🏗️ Architecture & Design

### Overall Approach

```js
// ✅ Good: Clear separation of concerns
class UserService {
  constructor(userRepository, emailService) {
    this.userRepository = userRepository;
    this.emailService = emailService;
  }
  
  async createUser(userData) {
    const user = await this.userRepository.create(userData);
    await this.emailService.sendWelcomeEmail(user.email);
    return user;
  }
}

// ❌ Red flag: Mixing concerns
class UserService {
  async createUser(userData) {
    // Database logic mixed with email logic
    const user = await db.query('INSERT INTO users...');
    await sendEmail(user.email, 'Welcome!');
    return user;
  }
}
```

### Questions to Ask

- Does this follow established patterns in the codebase?
- Is the abstraction level appropriate?
- Are dependencies injected rather than hardcoded?
- Does this change break existing functionality?
- Is the component/module doing too many things?

---

## 🐛 Logic & Correctness

### Edge Cases & Error Handling

```js
// ✅ Good: Handles edge cases
function calculateDiscount(price, discountPercent) {
  // Input validation
  if (typeof price !== 'number' || price < 0) {
    throw new Error('Price must be a positive number');
  }
  
  if (typeof discountPercent !== 'number' || discountPercent < 0 || discountPercent > 100) {
    throw new Error('Discount must be between 0 and 100');
  }
  
  // Handle edge case
  if (price === 0) return 0;
  
  return price * (1 - discountPercent / 100);
}

// ❌ Red flags to catch
function calculateDiscount(price, discountPercent) {
  return price * (1 - discountPercent / 100); // No validation, potential NaN
}
```

### Common Logic Issues to Check

- **Null/undefined handling**: Are all possible null cases covered?
- **Array bounds**: Are array accesses safe from index errors?
- **Division by zero**: Are mathematical operations protected?
- **Async operations**: Are promises properly awaited and errors caught?
- **Race conditions**: Can concurrent operations cause issues?

### Security Considerations

```js
// ❌ Security red flags
function getUser(req, res) {
  const query = `SELECT * FROM users WHERE id = ${req.params.id}`; // SQL injection
  const user = await db.query(query);
  res.json(user); // Might expose sensitive data
}

// ✅ Secure approach
function getUser(req, res) {
  const userId = parseInt(req.params.id);
  if (!userId || userId !== req.user.id) { // Authorization check
    return res.status(403).json({ error: 'Unauthorized' });
  }
  
  const user = await db.query('SELECT id, name, email FROM users WHERE id = ?', [userId]);
  res.json(user); // Only expose safe fields
}
```

---

## 📖 Code Quality & Readability

### Naming & Clarity

```js
// ❌ Poor naming
function calc(x, y, z) {
  const result = x * y * z;
  return result;
}

// ✅ Clear naming
function calculateVolume(length, width, height) {
  const volume = length * width * height;
  return volume;
}

// ✅ Even better with documentation
/**
 * Calculates the volume of a rectangular box
 * @param {number} length - Length in meters
 * @param {number} width - Width in meters  
 * @param {number} height - Height in meters
 * @returns {number} Volume in cubic meters
 */
function calculateBoxVolume(length, width, height) {
  return length * width * height;
}
```

### Function & Class Design

```js
const qualityChecklist = {
  functions: [
    "Does one thing well (single responsibility)",
    "Has a clear, descriptive name",
    "Takes minimal parameters (ideally 3 or fewer)",
    "Returns predictable outputs",
    "Has no side effects (when possible)"
  ],
  
  classes: [
    "Has a clear purpose and responsibility",
    "Methods are cohesive and related",
    "Dependencies are injected, not created",
    "Follows consistent naming conventions"
  ]
};
```

### Code Smells to Flag

```js
const codeSmells = {
  // Size issues
  tooLong: "Functions over 50 lines, classes over 300 lines",
  tooManyParams: "Functions with more than 4-5 parameters",
  
  // Complexity issues  
  deepNesting: "More than 3-4 levels of indentation",
  longParameterList: "Functions that take many arguments",
  
  // Duplication issues
  duplicatedCode: "Same logic repeated in multiple places",
  magicNumbers: "Hardcoded numbers without explanation",
  
  // Naming issues
  unclearNames: "Variables like 'data', 'temp', 'x'",
  abbreviations: "Shortened names that aren't clear"
};
```

---

## 🧪 Testing & Documentation

### Test Quality Assessment

```js
// ✅ Good test structure
describe('calculateShipping', () => {
  it('should calculate basic shipping cost correctly', () => {
    const result = calculateShipping(10, 100, false);
    expect(result).toBe(15); // 10 * 0.5 + 100 * 0.01
  });
  
  it('should apply priority multiplier', () => {
    const result = calculateShipping(10, 100, true);
    expect(result).toBe(30); // (10 * 0.5 + 100 * 0.01) * 2
  });
  
  it('should handle edge case of zero weight', () => {
    const result = calculateShipping(0, 100, false);
    expect(result).toBe(1); // 0 * 0.5 + 100 * 0.01
  });
  
  it('should throw error for negative values', () => {
    expect(() => calculateShipping(-5, 100, false)).toThrow();
  });
});
```

### Testing Checklist

- [ ] **Coverage**: Are the main code paths tested?
- [ ] **Edge cases**: Are boundary conditions and error cases covered?
- [ ] **Test names**: Do test descriptions clearly explain what's being tested?
- [ ] **Assertions**: Are the assertions meaningful and specific?
- [ ] **Setup/teardown**: Is test data properly isolated?

### Documentation Review

```js
// ✅ Good documentation
/**
 * Processes payment using the specified payment method
 * 
 * @param {Object} paymentData - Payment information
 * @param {string} paymentData.amount - Amount in cents (e.g., 1500 for $15.00)
 * @param {string} paymentData.currency - ISO currency code (e.g., 'USD')
 * @param {string} paymentData.paymentMethodId - Stripe payment method ID
 * 
 * @returns {Promise<Object>} Payment result with success status and transaction ID
 * @throws {PaymentError} When payment processing fails
 * 
 * @example
 * const result = await processPayment({
 *   amount: 1500,
 *   currency: 'USD', 
 *   paymentMethodId: 'pm_card_visa'
 * });
 */
async function processPayment(paymentData) {
  // Implementation...
}
```

---

## ⚡ Performance Considerations

### Performance Red Flags

```js
// ❌ Performance issues to catch
function findUsers(users, criteria) {
  return users.filter(user => {
    // Expensive operation inside loop
    const score = calculateComplexScore(user);
    return score > criteria.minScore;
  });
}

// ✅ Performance improvement
function findUsers(users, criteria) {
  // Pre-calculate expensive values
  const usersWithScores = users.map(user => ({
    ...user,
    score: calculateComplexScore(user)
  }));
  
  return usersWithScores.filter(user => user.score > criteria.minScore);
}
```

### Performance Checklist

- [ ] **Database queries**: Are N+1 queries avoided?
- [ ] **Loops**: Are expensive operations moved outside loops?
- [ ] **Memory usage**: Are large objects/arrays handled efficiently?
- [ ] **Caching**: Could repeated calculations be cached?
- [ ] **Async operations**: Are operations properly parallelized when possible?

---

## 🔧 Technology-Specific Checks

### React/Frontend Specific

```jsx
// ❌ React anti-patterns
function UserList({ users }) {
  return (
    <div>
      {users.map(user => (
        // Missing key prop
        <div onClick={() => selectUser(user.id)}> 
          {user.name}
        </div>
      ))}
    </div>
  );
}

// ✅ React best practices
function UserList({ users, onUserSelect }) {
  return (
    <div>
      {users.map(user => (
        <UserItem 
          key={user.id}
          user={user}
          onSelect={onUserSelect}
        />
      ))}
    </div>
  );
}
```

### Backend/API Specific

```js
// ❌ API anti-patterns
app.post('/users', (req, res) => {
  const user = new User(req.body); // No validation
  user.save(); // No error handling
  res.json(user); // Might expose sensitive data
});

// ✅ API best practices
app.post('/users', validateInput, async (req, res) => {
  try {
    const userData = sanitizeInput(req.body);
    const user = await userService.createUser(userData);
    res.status(201).json(sanitizeOutput(user));
  } catch (error) {
    logger.error('User creation failed', error);
    res.status(400).json({ error: 'Failed to create user' });
  }
});
```

---

## 💬 Providing Feedback

### Constructive Comment Templates

```markdown
## For Bugs/Issues
🐛 **Potential Issue**: This could throw an error if `data` is null.
**Suggestion**: Add a null check before accessing `data.items`.

## For Improvements  
💡 **Improvement**: This function is doing two things - parsing and validation.
**Suggestion**: Consider splitting into `parseUserData()` and `validateUserData()`.

## For Alternatives
🔄 **Alternative Approach**: Instead of multiple if statements, you could use a switch or lookup table.
**Example**: [code example]

## For Learning
📚 **Learning Opportunity**: This pattern is called the Strategy pattern.
**Resource**: [link to explanation]

## For Praise
✅ **Nice Work**: I like how you handled the error cases here - very thorough!
```

### Comment Guidelines

```js
const feedbackGuidelines = {
  // Be specific and actionable
  specific: "Point to exact lines and suggest concrete improvements",
  
  // Explain the why
  reasoning: "Help them understand why the change matters",
  
  // Offer to help
  collaborative: "I'm happy to pair on this if you'd like",
  
  // Balance criticism with praise
  positive: "Acknowledge good decisions and clean code",
  
  // Ask questions instead of making demands
  curious: "What do you think about trying X approach here?"
};
```

---

## ✅ Review Severity Levels

### Critical (Must Fix Before Merge)

- Security vulnerabilities
- Logic errors that break functionality
- Performance issues that affect users
- Code that doesn't compile or pass tests

### Important (Should Fix)

- Code quality issues affecting maintainability
- Missing error handling for likely scenarios
- Violations of team coding standards
- Missing or inadequate tests

### Minor (Nice to Have)

- Naming improvements
- Code organization suggestions
- Performance micro-optimizations
- Documentation enhancements

### Nitpick (Optional)

- Personal style preferences
- Alternative implementations
- Educational comments
- Future improvement ideas

---

## 🔄 Follow-up Process

### After Providing Feedback

```js
const followUpProcess = {
  // 1. Be available for questions
  availability: "Respond to questions within reasonable time",
  
  // 2. Re-review promptly
  reReview: "Check updates within 24 hours",
  
  // 3. Acknowledge improvements
  recognition: "Thank the author for addressing feedback",
  
  // 4. Approve when ready
  approval: "Don't hold up good code for minor issues"
};
```

### When Receiving Feedback

```js
const receivingFeedback = {
  // Stay open minded
  mindset: "Feedback is about improving code, not personal criticism",
  
  // Ask for clarification
  clarify: "If you don't understand, ask questions",
  
  // Discuss disagreements respectfully
  discuss: "Explain your reasoning if you disagree",
  
  // Thank reviewers
  gratitude: "Acknowledge time spent reviewing your code"
};
```

---

## 📊 Code Review Metrics

### Personal Review Quality

```js
const reviewMetrics = {
  // Effectiveness metrics
  bugsFound: "How many bugs do you catch in review?",
  timeToReview: "How quickly do you provide feedback?",
  reviewThoroughness: "Do you check all areas systematically?",
  
  // Collaboration metrics
  feedbackQuality: "Is your feedback actionable and helpful?",
  responseTime: "How quickly do you respond to questions?",
  conflictResolution: "How well do you handle disagreements?"
};
```

### Team Review Health

```markdown
## Weekly Review Metrics
- Average time from PR to first review: ___
- Average time from PR to merge: ___
- Number of bugs caught in review: ___
- Number of bugs found in production: ___

## Monthly Review Retrospective  
- What types of issues are we missing?
- Are reviews taking too long?
- Is feedback being received well?
- What can we improve in our process?
```

---

## 🛠️ Tools & Automation

### Automated Checks (Pre-Review)

```json
{
  "scripts": {
    "lint": "eslint src/",
    "test": "jest",
    "type-check": "tsc --noEmit",
    "security": "npm audit",
    "pre-commit": "lint-staged"
  },
  
  "lint-staged": {
    "*.js": ["eslint --fix", "jest --bail --findRelatedTests"],
    "*.{js,css,md}": "prettier --write"
  }
}
```

### Review Tools

- **GitHub/GitLab**: Built-in review features
- **SonarQube**: Code quality analysis
- **CodeClimate**: Automated code review
- **DeepCode**: AI-powered issue detection

---

## 📋 Quick Reference Checklist

### Before Reviewing

- [ ] Read PR description and understand context
- [ ] Check that CI/CD passes
- [ ] Allocate sufficient time for thorough review

### During Review

- [ ] Check overall architecture and approach
- [ ] Review logic for correctness and edge cases
- [ ] Assess code quality and readability
- [ ] Verify tests are adequate and meaningful
- [ ] Look for security and performance issues
- [ ] Ensure documentation is clear

### Providing Feedback

- [ ] Be specific and actionable in comments
- [ ] Explain reasoning behind suggestions
- [ ] Balance criticism with recognition
- [ ] Use appropriate severity levels
- [ ] Offer to help with complex changes

### Follow-up

- [ ] Respond to questions promptly
- [ ] Re-review changes within 24 hours
- [ ] Approve when code meets standards
- [ ] Thank author for addressing feedback

---

## 🎯 Key Principles

**Quality Over Speed**: Take time to do thorough reviews  
**Teach, Don't Just Correct**: Help teammates learn and improve  
**Focus on Impact**: Prioritize issues that matter most  
**Be Constructive**: Suggest solutions, not just problems  
**Stay Collaborative**: Code review is a team activity, not a gate