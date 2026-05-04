# Figma Token Codegen Skill

This skill generates a brand-specific coding skill from a user's Figma design tokens. The output is an installable `.skill` file containing a token map, component references, and coding rules, ready to constrain future vibe-coding sessions to your real design system.

> 💡 This skill is structured following the [5 Agent Skill design patterns](https://x.com/GoogleCloudTech/article/2033953579824758855) outlined by GoogleCloudTech. It composes multiple patterns to create a robust, multi-step workflow.

## Demo

[![Watch the demo on YouTube](https://img.youtube.com/vi/CXk4PPG52dU/hqdefault.jpg)](https://youtu.be/CXk4PPG52dU?si=BXpPS3k8IEJ_-it6)

## Why Not Claude Design?

Claude Design is genuinely impressive. But if your team already has a design system, it creates a specific problem: it generates from scratch, not from your spec.

**The allowance burns fast.** Claude Design is metered independently from the rest of Claude — its own weekly allowance, resets every seven days, extra usage costs extra. One iterative session comparing layouts, tweaking spacing, trying a dark mode variant, and it's gone. For teams that need to generate and compare dozens of options before a design review, the economics don't work.

This skill solves a different problem. It doesn't compete with Claude Design on aesthetics. It solves the constraint problem: how do you make any AI coder — Claude, Cursor, Copilot, or whatever drops next week — output UI that's already compliant with your design system before it writes a single line?

> **Use Claude Design for:** Exploratory ideation, early-stage concepts where brand compliance isn't the priority yet.
>
> **Use this skill for:** Any vibe-coding session where the output needs to be real — production-ready, brand-compliant, and directly importable into Figma.

---

## Skill Design Patterns Used

This skill is a real-world example of **pattern composition** as described in the article. It combines four of the five patterns:

| Pattern | Where it's used | Purpose |
|---------|----------------|---------|
| **Pipeline** | GATE 0 → GATE 6 | Enforces a strict 7-step workflow with hard checkpoints — no gate can be skipped or merged |
| **Inversion** | GATE 0 — Clarify | The agent interviews the user first (skill name, brand, output format, extra rules) before doing any work |
| **Generator** | GATE 3 — Generate | Produces three structured files (`token-map.css`, `components-reference.md`, `coding-rules.md`) from templates in `references/` |
| **Reviewer** | GATE 5 — Self-Review | Scores all generated files against a severity-based checklist (🔴 Error / 🟡 Warning / 🔵 Info) before packaging |

## Benefits

- **Design Consistency**: Ensures AI-generated code strictly adheres to your design system's tokens and component APIs.
- **Improved Accuracy**: Prevents the AI from hallucinating or guessing colors, spacing values, and component names.
- **Faster Workflows**: Drastically reduces back-and-forth prompting by proactively providing all rules and constraints upfront.
- **Seamless Figma Sync**: Enforces the inclusion of `capture.js` in generated HTML outputs and reminds users to open the page with `/#figmacapture` (e.g. `http://localhost:5173/#figmacapture`) for one-click synchronization back to Figma.
- **One-Click Component Replacement**: Works seamlessly with the [Codifig plugin](https://github.com/YvonneQin/codifig) to automatically replace generated designs with real Figma components.

## How it works

When you provide your Figma design tokens in JSON format (including colors, typography, spacing, and components), this skill will:
1. Parse your tokens to create a **Component Catalog** and token map.
2. Generate `token-map.css` with your design system's CSS custom properties.
3. Generate `components-reference.md` containing prop tables and HTML snippets for your components.
4. Generate `coding-rules.md` outlining specific rules and conventions.
5. Package everything into a new `.skill` file tailored to your brand.

## Installation

To use this skill locally with your AI assistant, you need to package the folder into a `.skill` file.

### Option 1: Manual
1. Download or clone this repository.
2. Compress the `figma-token-codegen` folder into a zip file and rename the extension to `.skill` (e.g. `figma-token-codegen.skill`).
3. Upload or provide the `.skill` file to your AI assistant to install it.

### Option 2: Command Line (Mac/Linux)
If you have a terminal open, you can create the skill file directly with `zip`:

```bash
# Clone the repository (replace with the actual repository URL)
git clone https://github.com/YvonneQin/figma-token-codegen-skill.git

# Navigate to the parent directory containing the skill folder
cd figma-token-codegen-skill

# Package the skill
zip -r figma-token-codegen.skill figma-token-codegen/
```

Then provide the generated `figma-token-codegen.skill` to your AI assistant.

Alternatively, depending on your AI's capabilities, you can simply point it to the `SKILL.md` file in this directory.

## Usage

Once the skill is installed, you can trigger it by asking your AI assistant:
- "Generate a skill from my Figma tokens"
- "Create a design system skill"
- "Make a coding skill from my components"

The AI will guide you through the process, asking for the desired skill name, brand name, target output format, and any extra rules you want to enforce. 

*Note: You will need to provide your exported Figma design tokens in JSON format. If you haven't extracted them yet, the AI will provide a script for you to do so.*
