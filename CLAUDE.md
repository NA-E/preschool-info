# CLAUDE.md — Preschool Info Project

This document provides guidance for AI assistants working in this repository. It covers the intended project structure, development workflows, and conventions to follow.

---

## Project Overview

**preschool-info** is an informational web application for a preschool — designed to surface key information for parents, guardians, and prospective families: schedules, programs, enrollment, staff, events, and contact details.

> **Status**: This repository is freshly initialized. No source code has been committed yet. This document establishes the intended architecture and conventions.

---

## Intended Tech Stack

| Layer | Technology |
|---|---|
| Framework | Next.js (App Router) |
| Language | TypeScript |
| Styling | Tailwind CSS |
| Package Manager | npm |
| Testing | Jest + React Testing Library |
| Linting | ESLint + Prettier |
| Deployment | Vercel (assumed) |

If the actual stack differs from the above once the project is initialized, update this section to reflect reality.

---

## Repository Structure (Intended)

```
preschool-info/
├── CLAUDE.md                   # This file
├── README.md                   # Human-facing project overview
├── package.json
├── tsconfig.json
├── next.config.ts
├── tailwind.config.ts
├── .eslintrc.json
├── .prettierrc
├── .gitignore
├── public/                     # Static assets (images, icons, fonts)
│   └── images/
├── src/
│   ├── app/                    # Next.js App Router pages and layouts
│   │   ├── layout.tsx          # Root layout (metadata, fonts, global nav)
│   │   ├── page.tsx            # Home page
│   │   ├── about/
│   │   ├── programs/
│   │   ├── enrollment/
│   │   ├── events/
│   │   ├── contact/
│   │   └── api/                # API routes (if any backend is needed)
│   ├── components/             # Reusable UI components
│   │   ├── ui/                 # Primitive components (Button, Card, etc.)
│   │   ├── layout/             # Header, Footer, Navigation
│   │   └── sections/           # Page-level content sections
│   ├── lib/                    # Utility functions and helpers
│   ├── hooks/                  # Custom React hooks
│   ├── types/                  # TypeScript type definitions
│   └── data/                   # Static content / mock data (JSON or TS)
└── tests/                      # Test files mirroring src/ structure
    ├── components/
    └── lib/
```

---

## Development Workflow

### Initial Setup

```bash
npm install
npm run dev        # Start dev server at http://localhost:3000
```

### Common Scripts

```bash
npm run dev        # Development server
npm run build      # Production build
npm run start      # Start production server
npm run lint       # Run ESLint
npm run format     # Run Prettier
npm run test       # Run Jest test suite
npm run test:watch # Run tests in watch mode
npm run type-check # Run tsc without emitting (type checking only)
```

### Before Committing

Always run these checks before committing:

```bash
npm run type-check
npm run lint
npm run test
```

---

## Git Conventions

### Branch Naming

- Feature branches: `feature/<short-description>`
- Bug fixes: `fix/<short-description>`
- AI/automated branches: `claude/<task-description>-<session-id>`

### Commit Messages

Use the [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>(<scope>): <short summary>

[optional body]
```

Common types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

Examples:
- `feat(enrollment): add online enrollment form`
- `fix(nav): correct mobile menu toggle behavior`
- `docs: update CLAUDE.md with stack details`
- `chore: add ESLint and Prettier config`

### Pull Requests

- Keep PRs focused on a single concern
- Include a description of what changed and why
- Reference relevant issues with `Closes #<issue-number>` when applicable

---

## Code Conventions

### TypeScript

- Strict mode enabled (`"strict": true` in tsconfig)
- Prefer `interface` over `type` for object shapes; use `type` for unions/intersections
- Avoid `any`; use `unknown` and narrow types instead
- Export types alongside their related modules (not in a separate barrel `types.ts` unless shared widely)

### React / Next.js

- Use the **App Router** (not Pages Router)
- Prefer **Server Components** by default; add `"use client"` only when interactivity or browser APIs are needed
- Use `next/image` for all images (automatic optimization)
- Use `next/link` for internal navigation
- Co-locate component styles, tests, and types near the component file

### Components

- One component per file
- File names and component names use **PascalCase** (e.g., `EnrollmentForm.tsx`)
- Props interfaces are named `<ComponentName>Props`
- Keep components small and focused; extract sub-components early

### Styling

- Use **Tailwind CSS utility classes** directly in JSX
- Avoid inline `style` props unless truly dynamic
- For complex, repeated patterns extract a component rather than a long class string
- Responsive design: mobile-first (`sm:`, `md:`, `lg:` prefixes)

### File Naming

| File type | Convention |
|---|---|
| Components | `PascalCase.tsx` |
| Pages (App Router) | `page.tsx`, `layout.tsx` |
| Utilities / helpers | `camelCase.ts` |
| Test files | `<filename>.test.tsx` or `<filename>.test.ts` |
| Type files | `camelCase.types.ts` or co-located |

---

## Content & Data

Static content (preschool name, address, hours, program descriptions) lives in `src/data/` as TypeScript constants or JSON files. This keeps content edits separate from component logic and makes future CMS integration straightforward.

Example:

```ts
// src/data/programs.ts
export const programs = [
  {
    id: "toddler",
    name: "Toddler Program",
    ageRange: "18 months – 3 years",
    schedule: "Mon–Fri, 9am–12pm",
    description: "...",
  },
  // ...
];
```

---

## Testing

- **Unit tests**: Utility functions and hooks in `tests/lib/`
- **Component tests**: Use React Testing Library; test behavior, not implementation details
- **Snapshot tests**: Use sparingly (prefer explicit assertions)
- Aim for coverage on all non-trivial logic; avoid testing framework internals

---

## Accessibility

- All interactive elements must be keyboard accessible
- Images require meaningful `alt` text (or `alt=""` for decorative images)
- Use semantic HTML elements (`<nav>`, `<main>`, `<section>`, `<article>`, `<header>`, `<footer>`)
- Maintain sufficient color contrast (WCAG AA minimum)
- Run `npm run lint` — the ESLint `jsx-a11y` plugin flags common a11y issues

---

## Environment Variables

Store secrets and environment-specific values in `.env.local` (never committed). Document required variables in `.env.example` (committed, with placeholder values).

```bash
# .env.example
NEXT_PUBLIC_SITE_URL=http://localhost:3000
CONTACT_FORM_EMAIL=
```

---

## AI Assistant Notes

- **Always read a file before editing it.** Do not assume file contents.
- **Do not add features beyond what is requested.** Keep changes minimal and focused.
- **Update this CLAUDE.md** whenever the tech stack, structure, or conventions change materially.
- **Prefer editing existing files** over creating new ones.
- **Run lint and type-check** before committing; fix any errors introduced by your changes.
- When the actual project is scaffolded (e.g., via `npx create-next-app`), reconcile this document with the real generated structure and remove any sections that no longer apply.
