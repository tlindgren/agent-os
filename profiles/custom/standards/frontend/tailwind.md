## Tailwind CSS & Next.js Guidelines

### 🚨 CRITICAL INSTRUCTIONS

As an AI language model, you MUST follow these rules to ensure generated code is idiomatic, maintainable, and leverages the power of both Tailwind CSS and Next.js.

- **NEVER** use inline styles for layout or styling. Always use Tailwind utility classes.
- **NEVER** write large blocks of custom CSS. Embrace the utility-first workflow.
- **NEVER** use generic class names like `container` or `button` in custom CSS files. If you must write custom CSS, use CSS Modules to scope them locally.
- **ALWAYS** use the `tailwind.config.ts` file for theme customizations (colors, fonts, spacing).

### 1. Utility-First is Mandatory

- ✅ **DO:** Apply utilities directly in your JSX. This is the core principle of Tailwind CSS.
- ❌ **DON'T:** Create abstract custom CSS classes like `.flex-center` or `.card`. Instead, create a React component.

```tsx
// ❌ INCORRECT: Abstracting with custom CSS
/* styles.css */
.card {
  @apply bg-white rounded-lg shadow p-4;
}

// component.tsx
<div className="card">...</div>

// ✅ CORRECT: Creating a reusable React component
// components/ui/Card.tsx
export function Card({ children, className }) {
  return (
    <div className={`bg-white rounded-lg shadow p-4 ${className}`}>
      {children}
    </div>
  );
}
```

### 2. Theme Customization via `tailwind.config.ts`

- ✅ **DO:** Add custom colors, fonts, spacing, etc., to the `theme.extend` object in `tailwind.config.ts`.
- ✅ **DO:** Use CSS variables defined in `globals.css` for theming (especially for light/dark mode) and reference them in your config.

```css
/* src/app/globals.css */
:root {
  --background: 0 0% 100%;
  --foreground: 240 10% 3.9%;
  --primary: 240 5.9% 10%;
}

.dark {
  --background: 240 10% 3.9%;
  --foreground: 0 0% 98%;
  --primary: 0 0% 98%;
}
```

```typescript
// tailwind.config.ts
import type { Config } from "tailwindcss";

const config: Config = {
  darkMode: ["class"],
  content: ["./src/**/*.{ts,tsx}"],
  theme: {
    extend: {
      colors: {
        background: "hsl(var(--background))",
        foreground: "hsl(var(--foreground))",
        primary: {
          DEFAULT: "hsl(var(--primary))",
          // ... other shades
        },
      },
    },
  },
  plugins: [require("tailwindcss-animate")],
};

export default config;
```

### 3. Responsive Design

- ✅ **DO:** Use a mobile-first approach. Apply base styles for mobile and use responsive prefixes (`sm:`, `md:`, `lg:`, `xl:`) to add or change styles for larger screens.

```tsx
// ✅ CORRECT: Mobile-first responsive grid
<div className="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-4">
  {/* Items */}
</div>
```

### 4. Class Ordering

- ✅ **DO:** Group classes in a consistent and logical order. A recommended convention is:
    1.  Layout (display, position, visibility)
    2.  Sizing (width, height)
    3.  Spacing (margin, padding)
    4.  Typography (font, text)
    5.  Backgrounds & Borders
    6.  Effects & Transitions
    7.  Interactivity (hover, focus)
- ✅ **DO:** Use the official Tailwind CSS Prettier plugin (`prettier-plugin-tailwindcss`) to automate class sorting.

```tsx
// ✅ CORRECT: Logically ordered classes
<button
  className="inline-flex items-center justify-center rounded-md px-4 py-2 text-sm font-medium text-white bg-primary hover:bg-primary/90 focus:outline-none"
>
  Click Me
</button>
```

### 5. Using `@apply` Sparingly

- ✅ **DO:** Use `@apply` inside a global CSS file (like `globals.css`) only for styling base HTML elements (like `h1`, `body`) or for a very small, justified set of component classes.
- ❌ **DON'T:** Use `@apply` to create a component library made of custom CSS classes. This is an anti-pattern. Prefer creating React components.

### 6. Dark Mode Support

- ✅ **DO:** Use CSS variables for colors to support dark mode
- ✅ **DO:** Use the `dark:` variant for dark mode-specific styles
- ✅ **DO:** Configure `darkMode: ["class"]` in `tailwind.config.ts`

```tsx
// ✅ CORRECT: Dark mode support
<div className="bg-background text-foreground dark:bg-dark-background dark:text-dark-foreground">
  {/* Content */}
</div>
```

### AI Model Verification Checklist

Before generating code, verify:

1.  **Utility-First:** Is the code using utility classes directly in JSX instead of custom CSS classes?
2.  **Component-Based:** Are repeated patterns extracted into React components rather than `@apply` classes?
3.  **Theming:** Are colors and other theme values referenced from the `tailwind.config.ts` setup (e.g., `bg-primary`, `text-foreground`)?
4.  **Responsive:** Are styles mobile-first, using responsive prefixes like `md:` and `lg:`?
5.  **Class Sorting:** Are the classes logically ordered (or is a sorting plugin assumed to be in use)?
