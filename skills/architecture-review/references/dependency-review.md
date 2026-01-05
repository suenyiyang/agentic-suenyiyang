# Dependency Review

## Package Dependencies

### Version Health
- Are dependencies up to date?
- Are there known vulnerabilities? (check npm audit, Snyk)
- Are deprecated packages being used?

### Bundle Impact
- Are heavy dependencies justified?
- Could lighter alternatives be used?
- Is tree-shaking working properly?

### Maintenance Risk
- Is the package actively maintained?
- How many open issues/PRs?
- Last release date?

## Internal Dependencies

### Circular Dependencies
```
// BAD - Circular import
// moduleA.ts imports from moduleB.ts
// moduleB.ts imports from moduleA.ts

// GOOD - Extract shared code
// shared.ts contains common code
// moduleA.ts imports from shared.ts
// moduleB.ts imports from shared.ts
```

### Dependency Direction
```
// GOOD - Depend on abstractions, not concretions
UI → Domain → Infrastructure

// BAD - Infrastructure leaking into domain
Domain → Infrastructure → UI
```

### Layer Violations
- UI should not import from infrastructure directly
- Domain should not know about specific frameworks
- Keep dependencies flowing in one direction

## Module Coupling

### Tight Coupling Signs
- Multiple modules changing together frequently
- Circular imports between modules
- God modules that everything depends on

### Loose Coupling Patterns
- Dependency injection
- Event-driven communication
- Interface segregation
