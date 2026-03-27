# Figma Token Codegen Skill

This skill generates a brand-specific coding skill from a user's Figma design tokens. The output is an installable `.skill` file containing a token map, component references, and coding rules, ready to constrain future vibe-coding sessions to your real design system.

## Benefits

- **Design Consistency**: Ensures AI-generated code strictly adheres to your design system's tokens and component APIs.
- **Improved Accuracy**: Prevents the AI from hallucinating or guessing colors, spacing values, and component names.
- **Faster Workflows**: Drastically reduces back-and-forth prompting by proactively providing all rules and constraints upfront.
- **Seamless Figma Sync**: Enforces the inclusion of `capture.js` in generated HTML outputs for one-click synchronization back to Figma.
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

### Figma Synchronization (capture.js)

The generated `.skill` file will include a coding rule that enforces the injection of the `capture.js` script (`https://mcp.figma.com/mcp/html-to-design/capture.js`) into any generated HTML files. This script enables one-click push from localhost to Figma via the html-to-design plugin.
