# Figma Token Codegen Skill

You're vibe-coding. The AI spits out beautiful UI. You paste it in. Then you spend the next 45 minutes fixing every color, replacing every generic button, and wondering why it used `#3B82F6` when your brand blue is clearly documented somewhere.

This skill fixes that. It turns your Figma design tokens into a `.skill` file — a machine-readable spec that tells any AI coder exactly which tokens, components, and rules to use. Before it writes a single line.

> 💡 Built on the [5 Agent Skill design patterns](https://x.com/GoogleCloudTech/article/2033953579824758855) by GoogleCloudTech — composes Pipeline, Inversion, Generator, and Reviewer patterns into one workflow.

---

## Why Not Just Use Claude Design?

Claude Design is genuinely good. But it's built for a different problem than the one you actually have.

**1. The allowance burns fast**
Claude Design has its own weekly meter, separate from the rest of Claude. One iterative session — comparing layouts, tweaking spacing, trying a dark mode variant — and it's gone. If you're generating dozens of options before a design review, the economics don't work.


**This skill solves a different problem.**
It doesn't compete with Claude Design on aesthetics. It solves the constraint problem: how do you make any AI coder — Claude, Cursor, Copilot, or whatever drops next week — output UI that's already compliant with your design system before it ever reaches Figma?

The answer is a spec file that travels with you. Not a closed tool that generates in isolation.

> **Use Claude Design for:** Exploratory ideation, client pitch visuals, early-stage concepts where brand compliance isn't the priority yet.
>
> **Use this skill for:** Any vibe-coding session where the output needs to be real — production-ready, brand-compliant, and directly importable into Figma via [Codifig](https://github.com/YvonneQin/codifig).

---

## What You Get

Run the skill once with your Figma token JSON. It outputs three files packaged into a `.skill`:

| File | What it does |
|------|-------------|
| `token-map.css` | Your exact CSS custom properties — no invented values |
| `components-reference.md` | Prop tables and HTML snippets for your actual components |
| `coding-rules.md` | Hard rules the AI must follow when generating UI for your brand |

Drop that `.skill` into any session and the AI codes to your spec from line one.

---

## How It Works

The skill runs a 7-step gated workflow. No step can be skipped.

| Pattern | Gate | What happens |
|---------|------|--------------|
| **Inversion** | GATE 0 — Clarify | The AI interviews you first: skill name, brand, output format, extra rules. It doesn't start generating until it has what it needs. |
| **Pipeline** | GATE 0 → GATE 6 | Strict checkpoints. Each gate must pass before the next opens. |
| **Generator** | GATE 3 — Generate | Produces all three files from templates in `references/`. |
| **Reviewer** | GATE 5 — Self-Review | Scores every file against a severity checklist (🔴 Error / 🟡 Warning / 🔵 Info) before packaging. |

---

## Figma Sync Built In

Generated HTML automatically includes `capture.js`. Open any output page with `/#figmacapture` (e.g. `http://localhost:5173/#figmacapture`) and it syncs straight back to Figma. Pair it with the [Codifig plugin](https://github.com/YvonneQin/codifig) to swap generated designs for real Figma components in one click.

---

## Install

**Option 1: Manual**
1. Clone or download this repo.
2. Zip the `figma-token-codegen` folder and rename the extension to `.skill`.
3. Upload the `.skill` to your AI assistant.

**Option 2: Terminal**
```bash
git clone https://github.com/YvonneQin/figma-token-codegen-skill.git
cd figma-token-codegen-skill
zip -r figma-token-codegen.skill figma-token-codegen/
```

Or just point your AI at the `SKILL.md` file directly — no packaging needed.

---

## Usage

Once installed, trigger it with anything like:
- "Generate a skill from my Figma tokens"
- "Create a coding skill for my design system"
- "Make a `.skill` from my components"

The AI will ask for your skill name, brand name, target format, and any extra rules. Then it'll ask for your Figma token JSON. If you haven't exported it yet, it'll give you a script to do it.

That's it. Next time you vibe-code, your design system comes with you.
