---
name: frontend-technologies
description: Master frontend development tools, frameworks, and patterns. Learn React, Vue, Angular, Next.js, TypeScript, and modern web development best practices. Use when building UIs, learning frontend frameworks, or optimizing web performance.
---

# Frontend Technologies Skill

Master modern frontend development with comprehensive guidance on building scalable, performant web applications.

## Quick Start

### React Fundamentals
```jsx
// Functional components with hooks
import React, { useState, useEffect } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```

### TypeScript + React
```typescript
interface Props {
  title: string;
  count: number;
  onIncrement: () => void;
}

const Counter: React.FC<Props> = ({ title, count, onIncrement }) => {
  return (
    <div>
      <h1>{title}</h1>
      <p>Count: {count}</p>
      <button onClick={onIncrement}>Increment</button>
    </div>
  );
};
```

### Vue 3 Composition API
```vue
<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="count++">Increment</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const count = ref(0);
</script>
```

### Next.js Server Components
```typescript
// app/page.tsx
export default function Page() {
  return (
    <div>
      <h1>Welcome to Next.js 13+</h1>
      <UserList />
    </div>
  );
}

async function UserList() {
  const users = await fetch('api/users');
  return (
    <ul>
      {users.map(user => <li key={user.id}>{user.name}</li>)}
    </ul>
  );
}
```

## Advanced Topics

### State Management
- **Redux**: Centralized state, middleware, selectors
- **Zustand**: Lightweight alternative with hooks
- **Context API**: React built-in solution for prop drilling
- **Recoil**: Atomic state management

### Performance Optimization
- Code splitting and lazy loading
- Component memoization (React.memo, useMemo)
- Image optimization (WebP, responsive sizes)
- Bundle size analysis (Webpack Bundle Analyzer)

### Responsive Design
- CSS Grid and Flexbox layouts
- Mobile-first approach
- Breakpoints and media queries
- Touch-friendly interfaces

### Testing
- Unit tests: Jest, Vitest
- Component testing: React Testing Library
- E2E testing: Cypress, Playwright
- Visual regression testing

## Resources

- [React.dev](https://react.dev)
- [Vue.js](https://vuejs.org)
- [Next.js Docs](https://nextjs.org/docs)
- [MDN Web Docs](https://developer.mozilla.org)
