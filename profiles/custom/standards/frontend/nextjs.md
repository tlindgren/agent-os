## Next.js App Router & TypeScript Guidelines

### 🚨 CRITICAL INSTRUCTIONS

As an AI language model, you MUST adhere to the architectural principles of the Next.js App Router. Failure to do so will result in code that is non-performant, incorrect, or breaks the build.

- **NEVER** use React class components. Always use function components and hooks.
- **NEVER** use the old `pages` directory structure. All routing, UI, and logic MUST use the `src/app` directory.
- **NEVER** fetch data on the client side for the initial render. Prioritize server-side data fetching in Server Components.
- **NEVER** import server-only modules (e.g., accessing a database) into a Client Component.

### 1. Server Components are the Default

- ✅ **DO:** Generate components as **Server Components** by default. They can be `async` and fetch data directly.
- ❌ **DON'T:** Add `'use client'` to the top of a file unless it is absolutely necessary for client-side interactivity.

```typescript
// ✅ CORRECT: A Server Component that fetches data.
import { db } from "@/lib/db";

export default async function Page() {
  const data = await db.query("SELECT * FROM posts");
  return (
    <main>
      {data.map(post => <div key={post.id}>{post.title}</div>)}
    </main>
  );
}
```

### 2. Client Components (`'use client'`) for Interactivity Only

- ✅ **DO:** Use the `'use client'` directive only for components that need interactivity (e.g., `useState`, `useEffect`, event listeners like `onClick`).
- ✅ **DO:** Keep Client Components as small as possible. Push them down the component tree to the leaves. Wrap a Server Component in a Client Component, not the other way around.

```typescript
// ✅ CORRECT: A small, interactive Client Component.
'use client';

import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

### 3. File-System Routing Conventions

- ✅ **DO:** Use the special file names `page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx`, and `route.ts`.
- ✅ **DO:** Use folder hierarchies inside `src/app` to define routes.
- ✅ **DO:** Use square brackets for dynamic route segments (e.g., `src/app/users/[userId]/page.tsx`).

| File | Purpose |
|---|---|
| `page.tsx` | The main UI for a route. |
| `layout.tsx` | Shared UI for a segment and its children. |
| `loading.tsx` | Loading UI for a segment using React Suspense. |
| `error.tsx` | Error UI for a segment using React Error Boundary. |
| `route.ts` | API endpoints (Route Handlers). |

### 4. Data Fetching

- ✅ **DO:** Fetch data in **Server Components** using `async`/`await`. This is the primary pattern.
- ✅ **DO:** Use Route Handlers (`route.ts`) to create API endpoints for client-side data fetching if needed (e.g., for search functionality that updates as the user types).
- ✅ **DO:** Use Server Actions for data mutations (e.g., form submissions). They are functions defined with `'use server'` and can be called from Client Components.

```typescript
// ✅ CORRECT: Using a Server Action for a form.

// lib/actions.ts
'use server';
import { revalidatePath } from 'next/cache';

export async function createPost(formData: FormData) {
  const title = formData.get('title');
  // ... logic to save to database
  revalidatePath('/'); // Revalidate the homepage to show the new post
}

// app/components/PostForm.tsx (Client Component)
'use client';
import { createPost } from '@/lib/actions';

export function PostForm() {
  return (
    <form action={createPost}>
      <input type="text" name="title" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### 5. TypeScript: `.ts` vs `.tsx`

- ✅ **DO:** Use `.ts` for files containing only TypeScript logic, types, or configuration (e.g., `lib/utils.ts`, `types/index.ts`).
- ✅ **DO:** Use `.tsx` for files that contain any JSX (i.e., React components).

```
src/
├── lib/
│   └── themes.ts          # ✅ Data and functions, no JSX
├── components/
│   └── ThemeCard.tsx      # ✅ React component with JSX
└── app/
    └── page.tsx           # ✅ Page component with JSX
```

### 6. Dynamic Route Parameters (Next.js 15+)

⚠️ **BREAKING CHANGE:** Next.js 15 changed how dynamic route parameters work. They are now async and must be unwrapped with `React.use()`.

- ✅ **DO:** Use `React.use()` to unwrap params in Client Components.
- ✅ **DO:** Use `await params` in Server Components (which are already async).
- ✅ **DO:** Update TypeScript interfaces to reflect that params are now `Promise<T>`.

```typescript
// ✅ CORRECT: Client Component with async params
'use client';
import { use } from 'react';

interface PageProps {
  params: Promise<{ slug: string }>;
}

export default function Page({ params }: PageProps) {
  const { slug } = use(params);
  // ... rest of component
}

// ✅ CORRECT: Server Component with async params
interface PageProps {
  params: Promise<{ id: string }>;
}

export default async function Page({ params }: PageProps) {
  const { id } = await params;
  // ... rest of component
}

// ❌ INCORRECT: Direct destructuring (Next.js 14 style)
export default function Page({ params }: { params: { slug: string } }) {
  const { slug } = params; // This will cause errors in Next.js 15+
}
```

### 7. Styling

- ✅ **DO:** Use Tailwind CSS for styling, following the utility-first approach.
- ✅ **DO:** Use CSS Modules (`*.module.css`) for component-scoped styles when necessary.
- ❌ **DON'T:** Use regular stylesheets (`*.css`) for components, as they are global. Global styles should only be in `src/app/globals.css`.

### AI Model Verification Checklist

Before generating code, verify:

1.  **Component Type:** Is it a Server or Client component? Does it correctly use (or not use) `'use client'`?
2.  **File Naming:** Does the file name match its purpose (`page.tsx`, `layout.tsx`, etc.)?
3.  **Data Fetching:** Is data fetched on the server where possible? Are mutations handled via Server Actions?
4.  **File Extension:** Is `.ts` used for logic-only files and `.tsx` for files with JSX?
5.  **Routing:** Does the code use `next/link` for navigation and follow the App Router file structure?
6.  **Dynamic Params (Next.js 15+):** Are dynamic route parameters properly unwrapped using `React.use()` (Client Components) or `await` (Server Components)?
