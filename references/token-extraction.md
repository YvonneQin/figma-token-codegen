# Token Extraction Guide

Tell the user:

> To generate UI that precisely matches your design system, I need your Figma design tokens.
> Here's how to extract them in 2 minutes:
>
> **1.** Open your Figma **component library file** — the one where your team's components
> are published, not just any Figma file.
>
> **2.** Open the browser **DevTools Console**:
> `Command + Option + I` (Mac) or `Ctrl + Shift + I` (Windows), then click the Console tab.
>
> **3.** If you see a "don't paste code you don't understand" warning, type `allow pasting`
> and press Enter.
>
> **4.** Paste this script and press Enter:

```javascript
const result = {
  colors: figma.getLocalPaintStyles().map(s => ({
    name: s.name, paints: s.paints
  })),
  typography: figma.getLocalTextStyles().map(s => ({
    name: s.name,
    fontFamily: s.fontName.family,
    fontStyle: s.fontName.style,
    fontSize: s.fontSize,
    lineHeight: s.lineHeight,
    letterSpacing: s.letterSpacing
  })),
  variables: figma.variables.getLocalVariableCollections().map(col => ({
    collection: col.name,
    variables: col.variableIds.map(id => {
      const v = figma.variables.getVariableById(id);
      return { name: v.name, values: v.valuesByMode };
    })
  })),
  components: figma.root.findAll(n => n.type === "COMPONENT").map(c => ({
    name: c.name,
    key: c.key,
    description: c.description
  })),
  componentSets: figma.root.findAll(n => n.type === "COMPONENT_SET").map(cs => ({
    name: cs.name,
    key: cs.key,
    variants: cs.children.map(c => c.name)
  }))
};
copy(JSON.stringify(result, null, 2));
console.log('✅ Copied to clipboard!');
```

> **5.** The script needs to scan all components and variables in the file — this may take
> a moment depending on the size of your library. **Please wait patiently until you see
> `✅ Copied to clipboard!` in the console.** Do not re-run the script or close the tab
> while it's running.
>
> **6.** Once you see the ✅ message, paste the result here.

**Wait for the JSON. Do not simulate or skip this step.**
