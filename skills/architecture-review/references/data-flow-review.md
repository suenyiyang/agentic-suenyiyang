# Data Flow Review

## Unidirectional Data Flow

### React Data Flow
```
State → Props → UI → Events → State Update
```

### Issues to Check
- Props drilling through many levels
- State updates from unexpected sources
- Side effects in render path

## API Data Flow

### Request/Response Pattern
```typescript
// GOOD - Clear data flow
Component
  → Hook (useEmployeeList)
    → SDK (sdk.employees.list)
      → API
    ← Response
  ← Processed data
← Render
```

### Loading States
- Is loading state tracked consistently?
- Are there race conditions in data fetching?
- Is stale data handled appropriately?

### Error Flow
- Do errors bubble up correctly?
- Are error states displayed to users?
- Is error recovery possible?

## State Synchronization

### Server State vs Client State
```typescript
// Server state - comes from API
const { data: employees } = useEmployees();

// Client state - UI-only
const [selectedId, setSelectedId] = useState(null);
```

### Cache Invalidation
- When should data be refetched?
- Are mutations updating cached data correctly?

## Event Flow

### User Events
```
User Action → Event Handler → State Update → Re-render
```

### System Events
- WebSocket messages
- Browser events (resize, online/offline)
- Timer-based updates

### Event Bubbling Issues
- Are events stopping propagation when needed?
- Are there conflicting handlers?

## Form Data Flow

### Controlled vs Uncontrolled
- Is the pattern consistent?
- Are form states managed predictably?

### Validation Flow
```
Input → Validate → Show Error → Submit → Server Validate → Respond
```

### Submission Flow
- Is optimistic update used appropriately?
- How are submission errors handled?
