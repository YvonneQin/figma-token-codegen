---
name: figma-token-codegen
description: >
  Use this skill to generate a brand-specific coding skill from a user's Figma design tokens.
  The output is an installable .skill file containing token-map.css, components-reference.md,
  and coding-rules.md — ready to constrain future vibe coding sessions to the user's real
  design system. Triggers on: "generate a skill from my Figma tokens", "create a design system
  skill", "make a coding skill from my components", "package my tokens into a skill",
  "figma token codegen", "figma to code skill", or any request to turn Figma tokens/components
  into reusable AI coding rules. Also triggers when the user pastes a Figma token JSON and
  wants it turned into a skill. ALWAYS use this skill when the goal is producing a token-bound
  skill file — not directly generating UI code.
---

# Figma-Token-Codegen

This skill does one thing: take a user's Figma design tokens and produce an **installable
`.skill` file** that constrains future vibe coding sessions to the user's real design system.

It does **not** generate UI code itself. Its output is the rulebook that other sessions use
to generate correct UI.

## What the Output Skill Contains

```
[skill-name]/
├── SKILL.md              ← Generated: triggers on UI/vibe coding, enforces all rules
├── references/
│   ├── token-map.css           ← Generated: all tokens as CSS custom properties
│   ├── components-reference.md ← Generated: prop tables + HTML snippets per component
│   └── coding-rules.md         ← Generated: tag patterns, naming rules, constraints
└── [skill-name].skill ← Packaged output
```

## First Step: Load References

**Read these three template files immediately when this skill triggers:**

| File | Path | Contents |
|------|------|----------|
| Coding Rules Template | `references/coding-rules.md` | Tag patterns, naming rules, pre-gen checklist |
| Output Templates | `references/output-templates.md` | CSS template, component reference template, layout slots |
| Token Extraction | `references/token-extraction.md` | Script the user runs to export their Figma tokens |

These are **templates** — they define the structure of the files this skill generates.
Do not lazy-load. All three must be in context before Gate 0.

---

## Pipeline

```
GATE 0  Clarify intent
GATE 1  Token JSON received
GATE 2  Token map confirmed
GATE 3  Generate the three reference files
GATE 4  Generate the output SKILL.md
GATE 5  Self-review
GATE 6  Package and deliver .skill file
```

Each gate produces a stated output before the next begins. Never skip or merge gates.

---

## GATE 0 — Clarify

Ask the user:

1. **Skill name** — What should the generated skill be called? This becomes the folder name
   and `.skill` filename. Use kebab-case, e.g. `zenlayer-design-system`, `acme-dashboard-tokens`.
2. **Brand / product name** — The product or brand these tokens belong to. Used in the
   SKILL.md title and trigger description. (e.g. "Zenlayer", "Acme Dashboard")
3. **Target output format** — Will future vibe coding sessions produce HTML, React JSX, or both?
4. **Any extra rules?** — Company-specific conventions beyond what's in the token JSON?
   (e.g. "we always use BEM naming", "dark mode must be supported")

**Gate condition:** All answered. If pre-answered, confirm and proceed.

> ⚠️ Do not include the word "OpenClaw" in any generated file — comments, filenames, or headings.

---

## GATE 1 — Token JSON

Check whether a Figma token JSON is in the conversation.

- **Present** → proceed to Gate 2.
- **Missing** → give the user the extraction script from `references/token-extraction.md`.
  Wait for the JSON. Do not simulate or skip.

**Gate condition:** Token JSON received. No JSON = no output.

---

## GATE 2 — Parse and Confirm

Parse the JSON and show this confirmation block:

```
✅ Token map loaded:
- Colors:         12  (Brand/Primary, Neutral/Gray/200, Status/Error, …)
- Text styles:     8  (Heading/H1, Body/Regular, Caption/Small, …)
- Spacing:         6  (spacing-xs → spacing-2xl)
- Components:      5  (Button, StatusBadge, …)
- Component Sets:  3  (Button, Card, NavItem — with variants)
```

The JSON now includes `components` (standalone COMPONENT nodes) and `componentSets`
(COMPONENT_SET nodes with variant children). Use these as the **primary source** for the
Component Catalog — they contain the exact names, keys, descriptions, and variant strings
as published in Figma.

Then build a **Component Catalog** — a lookup table of every component with its props
and valid values. Show the table to the user:

| Component | Prop | Valid Values |
|-----------|------|--------------|
| Button | variant | Primary, Secondary, Ghost |
| Button | size | sm, md, lg |
| ProductNavigation | product | Cloud Networking, Bare Metal |

**Gate condition:** Confirmation shown, user hasn't flagged errors.

---

## GATE 3 — Generate Reference Files

Create three files based on the parsed token data and the templates in `references/`:

### 3a. `token-map.css`

