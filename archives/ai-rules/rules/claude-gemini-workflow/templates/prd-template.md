# Feature: [Feature Name]

**Status:** 📋 Spec (not yet built)
**Author:** Claude
**Date:** YYYY-MM-DD
**Target Builder:** Gemini

---

## Purpose

[1-2 sentences: Why are we building this? What problem does it solve?]

## Scope

**In Scope:**
- [ ] Item 1
- [ ] Item 2

**Out of Scope:**
- Item 1 (we'll tackle this later)
- Item 2 (not needed)

---

## User Story

As a [user type], I want to [action] so that [benefit].

**Example:**
As a journal user, I want to filter entries by topic so that I can focus on specific themes.

---

## Component/Module Structure

```
[Directory structure showing files to create]
```

**Main Components:**
1. **ComponentName** (`path/to/file.tsx`)
   - Purpose: [what it does]
   - Responsibilities: [what it handles]

2. **ModuleName** (`path/to/file.ts`)
   - Purpose: [what it does]
   - Exports: [what functions/classes]

---

## Data Flow

```
[Diagram or description of how data moves through the system]

Example:
User clicks button →
  Component calls API →
  API fetches data →
  Data returned to component →
  Component renders results
```

---

## API/Interface Design

### Functions/Methods

```typescript
// Example function signature
function featureName(param: Type): ReturnType {
  // Implementation by Gemini
}
```

### Props (if React component)

```typescript
interface FeatureProps {
  data: DataType;
  onAction: (result: ResultType) => void;
  optional?: string;
}
```

---

## File Locations

**Files to Create:**
1. `path/to/component.tsx` - Main component
2. `path/to/utils.ts` - Helper functions
3. `path/to/types.ts` - TypeScript types

**Files to Modify:**
1. `existing/file.ts` - Add new export
2. `package.json` - Add dependencies

---

## Dependencies

**Required:**
- `library-name@version` - Why we need it

**Optional:**
- `library-name@version` - Nice to have for [reason]

---

## Acceptance Criteria

**Must Have:**
- [ ] Criterion 1 (describe expected behavior)
- [ ] Criterion 2 (describe expected behavior)
- [ ] All TypeScript types defined
- [ ] No console errors
- [ ] Responsive (works on mobile)

**Nice to Have:**
- [ ] Criterion 3 (enhancement)
- [ ] Criterion 4 (polish)

---

## Test Cases

1. **Test Name**
   - **Given:** [initial state]
   - **When:** [action]
   - **Then:** [expected result]

Example:
- **Given:** User is on timeline page
- **When:** User clicks a data point
- **Then:** Entry list filters to that date

---

## Example Usage

```typescript
// Example of how to use the feature
import { FeatureName } from './feature';

<FeatureName
  data={exampleData}
  onAction={(result) => console.log(result)}
/>
```

---

## Edge Cases to Handle

1. **Empty data:** [what should happen]
2. **Network error:** [how to handle]
3. **Invalid input:** [validation approach]

---

## Performance Considerations

- Expected data volume: [small/medium/large]
- Render frequency: [once/on-demand/continuous]
- Optimization notes: [any specific concerns]

---

## Security Considerations

- [ ] No sensitive data in logs
- [ ] Input sanitization
- [ ] Authentication required? (yes/no)

---

## Accessibility

- [ ] Keyboard navigation works
- [ ] Screen reader friendly
- [ ] Color contrast meets WCAG standards

---

## Open Questions

1. [Question for user to clarify before building]
2. [Another question]

---

## Handoff Checklist for Gemini

Before you start building:
- [ ] Read this spec completely
- [ ] Understand the data flow
- [ ] Check that all dependencies are available
- [ ] Ask user about any Open Questions

While building:
- [ ] Follow file structure exactly
- [ ] Meet all acceptance criteria
- [ ] Write clean, typed code
- [ ] Test all cases listed

When complete:
- [ ] Update `.ai/build-log.md`
- [ ] Note any deviations from spec
- [ ] Flag any issues encountered
- [ ] Mark as ready for review

---

## Notes

[Any additional context, references, or explanations]
