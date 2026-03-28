# Coding Rules

All rules are mandatory — violating any one is a failed output.

---

## Rule 1 — No Guessing

Never guess prop values, color names, component names, or spacing values. If a value is not
in the JSON, ask. "It's probably called..." is not acceptable.

## Rule 2 — Exact String Match

All component names, prop names, and variant values must match the JSON **character for
character**:

- **Preserve typos.** If the JSON says `Primay`, write `Primay`.
- **Case-sensitive.** `Button` ≠ `button` ≠ `BUTTON`
- **Space-sensitive.** `"With left icon"` ≠ `"with-left-icon"` ≠ `"WithLeftIcon"`

## Rule 3 — Triple-Attribute Tag Pattern

Every custom component tag must carry three attributes simultaneously:

```html
<productnavigation
  id="ProductNavigation"
  class="ProductNavigation"
  data-component="ProductNavigation"
  data-variant-product="Cloud Networking"
></productnavigation>
```

| Attribute | Value |
|-----------|-------|
| Tag name | lowercase (HTML spec) |
| `id` / `class` / `data-component` | exact casing from JSON |
| `data-variant-[prop]` | one per variant prop, exact string from JSON |

## Rule 4 — Figma Sync Script (Always Required)

Every generated HTML file must include this **before** the `<body>` tag:

```html
<script src="https://mcp.figma.com/mcp/html-to-design/capture.js"></script>
<body>
  <!-- page content -->
</body>
```

This enables one-click push from localhost to Figma via the html-to-design plugin.
For React projects, add it to `index.html` in the same position.

When previewing the page locally, always remind the user to append `/#figmacapture` to the URL
(e.g. `http://localhost:5173/#figmacapture`) to trigger automatic capture by the Figma plugin.

## Rule 5 — No Hardcoded Values

| Type | Use |
|------|-----|
| Colors | `var(--color-brand-primary)` |
| Spacing | `var(--spacing-md)` |
| Font sizes | `var(--font-body-size)` |

Never write a raw hex code, px value, or rem value that doesn't come from the token JSON.

---

## Component Catalog

After parsing, build a lookup table for every component found in the JSON:

| Component | Prop | Valid Values |
|-----------|------|--------------|
| Button | variant | Primary, Secondary, Ghost |
| Button | size | sm, md, lg |

For components with more than 5 props, list them separately with full detail below the table.
Reference this table during generation — never rely on memory.

---

## Known Quirks

Things that trip up AI — preserve these, never "fix" them:

- Figma component names may contain typos — preserve verbatim
- Variant values are often full phrases (`"With left icon"`, `"On dark bg"`) — never convert
  to camelCase or kebab-case
- Color token paths use `/` as separator (`Brand/Primary/500`) — convert to `-` for CSS
  variables: `--color-brand-primary-500`
- A token name may appear in multiple collections — prefer narrower scope
  (component-level > global)
- Some boolean props use `"Yes"` / `"No"` instead of `true` / `false` — use the string as-is

**User-specific quirks:** *(append anomalies found after parsing the user's JSON)*

---

## Pre-Generation Checklist

Run through this before delivering any output:

- [ ] Token JSON is in context
- [ ] All component names sourced from JSON — no guesses
- [ ] Colors use CSS variables — no hardcoded hex
- [ ] Spacing uses CSS variables — no hardcoded px
- [ ] Every component tag uses the triple-attribute pattern
- [ ] `capture.js` injected before `<body>`
- [ ] Known Quirks reviewed — no autocorrected typos or reformatted variant strings
