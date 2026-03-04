# Agent Anti-Pattern Code Generator

An Agent Skill for generating intentionally bad code with anti-patterns, for testing code review bots.

## What is this?

This skill generates test cases for code review agents/bots by creating code that contains common anti-patterns. This helps test if the review bot can detect these issues.

## Use Case

1. You have a code review bot (like AICodeReview)
2. You want to test if it detects various anti-patterns
3. Use this skill to generate bad code as test cases
4. Run your review bot against the generated code
5. Verify it detects the anti-patterns

## Anti-Patterns Covered (16 total)

| # | Pattern | Description |
|---|---------|-------------|
| 1 | God Objects | Classes doing too much |
| 2 | Tight Coupling | Direct instantiation |
| 3 | Hidden Global State | Mutable module vars |
| 4 | N+1 Queries | Loop with individual DB calls |
| 5 | Over-Abstracted | Unnecessary factories |
| 6 | Premature Optimization | Unnecessary caching |
| 7 | Circular Dependencies | Circular imports |
| 8 | Silent Exceptions | Empty catch blocks |
| 9 | Magic Numbers | Hard-coded values |
| 10 | Copy-Paste | Duplicated code |
| 11 | Bad Variable Names | x, y, d, temp, data |
| 12 | Boolean Traps | Obscured booleans |
| 13 | Long Parameters | Too many params |
| 14 | Feature Envy | Wrong class methods |
| 15 | Speculative Generality | Unused code |
| 16 | Primitive Obsession | Strings for everything |

## Usage

```bash
# Generate test code for review bot
agent-antipatterns generate --output ./test-cases --patterns god-object,n+1,magic-numbers
```

## Example

When the skill runs, it generates code like:

```typescript
// test-user-service.ts
// Anti-pattern: God Object + Random Names + Magic Numbers

let x, y, z;

class UserService {
  async create(name, email) { }
  async sendEmail() { }    // Should be EmailService
  async processPayment() { } // Should be PaymentService
  async log() { }         // Should be Logger
  
  // 20 more methods...
  
  validate(id) {
    if (id === 1) return true; // Magic number
  }
}
```

## Integration

This skill can be used with:
- AICodeReview - generate test cases before running review
- Gemini/Claude agents - create test scenarios
- CI/CD - generate negative test cases

## License

MIT
