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
