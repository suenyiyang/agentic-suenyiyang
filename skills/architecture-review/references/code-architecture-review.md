# Code Architecture Review

## Component Structure

### Separation of Concerns
- Is logic separated from rendering? (hooks + views)
- Are responsibilities clearly defined?
- Can components be tested in isolation?

### Component Size
```typescript
// BAD - God component
const UserDashboard = () => {
  // 500+ lines of mixed concerns
};

// GOOD - Composed from focused components
const UserDashboard = () => (
  <DashboardLayout>
    <UserStats />
    <RecentActivity />
    <QuickActions />
  </DashboardLayout>
);
```

### Component Cohesion
- Do all parts of a component serve the same purpose?
- Could parts be extracted into separate components?

## State Architecture

### State Location
- Is state at the right level?
- Is global state truly global, or could it be local?
- Are Zustand stores properly scoped?

### State Shape
```typescript
// BAD - Nested normalized data
{ users: { byId: {}, allIds: [], selected: {}, loading: {} } }

// GOOD - Flat, focused stores
useUserListStore    // list, filters, pagination
useSelectedUserStore // current selection
```

### Derived State
- Is computed state being stored when it should be derived?
- Are memoization hooks used appropriately?

## Code Organization

### Folder Structure
```
// GOOD - Feature-based
/features
  /onboarding
    /components
    /hooks
    /stores
    /types
  /employees
    /components
    /hooks
    /stores
    /types

// ALSO GOOD - For smaller apps
/components
/hooks
/stores
/types
```

### File Naming
- Consistent with coding-style.md?
- Predictable and searchable?

## Error Handling Architecture

### Error Boundaries
- Are error boundaries placed strategically?
- Do critical sections have fallbacks?

### Error Propagation
- Are errors handled at the right level?
- Is error reporting centralized?

## Performance Architecture

### Code Splitting
- Are large features lazy loaded?
- Are routes split appropriately?

### Caching Strategy
- Is data caching implemented?
- Are cache invalidation rules clear?
