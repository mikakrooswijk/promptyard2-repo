---
applyTo: "**/*.ts"
---

# TypeScript Conventions

## Types
- Prefer `interface` over `type` for object shapes; use `type` for unions, intersections, and aliases
- Never use `any` — use `unknown` and narrow, or model the type properly
- Avoid non-null assertion (`!`) — handle nullability explicitly
- Use `readonly` for properties that should not be reassigned after construction
- Prefer `const` assertions (`as const`) over manual enum declarations for string literal sets

## Functions
- Prefer arrow functions for callbacks and class methods that need lexical `this`
- Annotate return types on exported functions; let inference work for private/internal ones
- Keep functions focused — if a function needs a comment to explain what it does, consider splitting it

## Imports
- Use named imports; avoid `import * as`
- Group imports: external packages first, then internal (`@/`, relative), separated by a blank line
- Use `.js` extensions in ESM projects (even when the source file is `.ts`)

## Error Handling
- Never silently swallow errors with empty `catch` blocks
- Use typed error handling: check `instanceof Error` before accessing `.message`
- Only catch errors you can meaningfully handle; let the rest propagate

## Async
- Prefer `async`/`await` over raw Promise chains
- Always `await` or `return` a Promise — never fire-and-forget unless intentional (and comment it)
- Use `Promise.all()` for independent concurrent operations instead of sequential `await`

## Naming
- `PascalCase` for classes, interfaces, types, enums
- `camelCase` for variables, functions, methods, parameters
- `SCREAMING_SNAKE_CASE` for module-level constants that are truly fixed values
- Prefix boolean variables/props with `is`, `has`, `can`, `should`

## General
- Avoid magic numbers — extract to named constants with clear names
- Don't export things that don't need to be exported
- Prefer early returns to deeply nested `if/else` blocks
- Delete commented-out code instead of leaving it in
