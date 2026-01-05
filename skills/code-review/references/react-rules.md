# React-Specific Rules

## Hook Rules Violations

```typescript
// BAD - Conditional hooks
if (condition) {
  const [state, setState] = useState();
}

// BAD - Hooks in loops
items.forEach(() => {
  const ref = useRef();
});

// BAD - Hooks in nested functions
const handleClick = () => {
  const [state, setState] = useState(); // Wrong!
};
```

## Prop Drilling

If passing props through 3+ levels, consider:
- Zustand store for global state
- React Context for scoped state
- Component composition

## Key Prop Issues

```typescript
// BAD - Using index as key for dynamic lists
{items.map((item, index) => <Item key={index} {...item} />)}

// GOOD - Use stable unique identifier
{items.map((item) => <Item key={item.id} {...item} />)}
```

## State Management

- Is local state used when it could be derived?
- Is state lifted too high or not high enough?
- Are Zustand stores properly scoped?

```typescript
// BAD - Derived state stored separately
const [items, setItems] = useState([]);
const [count, setCount] = useState(0);
// count is always items.length

// GOOD - Derive from source of truth
const [items, setItems] = useState([]);
const count = items.length;
```

## useEffect Issues

```typescript
// BAD - Missing dependencies
useEffect(() => {
  fetchData(userId);
}, []); // userId should be in deps

// BAD - Object/function dependencies causing infinite loops
useEffect(() => {
  doSomething(config);
}, [config]); // config is new object every render

// GOOD - Stable dependencies
const stableConfig = useMemo(() => config, [config.id]);
useEffect(() => {
  doSomething(stableConfig);
}, [stableConfig]);
```

## Event Handler Binding

```typescript
// BAD - Creating new function in render
<button onClick={() => handleClick(id)}>

// GOOD - Stable callback
const handleItemClick = useCallback(() => handleClick(id), [id]);
<button onClick={handleItemClick}>

// ALSO GOOD - If it's a simple leaf component that won't re-render children
<button onClick={() => handleClick(id)}> // OK for buttons
```

## Controlled vs Uncontrolled Components

```typescript
// BAD - Mixing controlled and uncontrolled
<input value={value} /> // Missing onChange

// GOOD - Fully controlled
<input value={value} onChange={e => setValue(e.target.value)} />

// ALSO GOOD - Fully uncontrolled
<input defaultValue={initialValue} ref={inputRef} />
```

## Fragments

```typescript
// BAD - Unnecessary wrapper div
return (
  <div>
    <Header />
    <Content />
  </div>
);

// GOOD - Fragment when no wrapper needed
return (
  <>
    <Header />
    <Content />
  </>
);
```

## Conditional Rendering

```typescript
// BAD - Potential falsy value render
{count && <Count value={count} />} // Renders "0" when count is 0

// GOOD - Explicit boolean
{count > 0 && <Count value={count} />}
{Boolean(count) && <Count value={count} />}
```
