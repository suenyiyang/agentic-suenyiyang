# Maintainability Rules

## Code Organization

- Is logic separated from rendering? (hooks + views pattern)
- Are files in the right directories?
- Is naming consistent? (kebab-case files, PascalCase components)

## Type Safety

```typescript
// BAD - Using `any`
const handleData = (data: any) => { ... }

// BAD - Type assertions without validation
const employee = response as Employee;

// GOOD - Proper types with runtime validation
interface Employee {
  id: string;
  name: string;
  department: string;
}
const parseEmployee = (data: unknown): Employee => {
  // Validate shape at runtime
};
```

## Error Handling

```typescript
// BAD - Silent failures
try {
  await sdk.employees.create(data);
} catch (e) {
  // Nothing
}

// GOOD - Proper error handling
try {
  await sdk.employees.create(data);
} catch (error) {
  setError(getErrorMessage(error));
  reportError(error);
}
```

## Complexity

Functions should do one thing. Avoid deep nesting (> 3 levels).

```typescript
// BAD - Complex condition inline
if (user && user.role === 'admin' && user.department === 'HR' && !user.isDisabled) { ... }

// GOOD - Named variable
const canAccessHRFeatures = user && user.role === 'admin' && user.department === 'HR' && !user.isDisabled;
if (canAccessHRFeatures) { ... }
```

## Magic Numbers/Strings

```typescript
// BAD
if (status === 3) { ... }
setTimeout(fn, 86400000);

// GOOD
const STATUS_APPROVED = 3;
const ONE_DAY_MS = 24 * 60 * 60 * 1000;
if (status === STATUS_APPROVED) { ... }
setTimeout(fn, ONE_DAY_MS);
```

## DRY (Don't Repeat Yourself)

- Look for duplicated logic that should be extracted
- But avoid premature abstraction (3 similar usages before extracting)

## Dead Code

- Remove unused variables, functions, imports
- Delete commented-out code (use version control instead)

## Testing

- Are critical paths covered by tests?
- Are edge cases tested?
- Do tests actually assert meaningful things?
- Are tests readable and maintainable?

## Documentation

- Are complex algorithms documented?
- Are non-obvious business rules explained?
- Are public APIs documented?
