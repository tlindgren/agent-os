---
description: Guidelines for using shadcn/ui components and the CLI in Next.js projects with Tailwind CSS v4.
globs: "components/ui/**/*.tsx, lib/utils.ts, app/globals.css, src/index.css, src/app/globals.css, styles/globals.css"
---

# shadcn/ui Usage Guidelines (Tailwind CSS v4)

**Last Updated:** October 2025
**Tailwind CSS v4:** Stable (released January 22, 2025)
**shadcn/ui:** Compatible with Next.js 14 & 15, React 18 & 19

---

## 🚨 CRITICAL INSTRUCTIONS FOR AI ASSISTANTS 🚨

As an AI assistant, you MUST adhere to the following principles when working with shadcn/ui:

1. **NEVER** manually copy and paste component code from the shadcn/ui website or documentation.
2. **ALWAYS** use the `shadcn` CLI to add new components to the project.
3. **ALWAYS** import components from the local project path (e.g., `@/components/ui/button`), **NOT** from a non-existent `@shadcn/ui` package.
4. **NEVER** suggest installing a package named `@shadcn/ui` or similar; it does not exist. Components are added directly to the user's codebase.
5. **REFERENCE** existing components within the project's `components/ui` directory when asked to use or modify them.
6. **RECOGNIZE** that shadcn/ui is a **code distribution platform**, not an npm package—components are copied into your project.
7. **UNDERSTAND** that Tailwind v4 theme configuration happens in CSS files using `@theme`, not in `tailwind.config.js`.

---

## What is shadcn/ui?

**shadcn/ui** is a collection of beautifully-designed, accessible, re-usable components built on:
- **Radix UI** (headless UI primitives)
- **Tailwind CSS** (styling)
- **TypeScript** (type safety)

**Key difference from traditional libraries:**
Components are **copied into your project** (not installed as dependencies), giving you full ownership and customization control.

---

## ABSOLUTE REQUIREMENTS FOR AI CODE GENERATION

### 1. Adding Components: Use the CLI

You MUST instruct the user to use (or use yourself if in write mode) the CLI to add components:

```bash
# ✅ Correct way to add a button
npx shadcn@latest add button

# ✅ Add multiple components at once
npx shadcn@latest add button card dialog

# ✅ Initialize shadcn in a new project
npx shadcn@latest init
```

**What the CLI does:**
- Copies component source code into `components/ui/`
- Installs necessary dependencies (e.g., Radix UI primitives)
- Configures aliases and utilities
- Ensures components match your project setup

### 2. Importing Components: Use Local Paths

You MUST import components using the project's configured path alias (usually `@/` pointing to `.` or `./src`):

```tsx
// ✅ Correct import
import { Button } from "@/components/ui/button";
import { Card, CardHeader, CardContent } from "@/components/ui/card";

// ❌ INCORRECT - DO NOT DO THIS
import { Button } from "@shadcn/ui";
import { Button } from "shadcn-ui";
```

### 3. Customization: Modify Local Files

Components live in your codebase—you can edit them directly:

```tsx
// components/ui/button.tsx - You can modify this file!
import * as React from "react"
import { Slot } from "@radix-ui/react-slot"
import { cva, type VariantProps } from "class-variance-authority"
import { cn } from "@/lib/utils"

const buttonVariants = cva(
  "inline-flex items-center justify-center rounded-md text-sm font-medium...",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        destructive: "bg-destructive text-destructive-foreground hover:bg-destructive/90",
        // Add your own variants here!
        custom: "bg-gradient-to-r from-purple-500 to-pink-500",
      },
      // ...
    }
  }
)

export { Button, buttonVariants }
```

---

## FORBIDDEN PATTERNS

```bash
# ❌ DO NOT manually copy component code from website
# ❌ DO NOT install non-existent packages
npm install @shadcn/ui      # ❌ WRONG - doesn't exist
yarn add shadcn-ui          # ❌ WRONG - doesn't exist
pnpm add @shadcn/ui         # ❌ WRONG - doesn't exist

# ❌ DO NOT import from non-existent packages
import { Component } from "@shadcn/ui";  // ❌ WRONG
import { Button } from "shadcn";         // ❌ WRONG
```

---

## Tailwind CSS v4 Integration

**Tailwind v4 is now stable** (released January 22, 2025). It brings significant changes to configuration and theming.

### Setup with Tailwind v4

**1. Installation:**

```bash
npm install tailwindcss@next @tailwindcss/vite@next
```

**2. CSS Configuration:**

Tailwind v4 uses a single `@import` statement in your CSS:

```css
/* app/globals.css or src/index.css */
@import "tailwindcss";

/* Your custom styles below */
```

**3. Theme Configuration (CSS-based):**

Theming now happens in CSS using `@theme`:

