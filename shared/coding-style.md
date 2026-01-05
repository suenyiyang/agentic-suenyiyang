# Coding Style Guide

Single source of truth for coding conventions. Referenced by multiple skills and commands.

## Architecture Pattern

**Custom Hooks + View** - Separate logic from rendering:

```
component-name/
├── index.tsx              # Container: combines hook + view
├── use-component-name.ts  # Hook: all logic, state, API calls
├── component-name-view.tsx # View: pure rendering, receives props
└── component-name.module.css # Styles
```

## Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Files | kebab-case | `user-profile.tsx`, `use-employee-list.ts` |
| Components | PascalCase | `UserProfile`, `EmployeeList` |
| Hooks | camelCase with `use` prefix | `useEmployeeList`, `useOnboardingForm` |
| Stores (Zustand) | camelCase with `Store` suffix | `employeeStore`, `onboardingStore` |
| CSS Modules | kebab-case | `component-name.module.css` |
| Constants | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT`, `API_TIMEOUT` |

## State Management (Zustand)

```typescript
import { create } from 'zustand';

interface EmployeeStore {
  employees: Employee[];
  selectedEmployee: Employee | null;
  setEmployees: (employees: Employee[]) => void;
  selectEmployee: (employee: Employee) => void;
}

export const useEmployeeStore = create<EmployeeStore>((set) => ({
  employees: [],
  selectedEmployee: null,
  setEmployees: (employees) => set({ employees }),
  selectEmployee: (employee) => set({ selectedEmployee: employee }),
}));
```

## API Calls (Internal SDK)

Use domain methods:

```typescript
// GOOD - Domain methods
await sdk.employees.list();
await sdk.employees.create(data);
await sdk.employees.getById(id);
await sdk.onboarding.getSteps(employeeId);

// AVOID - Generic methods
await sdk.get('/api/employees');
```

## Styling

**Primary: CSS Modules**
```tsx
import styles from './component-name.module.css';
<div className={styles.container}>
```

**Transitioning to: Tailwind CSS**
```tsx
<div className="flex flex-col gap-4 p-6">
```

## Component Structure

```typescript
// index.tsx - Container (thin)
import { useUserOnboarding } from './use-user-onboarding';
import { UserOnboardingView } from './user-onboarding-view';

export const UserOnboarding = () => {
  const props = useUserOnboarding();
  return <UserOnboardingView {...props} />;
};
```

```typescript
// use-user-onboarding.ts - Logic Hook (thick)
export const useUserOnboarding = () => {
  const [step, setStep] = useState(0);
  const [loading, setLoading] = useState(false);

  const submitOnboarding = async (data: OnboardingData) => {
    setLoading(true);
    await sdk.employees.createOnboarding(data);
    setLoading(false);
  };

  return { step, setStep, loading, submitOnboarding };
};
```

```typescript
// user-onboarding-view.tsx - Pure View (receives props, no logic)
interface UserOnboardingViewProps {
  step: number;
  setStep: (step: number) => void;
  loading: boolean;
  submitOnboarding: (data: OnboardingData) => void;
}

export const UserOnboardingView = (props: UserOnboardingViewProps) => {
  return <div className={styles.container}>...</div>;
};
```

## Error Handling

```typescript
try {
  await sdk.employees.create(data);
} catch (error) {
  setError(getErrorMessage(error));
  reportError(error); // Error reporting service
}
```

## Type Safety

- Never use `any`
- Validate unknown data at boundaries
- Use strict TypeScript config
