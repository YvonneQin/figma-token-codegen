# Output Templates

## Layout Slots

When the user describes a page type, proactively recommend the right components.
Always verify the component name exists in the JSON before recommending it.

| Page Type | Recommended Slots | Keywords |
|-----------|-------------------|----------|
| Console / Dashboard | TopNav + Sidebar + MainContent | "console", "dashboard", "admin" |
| Form | FormField + Label + Button + Validation | "form", "input", "submit", "settings" |
| List / Data Table | Table + Pagination + Filter + SearchBar | "list", "table", "data grid" |
| Empty State | EmptyState + CTA Button | "empty", "no data", "zero state" |
| Billing / Usage | UsageChart + BillingCard + Summary | "billing", "usage", "plan" |
| Marketing / Landing | Hero + FeatureCard + CTA + Footer | "landing", "marketing", "homepage" |

If the page type doesn't match, ask which components the user expects before generating.

---

## token-map.css

All tokens as CSS custom properties:

```css
/* Auto-generated from Figma token JSON — do not edit manually */
:root {
  /* Colors */
  --color-[token-path]: [hex];

  /* Typography */
  --font-[style]-family: [value];
  --font-[style]-size: [value]px;
  --font-[style]-line-height: [value]px;

  /* Spacing */
  --spacing-[name]: [value]px;

  /* Radius */
  --radius-[name]: [value]px;
}
```

### Naming conventions

- Color token paths use `/` in Figma → convert to `-` for CSS: `Brand/Primary/500` → `--color-brand-primary-500`
- Typography style names follow the same pattern: `Heading/H1` → `--font-heading-h1-size`
- All names are lowercased in the CSS variable

---

## components-reference.md

Full prop table for every component with HTML snippets. Useful as a Windsurf/Cursor rules
file or AI context file for future sessions.

### Template structure

```markdown
# Component Reference

> Auto-generated from Figma token JSON — do not edit manually

## [ComponentName]

| Prop | Type | Valid Values |
|------|------|-------------|
| variant | enum | Primary, Secondary, Ghost |
| size | enum | sm, md, lg |
| disabled | boolean | Yes, No |

### Usage

\`\`\`html
<componentname
  id="ComponentName"
  class="ComponentName"
  data-component="ComponentName"
  data-variant-variant="Primary"
  data-variant-size="md"
></componentname>
\`\`\`
```

Repeat for every component found in the JSON.

---

> ⚠️ Do not include the word "OpenClaw" anywhere in either file — comments, filenames,
> or headings. The user's own brand name is fine.