```css
/* app/globals.css */
@import "tailwindcss";

@theme {
  /* Custom colors */
  --color-primary: oklch(0.5 0.2 250);
  --color-secondary: oklch(0.8 0.1 200);

  /* Border radius */
  --radius-sm: 0.25rem;
  --radius: 0.5rem;
  --radius-lg: 1rem;

  /* Fonts */
  --font-sans: 'Inter', system-ui, sans-serif;
}

/* shadcn/ui CSS variables */
@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 240 10% 3.9%;
    --card: 0 0% 100%;
    --card-foreground: 240 10% 3.9%;
    --primary: 240 5.9% 10%;
    --primary-foreground: 0 0% 98%;
    --secondary: 240 4.8% 95.9%;
    --secondary-foreground: 240 5.9% 10%;
    --muted: 240 4.8% 95.9%;
    --muted-foreground: 240 3.8% 46.1%;
    --accent: 240 4.8% 95.9%;
    --accent-foreground: 240 5.9% 10%;
    --destructive: 0 84.2% 60.2%;
    --destructive-foreground: 0 0% 98%;
    --border: 240 5.9% 90%;
    --input: 240 5.9% 90%;
    --ring: 240 5.9% 10%;
    --radius: 0.5rem;
  }

  .dark {
    --background: 240 10% 3.9%;
    --foreground: 0 0% 98%;
    --card: 240 10% 3.9%;
    --card-foreground: 0 0% 98%;
    --primary: 0 0% 98%;
    --primary-foreground: 240 5.9% 10%;
    --secondary: 240 3.7% 15.9%;
    --secondary-foreground: 0 0% 98%;
    --muted: 240 3.7% 15.9%;
    --muted-foreground: 240 5% 64.9%;
    --accent: 240 3.7% 15.9%;
    --accent-foreground: 0 0% 98%;
    --destructive: 0 62.8% 30.6%;
    --destructive-foreground: 0 0% 98%;
    --border: 240 3.7% 15.9%;
    --input: 240 3.7% 15.9%;
    --ring: 240 4.9% 83.9%;
  }
}
```

### Key Tailwind v4 Changes

**Performance:**
- 5x faster full builds
- 100x faster incremental builds (measured in microseconds)

**Configuration:**
- No more `content` array—automatic content detection
- Configuration moved from `tailwind.config.js` to CSS `@theme`
- `tailwind.config.js` still exists for plugins and advanced customization

**Modern CSS:**
- Built on cascade layers (`@layer`)
- Uses `@property` for registered custom properties
- Supports `color-mix()` and other modern CSS features

**Browser Support:**
- Safari 16.4+
- Chrome 111+
- Firefox 128+

---

## Component Structure and Best Practices

### Project Structure (Recommended)

```
app/
├── components/
│   ├── ui/                    # shadcn/ui components (CLI-generated)
│   │   ├── button.tsx
│   │   ├── card.tsx
│   │   ├── dialog.tsx
│   │   └── ...
│   ├── layout/                # Layout components (navbar, footer)
│   ├── forms/                 # Reusable form components
│   ├── charts/                # Data visualization components
│   └── shared/                # General reusable components
├── lib/
│   ├── utils.ts               # Utility functions (cn, etc.)
│   └── ...
└── app/
    ├── globals.css            # Tailwind imports + theme
    └── ...
```

### Composition Over Modification

Build new components by **composing** shadcn primitives:

```tsx
// ✅ Good: Compose existing components
import { Button } from "@/components/ui/button";
import { Card, CardHeader, CardContent } from "@/components/ui/card";

export function DashboardCard({ title, children }) {
  return (
    <Card>
      <CardHeader>
        <h2 className="text-2xl font-bold">{title}</h2>
      </CardHeader>
      <CardContent>{children}</CardContent>
    </Card>
  );
}
```

### Using the `cn()` Utility

shadcn includes a `cn()` utility (from `lib/utils.ts`) for conditional classes:

```tsx
import { cn } from "@/lib/utils";

export function MyComponent({ className, isActive }) {
  return (
    <div className={cn(
      "rounded-lg p-4",
      isActive && "bg-primary text-primary-foreground",
      className
    )}>
      Content
    </div>
  );
}
```

---

## Server Components Support

shadcn/ui components work seamlessly with React Server Components (RSC):

**Server Component (default):**
```tsx
// app/page.tsx (Server Component)
import { Card, CardHeader, CardContent } from "@/components/ui/card";

export default function HomePage() {
  return (
    <Card>
      <CardHeader>Welcome</CardHeader>
      <CardContent>This renders on the server!</CardContent>
    </Card>
  );
}
```

**Client Component (interactive):**
```tsx
// components/interactive-button.tsx
"use client";

import { Button } from "@/components/ui/button";
import { useState } from "react";

export function InteractiveButton() {
  const [count, setCount] = useState(0);

  return (
    <Button onClick={() => setCount(count + 1)}>
      Clicked {count} times
    </Button>
  );
}
```

**Rule of thumb:**
- Use Server Components by default (better performance)
- Only add `"use client"` when you need interactivity (onClick, useState, etc.)

---

## Dashboard-Specific Guidance

