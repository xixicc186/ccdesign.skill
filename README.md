<div align="center">

<h1>ccdesign.skill</h1>
<p>A design agent skill for Claude Code</p>

<p>
  <a href="README.md">English</a> · <a href="README-CN.md">中文</a>
</p>

</div>

---

## What It Does

A skill that turns Claude Code into a professional design agent — capable of producing slides, landing pages, posters, documents, and motion videos on demand.

### Output Types

| Ask for | You get |
|---|---|
| Slides / deck / presentation | A `.pptx` file ready to open in Keynote or PowerPoint |
| Landing page / UI prototype / app mockup | A standalone `.html` file, opens in any browser |
| Poster / cover / banner / social image | A pixel-perfect image file at your target dimensions |
| Report / handbook / PDF document | A designed, print-ready `.pdf` |
| Intro animation / motion video | A rendered video via Remotion |

### Design Intelligence

- **Style library** — 60+ real brand design systems built in. Pick a brand (Notion, Stripe, Cursor, Apple, Ferrari…) and your output matches that aesthetic with exact colors, typography, and spacing rules.
- **Multiple variants** — Generates 3 design directions by default (conservative → standard → bold) so you can pick the one you prefer.
- **No AI slop** — Gradient-heavy backgrounds, filler copy, and generic system fonts are explicitly forbidden. Outputs look intentional, not generated.

### Example Prompts

```
Make a 10-slide pitch deck for a B2B SaaS product, Stripe style
```
```
Design a mobile landing page for my app, dark theme, similar to Raycast
```
```
Create a poster for our team offsite, warm and playful, 1080×1350px
```
```
Build a UI prototype for a dashboard, desktop, dark mode
```

## Installation

```bash
claude mcp install https://github.com/xixicc186/ccdesign.skill
```

Or manually: clone this repo and place the contents into your `~/.claude/skills/design/` directory, then restart Claude Code.