Follow the template in `references/output-templates.md` (token-map.css section).
Convert every token from the JSON into a CSS custom property:

- Color paths: `Brand/Primary/500` → `--color-brand-primary-500`
- Typography: `Heading/H1` → `--font-heading-h1-size`, `--font-heading-h1-family`, etc.
- Spacing: exact names from JSON → `--spacing-xs`, `--spacing-md`, etc.
- Radius: same pattern → `--radius-sm`, etc.

All names lowercased. No hardcoded values outside this file.

### 3b. `components-reference.md`

Follow the template in `references/output-templates.md` (components-reference.md section).
For every component found in the JSON:

- Full prop table (prop name, type, valid values — exact strings from JSON)
- HTML snippet using the **triple-attribute tag pattern**:

```html
<button
  id="Button"
  class="Button"
  data-component="Button"
  data-variant-variant="Primary"
  data-variant-size="md"
></button>
```

### 3c. `coding-rules.md`

Use `references/coding-rules.md` as the base template. Customize:

- Populate the **Component Catalog** table with real data from Gate 2
- Append any **user-specific quirks** found during parsing (typos in component names,
  unusual variant values, duplicate token names across collections, etc.)
- Include the user's extra rules from Gate 0 if any

**Gate condition:** Three files generated. Do not deliver yet.

---

## GATE 4 — Generate the Output SKILL.md

Write a SKILL.md for the output skill. This is the file that will trigger in future
vibe coding sessions and enforce all the rules.

**The output SKILL.md must:**

1. **Frontmatter `description`** — trigger on UI/vibe coding requests for this brand.
   Be specific: include the brand name, "vibe code", "UI", "components", "design system".
2. **Instruct the AI to load all three reference files upfront** — no lazy-loading.
3. **Enforce mandatory constraints:**
   - Component names: exact string from `components-reference.md`
   - Colors: CSS variables from `token-map.css` only — never raw hex
   - Spacing: CSS variables from `token-map.css` only — never raw px/rem
   - Every component tag: triple-attribute pattern (id + class + data-component + data-variant-*)
   - `capture.js` injected before `<body>`
4. **Include a self-review checklist** (🔴 Errors / 🟡 Warnings / 🔵 Info)
5. **Include a "Common Mistakes" quick-reference table**

**Gate condition:** SKILL.md written for the output skill.

---

## GATE 5 — Self-Review

Review **all four generated files** against these checks:

### 🔴 Errors — fix before packaging
- [ ] Every component name in `components-reference.md` exact-matches the JSON
- [ ] Every CSS variable in `token-map.css` maps to a real token in the JSON
- [ ] `coding-rules.md` Component Catalog matches the JSON — no invented entries
- [ ] Output SKILL.md references all three files correctly
- [ ] No hardcoded hex or px values (except inside `token-map.css` definitions)
- [ ] No "OpenClaw" anywhere
- [ ] Triple-attribute pattern correct in all HTML snippets
- [ ] Typos / unusual variant strings preserved verbatim

### 🟡 Warnings — surface to user
- [ ] Token collections present in JSON but unused
- [ ] Components with >5 props (confirm all listed)

### 🔵 Info
- [ ] Total files, components, and tokens

Show summary:

```
🔴 Errors fixed: 0
🟡 Warnings: none
🔵 Info: 4 files, 5 components, 26 tokens
```

**Gate condition:** Zero 🔴 Errors.

---

## GATE 6 — Package and Deliver

1. Write the output skill folder to `/tmp/[skill-name]/`
2. Package:

```bash
cd /mnt/skills/examples/skill-creator && python -c "
import sys; sys.path.insert(0, '.')
from scripts.package_skill import package_skill
package_skill('/tmp/[skill-name]', '/home/claude/[skill-name].skill')
"
```

3. Copy to outputs and present to user:

```bash
cp /home/claude/[skill-name].skill/[skill-name].skill /mnt/user-data/outputs/
```

4. Tell the user:

> Here's your design system skill. Install it, and future vibe coding sessions will
> automatically follow your component specs, color tokens, and spacing rules.

---

## Quick Reference: Common Mistakes in Generated Files

| Mistake | Fix |
|---------|-----|
| Guessed a component name not in JSON | Only use names from the parsed data |
| Raw hex in `coding-rules.md` examples | Reference `var(--color-…)` |
| Converted `"With left icon"` to `withLeftIcon` | Keep exact string from JSON |
| Token path `Brand/Primary/500` in CSS var name | Convert to `--color-brand-primary-500` |
| Boolean prop `"Yes"`/`"No"` → `true`/`false` | Keep as string |
| Output SKILL.md description too generic | Include brand name + specific triggers |
| Forgot `capture.js` in output SKILL.md | Always include as mandatory constraint |
