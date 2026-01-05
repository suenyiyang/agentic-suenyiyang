# Performance Rules

## Unnecessary Re-renders

```typescript
// BAD - New object reference every render
<Component style={{ color: 'red' }} />
<Component onClick={() => handleClick(id)} />

// GOOD - Stable references
const style = useMemo(() => ({ color: 'red' }), []);
const handleItemClick = useCallback(() => handleClick(id), [id]);
<Component style={style} onClick={handleItemClick} />
```

## Missing Memoization

```typescript
// BAD - Expensive computation every render
const sortedEmployees = employees.sort((a, b) => a.name.localeCompare(b.name));

// GOOD - Memoize expensive operations
const sortedEmployees = useMemo(
  () => [...employees].sort((a, b) => a.name.localeCompare(b.name)),
  [employees]
);
```

## Memory Leaks

```typescript
// BAD - No cleanup
useEffect(() => {
  const subscription = sdk.employees.subscribe(setEmployees);
}, []);

// GOOD - Cleanup subscriptions
useEffect(() => {
  const subscription = sdk.employees.subscribe(setEmployees);
  return () => subscription.unsubscribe();
}, []);
```

## Bundle Size

- Check for unnecessary imports
- Verify tree-shaking works (`import { specific } from 'lib'` not `import lib`)
- Look for heavy dependencies that could be lighter
- Consider lazy loading for large components

```typescript
// GOOD - Lazy load heavy components
const HeavyComponent = lazy(() => import('./HeavyComponent'));
```

## Network Efficiency

```typescript
// BAD - Fetching on every render
useEffect(() => {
  fetchEmployees();
}); // Missing dependency array!

// BAD - Multiple redundant calls
await sdk.employees.getById(id);
await sdk.employees.getById(id); // Duplicate!

// GOOD - Fetch once, cache appropriately
useEffect(() => {
  fetchEmployees();
}, []); // Or use React Query/SWR for caching
```

## Large Lists

```typescript
// BAD - Rendering thousands of items
{employees.map(e => <EmployeeRow key={e.id} {...e} />)}

// GOOD - Virtualize large lists
import { FixedSizeList } from 'react-window';
<FixedSizeList height={600} itemCount={employees.length} itemSize={50}>
  {({ index, style }) => <EmployeeRow style={style} {...employees[index]} />}
</FixedSizeList>
```

## Image Optimization

- Check for unoptimized images
- Verify lazy loading for below-fold images
- Use appropriate formats (WebP when supported)
