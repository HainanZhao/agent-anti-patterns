# Agent Anti-Pattern Code Review Skill

An Agent Skill for detecting common anti-patterns in code reviews.

## What is this?

A skill that teaches AI agents (Gemini, Claude, etc.) to identify and flag common software anti-patterns during code reviews.

## Anti-Patterns Covered

| Pattern | Severity | Description |
|---------|----------|-------------|
| God Objects | High | Classes doing too much |
| Tight Coupling | High | Over-dependence on concretions |
| Hidden Global State | High | Mutable shared state |
| N+1 Queries | High | Database inefficiency |
| Over-Abstracted | Medium | Unnecessary indirection |
| Premature Optimization | Medium | Premature perf tweaks |
| Circular Dependencies | High | Circular imports |
| Silent Exception Swallowing | High | Hidden errors |
| Magic Numbers/Strings | Low | Hard-coded values |
| Copy-Paste Code | Medium | Duplicated logic |

## Usage

### As Agent Skill

Use this skill when reviewing code. The agent will automatically check for these anti-patterns.

### CLI Usage

```bash
# Review a file
agent-antipatterns review src/server.ts

# Check for specific pattern
agent-antipatterns check --pattern n+1 src/
```

## Installation

```bash
npm install -g agent-anti-patterns
```

## Integration

This skill can be integrated with:
- Gemini CLI
- Claude Code  
- AICodeReview
- Any LLM-based code review tool

## License

MIT
