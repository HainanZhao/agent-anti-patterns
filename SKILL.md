# Agent Anti-Pattern Code Generator (For Testing)

## Purpose
Generate intentionally bad code containing common anti-patterns. This is used to create test cases for a code review agent/bot.

## Anti-Patterns to Generate

Generate code that demonstrates each anti-pattern. Vary the implementations so they're not all identical.

### 1. God Objects
Create a class that does too much:
- 500+ lines
- Many unrelated methods (email + auth + billing in one service)
- Low cohesion

### 2. Tight Coupling
Code with direct instantiation inside classes:
- `new Service()` inside business logic
- No dependency injection

### 3. Hidden Global State
Mutable module-level variables:
```javascript
let currentUser = null;
let cache = {};
function setUser(u) { currentUser = u; }
```

### 4. N+1 Queries
Loop with individual DB calls:
```javascript
for (const id of ids) {
  const user = await db.getUser(id);
  // process user
}
```

### 5. Over-Abstracted Factories
Unnecessary layers:
- Factory that creates Factory that creates Factory
- Interface for every single class

### 6. Premature Optimization
Unnecessary caching/memoization:
```javascript
const cache = new Map();
function getValue(key) {
  if (cache.has(key)) return cache.get(key);
  const result = compute(key);
  cache.set(key, result);
  return result;
}
// ^ when this function is called once
```

### 7. Circular Dependencies
Circular imports:
```javascript
// a.js
import { b } from './b';
// b.js  
import { a } from './a';
```

### 8. Silent Exception Swallowing
Empty catch blocks:
```javascript
try {
  riskyOperation();
} catch (e) {
  // do nothing
}
```

### 9. Magic Numbers/Strings
Hard-coded values:
```javascript
if (status === 1) { ... } // what is 1?
setTimeout(fn, 300000); // 5 minutes?? why?
```

### 10. Copy-Paste Code
Duplicate code in multiple files with slight variations.

### 11. Random Variable Names
Meaningless names:
```javascript
let x, y, z;
const d = new Date();
const temp = compute();
const info = fetchData();
```

### 12. Boolean Traps
Obscured boolean params:
```javascript
processUser(id, true, false, true);
// what do the booleans mean?
```

### 13. Long Parameter Lists
Too many parameters:
```javascript
function create(name, email, age, phone, address, city, zip, country, isAdmin, isVerified) {}
```

### 14. Feature Envy
Method that uses another class more than its own:
```javascript
class OrderService {
  process(order) {
    const user = userService.getUser(order.userId); // envying UserService
    const product = productService.getProduct(order.productId);
    // 20 more lines of other class access
  }
}
```

### 15. Speculative Generality
Code "for future use":
- Commented out code
- Unused parameters
- Empty abstract classes

### 16. Primitive Obsession
Primitives instead of types:
```javascript
const status = "1"; // string not number
const date = "2024-01-01"; // string not Date
```

## Output Format

Generate realistic, production-like code with these anti-patterns. Mix multiple anti-patterns in the same file.

### Example Output

```typescript
// This file has multiple anti-patterns for testing

// === ANTI-PATTERN: Random Variable Names ===
let x, y, z;
const d = new Date();
const temp = compute();

// === ANTI-PATTERN: God Object ===
class UserManager {
  async createUser() { /* ... */ }
  async sendEmail() { /* ... */ }
  async processPayment() { /* ... */ }
  async logAnalytics() { /* ... */ }
  async generateReport() { /* ... */ }
  // 20 more methods doing different things
}

// === ANTI-PATTERN: N+1 ===
for (const id of userIds) {
  const user = await db.users.findById(id);
}

// === ANTI-PATTERN: Magic Numbers ===
if (role === 1) { } // 1 = admin?
setTimeout(cleanup, 180000); // 3 min?
```

## Guidelines

1. Generate realistic code that looks like someone actually wrote it
2. Include 3-5 different anti-patterns per file
3. Use variety - don't use the same bad pattern every time
4. Make code compilable/runnable (not just snippets)
5. Include enough context to be reviewable

## Usage

When asked to generate test code for a code review bot, create files with these anti-patterns. The review bot should detect them.
