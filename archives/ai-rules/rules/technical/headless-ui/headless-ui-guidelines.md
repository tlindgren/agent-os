---
description: "Guidelines for building accessible and reusable components using Headless UI patterns with React and Tailwind CSS."
globs: ["src/components/**/*.tsx"]
---

# Headless UI AI Guidelines

## 🚨 CRITICAL INSTRUCTIONS FOR AI LANGUAGE MODELS 🚨

As an AI language model, you MUST understand that Headless UI is a pattern, not just a single library. It's about separating component logic and state from the visual presentation. The primary goal is maximum flexibility and accessibility.

- **NEVER** assume a component's visual appearance. Headless components are unstyled by default.
- **NEVER** tightly couple logic to specific styles. The styling should be fully composable.
- **ALWAYS** prioritize accessibility. Use the props and patterns provided by the headless library to ensure components are keyboard-navigable and screen-reader friendly.
- **ALWAYS** use the headless library's components as the foundation (e.g., `Menu`, `Dialog`, `Listbox` from `@headlessui/react` or primitives from Radix UI).

## 1. The Core Principle: Separation of Concerns

- ✅ **DO:** Use headless components to manage complex state and accessibility (e.g., open/closed state, ARIA attributes, focus management).
- ✅ **DO:** Use Tailwind CSS utility classes to apply the visual presentation to the unstyled headless components.

```tsx
// ✅ CORRECT: Combining Headless UI (Radix) with Tailwind CSS
import * as DropdownMenu from '@radix-ui/react-dropdown-menu';

export function ProfileMenu() {
  return (
    <DropdownMenu.Root>
      <DropdownMenu.Trigger className="rounded-full p-2 hover:bg-gray-100">
        <UserIcon />
      </DropdownMenu.Trigger>

      <DropdownMenu.Portal>
        <DropdownMenu.Content 
          className="mt-2 w-56 rounded-md bg-white p-1 shadow-lg ring-1 ring-black ring-opacity-5"
        >
          <DropdownMenu.Item 
            className="block px-4 py-2 text-sm text-gray-700 rounded-md hover:bg-gray-100 focus:outline-none focus:bg-gray-100"
          >
            Your Profile
          </DropdownMenu.Item>
          <DropdownMenu.Item 
            className="block px-4 py-2 text-sm text-gray-700 rounded-md hover:bg-gray-100 focus:outline-none focus:bg-gray-100"
          >
            Sign Out
          </DropdownMenu.Item>
        </DropdownMenu.Content>
      </DropdownMenu.Portal>
    </DropdownMenu.Root>
  );
}
```

## 2. Composition is Key

- ✅ **DO:** Build complex components by composing smaller, single-purpose headless primitives.
- ✅ **DO:** Expose props that allow developers to pass in custom styles or even different sub-components.
- ❌ **DON'T:** Create monolithic components with intertwined logic and styling.

## 3. State Management

- ✅ **DO:** Let the headless library manage its internal state (like the open state of a dropdown).
- ✅ **DO:** If you need to control the state from a parent component (e.g., to programmatically open a dialog), use the controlled props provided by the library.

```tsx
// ✅ CORRECT: Controlled Dialog component
import { Dialog } from '@headlessui/react';
import { useState } from 'react';

export function MyControlledDialog() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <>
      <button onClick={() => setIsOpen(true)}>Open Dialog</button>
      <Dialog open={isOpen} onClose={() => setIsOpen(false)} className="relative z-50">
        {/* Dialog panel and content... */}
      </Dialog>
    </>
  );
}
```

## 4. Accessibility First

- ✅ **DO:** Trust that the headless library handles most ARIA attributes, focus trapping, and keyboard navigation out of the box.
- ✅ **DO:** Use semantic HTML elements (`<button>`, `<input>`) within your headless components where appropriate.
- ✅ **DO:** Ensure all interactive elements are clearly focusable. Use Tailwind's `focus:` and `focus-visible:` variants to provide clear focus indicators.

```tsx
// ✅ CORRECT: Applying focus styles
<DropdownMenu.Item 
  className="...
             focus:outline-none focus:ring-2 focus:ring-indigo-500"
>
  Menu Item
</DropdownMenu.Item>
```

## 5. Styling with Data Attributes

- ✅ **DO:** Leverage data attributes that headless libraries often provide for styling different states.
- ✅ **DO:** Use Tailwind's arbitrary variants to style based on these data attributes.

```tsx
// ✅ CORRECT: Styling based on `data-state` from Radix UI
<DropdownMenu.Content
  className="...
             data-[side=top]:animate-slide-down
             data-[side=bottom]:animate-slide-up"
>
  {/* ... */}
</DropdownMenu.Content>
```

This pattern is common with Radix UI and is used extensively by `shadcn/ui`.

## AI Model Verification Checklist

Before generating code, verify:

1.  **Separation:** Is the component logic (from the headless library) separate from the styling (from Tailwind)?
2.  **Accessibility:** Are the foundational elements from a reputable headless library being used, ensuring accessibility is built-in?
3.  **Composition:** Is the component built by composing primitives (`Trigger`, `Content`, `Item`) rather than as a single block?
4.  **Styling:** Are styles applied via Tailwind utility classes? Are state-based styles (e.g., for open/closed, active/inactive) handled correctly, possibly using data attributes?
