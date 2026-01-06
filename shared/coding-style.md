# React Coding Style Skill

Guide code generation following the established React/TypeScript coding patterns for this project.

## Trigger

Use this skill when:
- Creating new React components
- Writing custom hooks
- Adding helper/utility functions
- Structuring new feature pages

## Project Structure

When creating a new feature page or component module:

```
FeaturePage/
├── index.tsx              # Main component (thin, delegates to logic hooks)
├── styles.module.less     # Styles using CSS Modules
├── types.ts               # Shared types for this module
├── helpers/               # Pure utility functions
│   └── index.ts           # Barrel export
├── logic/                 # Custom hooks organized by domain
│   ├── index.ts           # Main logic hook that composes sub-hooks
│   ├── types.ts           # Logic-specific types
│   └── use[Feature]Logic.ts
└── components/            # Sub-components
    └── ComponentName/
        ├── index.tsx
        ├── styles.module.less
        ├── types.ts
        ├── helpers/
        └── widgets/       # Smaller sub-components
            └── index.ts   # Barrel export
```

## Naming Conventions

| Element | Pattern | Example |
|---------|---------|---------|
| Component | PascalCase | `TaskInfo`, `DetailNavBar` |
| Component file | `index.tsx` in named folder | `TaskInfo/index.tsx` |
| Custom Hook | `use[Feature]Logic` | `useSubmitLogic`, `useTaskLogic` |
| Hook file | camelCase | `useSubmitLogic.ts` |
| Helper Function | verb-prefix camelCase | `getGlobalNoticeList`, `convertItemToBtn` |
| Props Type | `[Component]Props` | `TaskInfoProps` |
| Hook Props Type | `Use[Hook]Props` | `UseSubmitLogicProps` |
| Hook Return Type | `Use[Hook]ReturnValue` | `UseCurrentSubTaskLogicReturnValue` |
| Constants | UPPER_SNAKE_CASE | `MAX_EXPOSED_BUTTONS`, `DEFAULT_INDEX` |
| Style file | `styles.module.less` | - |
| Type file | `types.ts` | - |

## React Component Template

```tsx
import React, { FC, useMemo } from 'react';

import { externalLib } from 'external-lib';

import { InternalModule } from '@/path/to/module';
import { ChildWidget } from './widgets';
import { useComponentLogic } from './logic';
import { ComponentNameProps } from './types';

import styles from './styles.module.less';

export const ComponentName: FC<ComponentNameProps> = (props) => {
  const { propA, propB, onAction } = props;

  const derivedValue = useMemo(() => {
    // Computation logic
    return computedResult;
  }, [propA]);

  const { state, callback } = useComponentLogic({ propB });

  return (
    <div className={styles.container}>
      <ChildWidget value={derivedValue} />
      {state ? (
        <div className={styles.content}>
          {/* Content */}
        </div>
      ) : null}
    </div>
  );
};
```

## Custom Hook Template

```ts
import { useCallback, useMemo } from 'react';

import { externalLib } from 'external-lib';

import { InternalType } from '@/path/to/types';

interface UseFeatureLogicProps {
  dataSource: DataSource;
  formRef: FormRef;
}

export const useFeatureLogic = (props: UseFeatureLogicProps) => {
  const { dataSource, formRef } = props;

  const derivedState = useMemo(() => {
    if (!dataSource) {
      return defaultValue;
    }
    return computeValue(dataSource);
  }, [dataSource]);

  const handleAction = useCallback(async () => {
    // Step-by-step logic with logging for complex operations
    logger().info('Step 1: Validate input');
    if (!isValid) {
      return false;
    }

    logger().info('Step 2: Process data');
    const result = await processData();

    return result;
  }, [dataSource]);

  return {
    derivedState,
    handleAction,
  };
};

export type UseFeatureLogicReturnValue = ReturnType<typeof useFeatureLogic>;
```

## Helper Function Template

```ts
import { SomeType } from './types';

const SOME_CONSTANT = 'value';

/**
 * 业务逻辑说明（Chinese comment for business logic）
 *
 * 逻辑：
 * - 条件 1：处理方式
 * - 条件 2：处理方式
 */
export const getProcessedList = (data: InputData): OutputItem[] => {
  const result: OutputItem[] = [];

  // Early return for edge cases
  if (!data?.length) {
    return result;
  }

  data.forEach((item) => {
    if (!item.isValid) {
      return;
    }

    result.push(transformItem(item));
  });

  return result;
};
```

## Types Template

```ts
import { ExternalType } from 'external-lib';

import { InternalType } from '@/path/to/types';

// Props types
export type ComponentNameProps = {
  requiredProp: string;
  optionalProp?: number;
  onAction: (value: string) => void;
} & Pick<AnotherProps, 'sharedProp'>;

// Function types
export type GetListFn = (data: DataSource) => Item[] | undefined;

// Derived types
export type PartialComponentProps = Pick<ComponentNameProps, 'requiredProp' | 'onAction'>;
```

## Import Order

Organize imports in this order with blank lines between groups:

1. React imports
2. External packages (alphabetical)
3. Internal absolute imports (`@/...`, `@corehr/...`)
4. Relative imports (parent `../` before sibling `./`)
5. Style imports (always last)

```ts
import React, { FC, useMemo, useCallback } from 'react';

import { useDialog } from '@universe-design/mobile-react';
import i18next from 'i18next';

import { someUtil } from '@corehr/tools';
import { SharedComponent } from '@/components/Shared';
import { ParentType } from '../types';
import { ChildWidget } from './widgets';
import { useLocalLogic } from './logic';

import styles from './styles.module.less';
```

## Export Strategy

- **Named exports only**: Never use default exports
- **Barrel exports**: Create `index.ts` files for re-exporting

```ts
// components/index.ts
export * from './ComponentA';
export * from './ComponentB';

// widgets/index.ts
export * from './WidgetA';
export * from './WidgetB';
```

## Code Style Rules

### Functional Programming
- Use `forEach`, `map`, `filter`, `reduce` over imperative loops
- Prefer immutable operations (spread operator, `concat`)
- Use pure functions in helpers

### Early Returns
- Guard clauses at the top of functions
- Return early for invalid/empty states

```ts
if (!data?.length) {
  return [];
}

if (!isValid) {
  return null;
}
```

### Conditional Rendering
- Use ternary with `null` for conditional JSX
- Avoid `&&` for conditional rendering (use ternary instead)

```tsx
{condition ? (
  <Component />
) : null}
```

### Comments
- Chinese comments for business logic explanations
- JSDoc-style comments for complex public functions
- Step-by-step comments in complex async flows

### CSS Modules
- Always import as `styles`
- Use `className={styles.containerName}` pattern

### Path Aliases
- Use `@/` for src-level absolute imports
- Use relative imports for same-module files

## Anti-patterns to Avoid

- Default exports
- Inline styles
- Class components
- `any` type (use proper typing)
- Logic in components (extract to hooks)
- `&&` for conditional rendering
- Imperative loops when functional alternatives exist
