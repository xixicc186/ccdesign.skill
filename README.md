<div align="center">

<h1>ccdesign.skill</h1>
<p>A design agent skill for Claude Code</p>

<p>
  <a href="#english">English</a> · <a href="#中文">中文</a>
</p>

</div>

---

<a name="english"></a>

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

---

<a name="中文"></a>

## 这是什么

让 Claude Code 具备专业设计能力的 skill，可以直接输出幻灯片、落地页、海报、文档和动画视频。

### 支持的输出格式

| 你的需求 | 输出结果 |
|---|---|
| 幻灯片 / PPT / 演讲稿 | 可直接用 Keynote 或 PowerPoint 打开的 `.pptx` 文件 |
| 落地页 / 原型 / App 界面 | 独立 `.html` 文件，浏览器直接打开 |
| 海报 / 封面 / Banner / 小红书封面 | 按目标尺寸精确输出的图片文件 |
| 报告 / 手册 / 文档 | 设计精良、可打印的 `.pdf` |
| 片头动画 / MG 动画 / 视频 | 通过 Remotion 渲染的视频文件 |

### 设计智能

- **风格库** — 内置 60+ 真实品牌设计系统。选一个品牌（Notion、Stripe、Cursor、苹果、法拉利……），输出就会完整复刻该品牌的配色、字体和间距规范。
- **多方案对比** — 默认生成 3 个设计方向（保守 → 标准 → 大胆），让你选最满意的。
- **杜绝 AI 烂设计** — 强渐变背景、占位符文案、通用系统字体一律禁用，输出看起来是有人设计过的。

### 示例提示词

```
帮我做一份 10 页的 B2B SaaS 融资 PPT，参考 Stripe 的风格
```
```
设计一个 App 落地页，移动端，暗色主题，类似 Raycast 的感觉
```
```
做一张团建活动海报，风格温暖活泼，尺寸 1080×1350
```
```
帮我搭一个数据仪表盘的 UI 原型，桌面端，暗色模式
```

## 安装方式

```bash
claude mcp install https://github.com/xixicc186/ccdesign.skill
```

或手动安装：克隆本仓库，将内容放入 `~/.claude/skills/design/` 目录，重启 Claude Code 即可。
