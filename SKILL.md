# Agent Anti-Pattern Code Review Skill

## Purpose
Teach agents to identify and flag common anti-patterns in code reviews.

## Anti-Patterns to Detect

### 1. God Objects
A class that tries to do too much, accumulating responsibilities that should be spread across multiple classes.

**Signs:**
- Class with 500+ lines
- Many unrelated methods (e.g., `UserService` doing email, auth, billing, logging)
- Low cohesion, high coupling

**Better:** Single Responsibility Principle - each class does one thing well.

---

### 2. Tight Coupling
Classes that depend heavily on concrete implementations rather than abstractions.

**Signs:**
- Direct instantiation: `new Service()` inside classes
- Changing one class requires changes in many others
- No interface/use of dependency injection

**Better:** Depend on abstractions (interfaces), not concretions.

---

### 3. Hidden Global State
Mutable shared state hidden inside modules, causing unpredictable behavior.

**Signs:**
- Module-level variables that get modified
- Singleton patterns without justification
- Race conditions in async code

**Better:** Pass dependencies explicitly, use immutable data.

---

### 4. N+1 Queries
Making N database queries for N rows instead of batching.

**Signs:**
- Loop calling `db.get()` or `repo.findById()` inside `forEach`
- Missing eager loading / batch queries

**Better:** Use `whereIn`, `include`, `join`, or batch queries.

---

### 5. Over-Abstracted Factories
Unnecessary indirection through factories, builders, abstractions with no benefit.

**Signs:**
- `FactoryFactory<T>` pattern
- 5 layers of abstraction for simple object creation
- Interface/implementation pairs that are 1:1

**Better:** YAGNI - You Aren't Gonna Need It. Keep it simple.

---

### 6. Premature Optimization
Optimizing before profiling, making code complex for marginal gains.

**Signs:**
- Caching every method result
- Complex memoization for rarely-called functions
- Micro-optimizations that hurt readability

**Better:** Write clear code first, profile, then optimize the bottlenecks.

---

### 7. Circular Dependencies
Class A imports B, B imports C, C imports A.

**Signs:**
- Build errors about circular references
- Complex import statements
- Classes that can't be imported in isolation

**Better:** Refactor to use dependency injection, move shared code to a new module.

---

### 8. Silent Exception Swallowing
Catching errors and doing nothing, or logging but not handling.

**Signs:**
- `catch (e) { }` empty catch blocks
- `catch { return null }` returning null from anywhere
- Generic error handling that hides bugs

**Better:** Handle errors explicitly, throw specific errors, use Result types.

---

### 9. Magic Numbers/Strings
Hard-coded values without explanation.

**Signs:**
- `if (status === 1)` instead of `if (status === ORDER_STATUS.PENDING)`
- `delay(300000)` instead of `delay(5 * MINUTES)`

**Better:** Use named constants, enums, or config objects.

---

### 10. Copy-Paste Code
Duplicated logic that should be extracted.

**Signs:**
- Same 10+ lines appearing in multiple files
- Slight variations of the same algorithm

**Better:** Extract to shared function, use inheritance/composition.

---

### 11. Random/Inconsistent Variable Names
Using meaningless, random, or inconsistent variable names.

**Signs:**
- `let x, y, z` instead of descriptive names
- `const d = new Date()` - what is `d`?
- Mixing snake_case and camelCase inconsistently
- Single letter names except in loops (i, j, k are OK)
- Names that don't match domain terms
- Variables that change type during function (array then object)
- `temp`, `tmp`, `data`, `info` as names without meaning

**Better:** Use descriptive names that convey intent. Match domain terminology. Use `createdAt` not `timestamp`, `userCount` not `num`.

---

### 12. Boolean Traps
Using booleans as parameters that obscure meaning.

**Signs:**
- `process(true, false, true)` - what do those booleans mean?
- `enableFeature(userId, true, false)` - what do the booleans do?
- Using numbers as booleans (1/0 instead of true/false)

**Better:** Use named parameters, enums, or config objects:
```typescript
// Bad
createUser(name, true, false)

// Good
createUser(name, { isAdmin: false, sendEmail: true })
```

---

### 13. Long Parameter Lists
Functions/methods with too many parameters.

**Signs:**
- Function with 6+ parameters
- Multiple optional parameters at the end
- Same groups of parameters repeated across functions

**Better:** Use config objects, builder pattern, or split into smaller functions.

---

### 14. Feature Envy
A method accesses another class's data more than its own.

**Signs:**
- Class A method getting/setting many properties on class B
- A function that works primarily with another object's data more than its own
- Excessive delegation patterns

**Better:** Move method to the class it envies. Consider if the method belongs elsewhere.

---

### 15. Speculative Generality
Adding code "just in case" for future use.

**Signs:**
- Commented-out code left in production
- Unused parameters in function signatures
- Abstract classes with only one concrete subclass
- Empty try-catch blocks

**Better:** YAGNI - implement what you need now, not what you might need.

---

### 16. Primitive Obsession
Using primitives where objects would be better.

**Signs:**
- `String dateString` instead of `Date` object
- `Number status` instead of proper enum
- Parsing strings constantly instead of typed objects

**Better:** Use domain-specific types, enums, and value objects.

---

## Review Prompt Template

When reviewing code, use this structure:

```markdown
## Anti-Pattern Review

### Found Issues

#### Issue: [Name]
**Severity:** [High/Medium/Low]
**Location:** [File:Line]
**Problem:** [Description]
**Example (Bad):**
```[lang]
// problematic code here
```
**Suggestion:** [How to fix]

### Summary
- Total issues: N
- Critical: N
- Warnings: N
- Suggestions: N
```

## Usage

This skill is automatically loaded when reviewing code. Apply these checks to all code reviews.
