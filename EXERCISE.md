# SCSS → Tailwind v4 Migration Exercise

## Context

You are migrating a component from the **Techem Design System** (`ui-experience-kit`) from SCSS Modules to Tailwind CSS v4.

**Stack:**
- React + TypeScript + Radix UI primitives
- `clsx()` for conditional class composition (never `cn()`)
- SCSS Modules → Tailwind v4 utility classes
- Tokens are CSS custom properties on `:root` — not SCSS variables
- Tailwind v4 is already wired: `@tailwindcss/postcss` in `postcss.config.mjs`

**Goal:** Identical visual output, zero SCSS remaining. Delete the `.module.scss` file and its import.

---

## Prompt — paste this into your AI tool, then append the component files

```
You are migrating a component from the Techem Design System (ui-experience-kit)
from SCSS Modules to Tailwind CSS v4.

Stack: React + TypeScript + Radix UI — clsx() for class composition (never cn())
— tokens are CSS custom properties on :root (no SCSS variables) — Tailwind v4
already wired, only migrate the classes.

Steps:
1. Show a mapping table: SCSS rule → Tailwind class (one row per rule)
2. Rewrite the .tsx with inline utility classes
3. Remove the SCSS import and all styles.* references
4. For conditional variants (size, type, state), use a const lookup object
5. For anything you cannot express as utilities, keep a minimal CSS block with a TODO

Rules:
- Use clsx() — already imported in every component
- Reference CSS var tokens as: bg-[--color-primary-500] or text-[--d3x-white]
- Use hover: focus: disabled: variants for interactive states
- v4 feature: use not-disabled: for :hover:not(:disabled) patterns
- Group classes: layout → sizing → color → typography → state
- Goal: zero SCSS, visually identical output

--- Component (.tsx) ---
[paste here]

--- SCSS Module (.module.scss) ---
[paste here]

--- Available tokens ---
--color-primary-500, --color-primary-600, --color-primary-100
--color-white, --d3x-white, --d3x-gray-700
--color-gray-100, --color-gray-200, --color-gray-700
--spacing-xs, --spacing-sm, --spacing-md, --spacing-lg
--radius-size-xs, --radius-size-sm, --radius-size-md, --radius-size-lg, --radius-size-full
--border-width-xs (alias: --border-1)
--elevation-shadow-sm, --elevation-shadow-md, --elevation-focus
--typography-font-weight-bold
```

---

## Group assignments

| Group | Component | Complexity | Key challenge |
|---|---|---|---|
| 1 | Avatar | Simple | CSS var tokens, size lookup object |
| 2 | Badge / Tag | Medium | Variant → color token mapping |
| 3 | Button | Complex | Many variants, `hover:not-disabled:` pattern |

---

## Iteration prompts — when the AI misses something

**Missing interactive state:**
```
The .primary:hover:not(:disabled) state is missing.
SCSS was: &:hover:not(:disabled) { background: var(--color-primary-600) }
Add it using the hover:not-disabled: Tailwind v4 variant.
```

**Wrong token reference:**
```
bg-white resolves to Tailwind's white (#fff) not our token.
Use bg-[--color-white] to reference the CSS custom property directly.
```

**Missed conditional class:**
```
The styles[size] prop is not handled.
Replace it with a const sizeClasses = { sm: '...', md: '...', lg: '...' }
lookup object and use sizeClasses[size] in clsx().
```

---

## Success criteria

```bash
# No SCSS references remain
grep -rn "styles\." src/YourComponent/
grep -rn ".module.scss" src/YourComponent/
# → zero results means done ✓

# TypeScript compiles clean
pnpm run type-check

# Count remaining files in the whole repo
find src -name "*.module.scss" | wc -l
```

---

## Claude Code specific (CLI)

```bash
# In the repo root
claude

# Ask Claude to read the files directly:
"Read Avatar.tsx and Avatar.module.scss and migrate the component
to Tailwind v4 using the context in EXERCISE.md"

# Or paste the prompt block above in the first message
```

## Copilot specific (VS Code)

```
1. Open both .tsx and .module.scss files in VS Code
2. Open Copilot Chat (Ctrl+Shift+I)
3. Attach both files with @file Avatar.tsx and @file Avatar.module.scss
4. Paste the prompt block above and send
```