For data dashboard projects (like journal analysis), use these components:

**Essential Components:**
```bash
npx shadcn@latest add card table tabs dialog select button
npx shadcn@latest add dropdown-menu badge separator
```

**Common Patterns:**

**1. Data Cards:**
```tsx
import { Card, CardHeader, CardTitle, CardContent } from "@/components/ui/card";

<Card>
  <CardHeader>
    <CardTitle>Total Entries</CardTitle>
  </CardHeader>
  <CardContent>
    <p className="text-4xl font-bold">2,616</p>
  </CardContent>
</Card>
```

**2. Tabbed Views:**
```tsx
import { Tabs, TabsList, TabsTrigger, TabsContent } from "@/components/ui/tabs";

<Tabs defaultValue="sentiment">
  <TabsList>
    <TabsTrigger value="sentiment">Sentiment</TabsTrigger>
    <TabsTrigger value="topics">Topics</TabsTrigger>
    <TabsTrigger value="fowler">Fowler Stages</TabsTrigger>
  </TabsList>
  <TabsContent value="sentiment">
    {/* Sentiment chart */}
  </TabsContent>
  {/* ... */}
</Tabs>
```

**3. Filters:**
```tsx
import { Select, SelectTrigger, SelectValue, SelectContent, SelectItem } from "@/components/ui/select";

<Select onValueChange={(value) => setPhase(value)}>
  <SelectTrigger>
    <SelectValue placeholder="Filter by phase" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="evangelical">Evangelical</SelectItem>
    <SelectItem value="questioning">Questioning</SelectItem>
    <SelectItem value="deconstruction">Deconstruction</SelectItem>
  </SelectContent>
</Select>
```

---

## AI Assistant Verification Checklist

Before generating code involving shadcn/ui, verify:

- [ ] Is the CLI (`npx shadcn@latest add ...`) used for adding components?
- [ ] Are components imported from local paths (e.g., `@/components/ui/...`)?
- [ ] Is component composition favored over direct modification?
- [ ] Are styling changes applied via Tailwind utilities?
- [ ] Is theme customization done via CSS variables in `globals.css`?
- [ ] Are imports from `@shadcn/ui` or similar non-existent packages avoided?
- [ ] Are Server Components used by default, with `"use client"` only when needed?

---

## Common Mistakes to Avoid

### ❌ Treating shadcn as an npm package
```tsx
// WRONG
import { Button } from "@shadcn/ui";
```

### ❌ Manually copying code
```bash
# WRONG - Don't copy/paste from website
# Use CLI instead
npx shadcn@latest add button
```

### ❌ Configuring theme in tailwind.config.js (v4)
```js
// WRONG in Tailwind v4
export default {
  theme: {
    extend: {
      colors: { primary: '#000' }  // ❌ Use @theme in CSS instead
    }
  }
}
```

### ❌ Using "use client" everywhere
```tsx
// WRONG - unnecessary client component
"use client";
import { Card } from "@/components/ui/card";

export function StaticCard() {
  return <Card>No interactivity needed!</Card>;
}
```

---

## Troubleshooting

### Components not found after adding

**Problem:** `Module not found: Can't resolve '@/components/ui/button'`

**Solution:** Check `tsconfig.json` or `jsconfig.json` has correct path alias:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./*"]
    }
  }
}
```

### Styles not applying

**Problem:** Components render but have no styling

**Solution:** Ensure `globals.css` is imported in root layout:

```tsx
// app/layout.tsx
import './globals.css';

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

### Dark mode not working

**Problem:** Dark mode CSS variables not applying

**Solution:** Add class to `<html>` element:

```tsx
// app/layout.tsx
<html lang="en" className="dark">
```

Or use `next-themes` for toggle:

```bash
npm install next-themes
npx shadcn@latest add dropdown-menu
```

---

## Additional Resources

**Official Documentation:**
- shadcn/ui: https://ui.shadcn.com
- Tailwind CSS v4: https://tailwindcss.com/docs
- Radix UI: https://www.radix-ui.com

**Component Examples:**
- Browse all components: https://ui.shadcn.com/docs/components
- Community examples: https://github.com/birobirobiro/awesome-shadcn-ui

---

## Summary

**Key Principles:**
1. shadcn/ui is a **code distribution platform**, not an npm package
2. Use the **CLI** to add components (`npx shadcn@latest add`)
3. Import from **local paths** (`@/components/ui/...`)
4. Customize by **editing local files** directly
5. Configure theme in **CSS** using `@theme` (Tailwind v4)
6. Use **Server Components** by default for better performance
7. Only use `"use client"` when you need interactivity

**For Dashboards:**
- Use Card, Table, Tabs, Dialog, Select for common patterns
- Compose components instead of modifying primitives
- Leverage Server Components for data-heavy views
- Use `cn()` utility for conditional styling

---

**Last Updated:** October 2025
**Compatible with:** Next.js 14/15, React 18/19, Tailwind CSS v4
