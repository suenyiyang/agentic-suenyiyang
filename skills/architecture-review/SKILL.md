---
name: architecture-review
description: Review system architecture at all levels (component, service, system) to find potential problems. Use when assessing design decisions, evaluating technical debt, or reviewing new architecture proposals.
---

# Architecture Review

Review architecture top-down at all levels to identify issues, risks, and improvement opportunities.

## When to Use

- Evaluating new feature architecture proposals
- Assessing existing system design
- Reviewing dependency choices
- Identifying technical debt
- Pre-implementation design review

## Coding Standards

Verify architecture follows [coding-style.md](../../shared/coding-style.md) conventions.

## Project-Specific Rules

Check for custom rules in project root: `.arch-review/references/`

If present, load and apply project-specific review criteria before global rules.

## Review Levels (Top-Down)

### 1. System Level
- Overall architecture patterns
- Service boundaries
- Data flow across system
- External integrations

### 2. Service Level
- API design
- Service responsibilities
- Inter-service communication
- Database design

### 3. Component Level
- Module structure
- Component responsibilities
- Internal dependencies
- State management

## Review Categories

For detailed checklists, see:
- [Dependency Review](references/dependency-review.md)
- [Code Architecture Review](references/code-architecture-review.md)
- [Data Flow Review](references/data-flow-review.md)
- [API Design Review](references/api-design-review.md)

## Output Format

```markdown
## Architecture Summary
[Brief description of what was reviewed]

## Level: System
[System-level findings]

## Level: Service
[Service-level findings]

## Level: Component
[Component-level findings]

## Risk Assessment
| Risk | Severity | Impact | Recommendation |
|------|----------|--------|----------------|

## Technical Debt
[Identified debt items and suggested prioritization]

## Recommendations
[Actionable next steps]
```

## Severity Levels

| Level | Description |
|-------|-------------|
| **Critical** | Fundamental design flaw, blocks progress |
| **High** | Significant issue, should address soon |
| **Medium** | Improvement opportunity |
| **Low** | Nice to have, future consideration |
