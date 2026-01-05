---
name: code-review
description: Comprehensive code review for React/TypeScript codebases covering security, performance, and maintainability. Use when reviewing PRs, checking code quality, or analyzing existing code for issues.
---

# Code Review

Perform thorough code reviews covering security, performance, and maintainability for React/TypeScript applications.

## When to Use

- Reviewing pull requests
- Analyzing code quality before merge
- Finding potential issues in existing code
- Pre-commit quality checks

## Coding Standards

Verify code follows [coding-style.md](../../shared/coding-style.md) conventions.

## Review Process

1. **Quick Scan**: Understand purpose, check organization
2. **Detailed Review**: Security, performance, maintainability
3. **Holistic View**: Architecture fit, alternatives, gaps

## Review Categories

For detailed checklists, see:
- [Security Rules](references/security-rules.md)
- [Performance Rules](references/performance-rules.md)
- [Maintainability Rules](references/maintainability-rules.md)
- [React-Specific Rules](references/react-rules.md)

## Output Format

```markdown
## Summary
[1-2 sentence overview]

## Critical Issues
[Security vulnerabilities, breaking bugs]

## Performance Concerns
[UX-impacting issues]

## Maintainability Suggestions
[Non-blocking improvements]

## Positive Notes
[What was done well]
```

## Severity Levels

| Level | Action | Examples |
|-------|--------|----------|
| **Critical** | Must fix | Security holes, data loss |
| **High** | Should fix | Bugs, major perf issues |
| **Medium** | Recommend | Code smells, minor perf |
| **Low** | Nice to have | Style, minor cleanup |
