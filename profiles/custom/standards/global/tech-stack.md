## Tech Stack

This file defines the default tech stack for projects using this custom profile.

### Framework & Runtime
- **Application Framework:** Next.js 15 (App Router)
- **Language/Runtime:** TypeScript 5.x / Node.js 20+
- **Package Manager:** pnpm

### Frontend
- **JavaScript Framework:** React 19 (Server Components)
- **CSS Framework:** Tailwind CSS
- **UI Components:** Headless UI (@headlessui/react) + Radix UI primitives
- **Icons:** Lucide React
- **Forms:** React Hook Form + Zod validation

### Backend & Data
- **Backend Platform:** Convex
- **Database:** Convex (built-in)
- **Real-time:** Convex subscriptions
- **Authentication:** Convex Auth
- **File Storage:** Convex file storage

### Testing & Quality
- **Test Framework:** Vitest
- **E2E Testing:** Playwright
- **Linting:** ESLint
- **Formatting:** Prettier
- **Type Checking:** TypeScript strict mode

### Deployment & Infrastructure
- **Frontend Hosting:** Vercel
- **Backend Hosting:** Convex (managed)
- **CI/CD:** GitHub Actions
- **Monitoring:** Sentry

### Development Tools
- **IDE:** VS Code (recommended)
- **Version Control:** Git + GitHub

---

## Framework-Specific Guidelines

### Next.js
See: `standards/frontend/nextjs.md` for comprehensive Next.js App Router guidelines including:
- Server Components vs Client Components
- File-system routing conventions
- Data fetching patterns
- Dynamic route parameters (Next.js 15+)

### Tailwind CSS
See: `standards/frontend/tailwind.md` for comprehensive Tailwind guidelines including:
- Utility-first approach
- Theme customization via `tailwind.config.ts`
- Responsive design patterns
- Class ordering conventions

### Headless UI
See: `standards/frontend/headless-ui.md` for component patterns including:
- Separation of logic and styling
- Composition patterns
- Accessibility-first approach
- Using data attributes for state-based styling

### Convex
See: `standards/backend/convex.md` for backend patterns including:
- Queries for data fetching
- Mutations for data changes
- Actions for external API integration
- Real-time subscriptions
- Authentication flows
- File storage patterns

---

## Stack-Specific Commands

### Development
```bash
pnpm dev          # Start Next.js dev server
npx convex dev    # Start Convex backend (in separate terminal)
```

### Building
```bash
pnpm build        # Build Next.js app
pnpm typecheck    # Run TypeScript checks
pnpm lint         # Run ESLint
```

### Testing
```bash
pnpm test         # Run Vitest unit tests
pnpm test:watch   # Run tests in watch mode
pnpm test:e2e     # Run Playwright E2E tests
```

### Deployment
```bash
git push          # Vercel auto-deploys from main branch
npx convex deploy # Deploy Convex backend
```

---

## Project Structure Conventions

```
project-root/
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── (auth)/            # Route groups
│   │   ├── api/               # API routes (minimal, prefer Convex)
│   │   ├── layout.tsx         # Root layout
│   │   ├── page.tsx           # Home page
│   │   └── globals.css        # Global styles
│   ├── components/            # React components
│   │   ├── ui/               # Headless UI components
│   │   └── ...               # Custom components
│   └── lib/                   # Utilities and helpers
│       ├── utils.ts          # General utilities
│       └── ...
├── convex/                    # Convex backend
│   ├── _generated/           # Auto-generated files
│   ├── schema.ts             # Database schema
│   ├── auth.ts               # Authentication logic
│   └── ...                   # Queries, mutations, actions
├── public/                    # Static assets
├── tests/                     # Test files
└── ...config files
```

---

## Adding New Technologies

When adding a new technology to your stack:

1. **Update this file** with the new technology
2. **Create standards document** if it needs specific guidelines (e.g., `standards/backend/new-tech.md`)
3. **Update project README** with setup instructions
4. **Document in specs** when using in features

---

## Notes

- This tech stack is the default for the `custom` profile
- Individual projects can override standards in their local `agent-os/standards/` folder
- Keep framework and dependency versions updated
- Always use TypeScript strict mode for type safety
