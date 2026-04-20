<div align="center">

# ccdesign.skill

**让 Claude Code 变成你的 AI 创意总监。**  
你说做什么，它像设计师一样思考，而不只是生成代码。

</div>

---

## 能做什么

`/design` 是所有视觉创作任务的统一入口。你描述需求 —— 投资人 PPT、落地页、海报、PDF 报告 —— skill 先做设计决策（颜色、字体、布局、多方案对比），再交给对应工具生成文件。

**支持的输出类型：**

| 需求 | 输出 |
|------|------|
| PPT / 幻灯片 / deck | `.pptx`（PptxGenJS 生成）|
| 落地页 / 原型 / 网页界面 | 独立 `.html` 文件 |
| 海报 / 封面 / 小红书封面 | 固定尺寸 `.html` → 截图 |
| 报告 / 手册 / A4 文档 | 可打印 `.html` → `.pdf` |
| 动画 / 视频 / MG 动画 | Remotion 项目 |

---

## 54 个内置品牌设计系统

本地离线，无需网络请求，读取速度更快。

**AI / 大模型：** `claude` · `x.ai` · `elevenlabs` · `mistral.ai` · `ollama` · `replicate` · `minimax` · `together.ai` · `voltagent` · `cohere`

**极简 / 工程师审美：** `notion` · `linear` · `vercel` · `stripe` · `resend` · `cal` · `expo` · `opencode.ai`

**暗色 / 科技感：** `cursor` · `raycast` · `superhuman` · `warp` · `spacex` · `nvidia` · `sentry` · `runwayml`

**活泼 / 创意：** `figma` · `framer` · `lovable` · `clay` · `miro` · `posthog` · `zapier` · `webflow` · `airtable`

**商务 / 企业：** `apple` · `ibm` · `airbnb` · `intercom` · `hashicorp` · `mongodb` · `sanity`

**金融：** `coinbase` · `revolut` · `wise` · `kraken`

**其他：** `spotify` · `mintlify` · `supabase` · `uber` · `pinterest` · `composio` · `clickhouse` · `bmw`

每个文件包含：精确 HEX 色值、字体搭配、间距体系、组件样式、设计规范。

---

## 工作流程

```
你：/design 给我做个 SaaS 产品落地页，Notion 风格
```

skill 会：

1. 判断输出类型（原型 → HTML）
2. 问 3–5 个定向问题（受众、内容、是否需要交互）
3. 读取本地 `design-md/notion/DESIGN.md`（无网络请求）
4. 在写任何代码前，大声说出每个设计决定
5. 生成 3 个方案（保守 → 标准 → 大胆）
6. 自动截图，并排展示
7. 你选一个，导出

---

## 安装

```bash
git clone https://github.com/xixicc186/ccdesign.skill ~/.claude/skills/design
```

然后在 Claude Code 中：

```
/design 你的需求
```

---

## 设计理念

- **拒绝 AI 垃圾设计** — 不用渐变大背景、不用 emoji 装饰、不用 Inter 做展示字、不用左边彩色 border 的卡片
- **品牌原汁原味** — 读真正的设计规范文件，不是摘要
- **至少 3 个方案** — 保守、标准、大胆，让你选择，而不是直接接受
- **解释每个决定** — "V1 用深海军蓝配奶油白传递专业感；V2 用珊瑚红更有活力" — 你始终掌控方向
- **正确分工** — design skill 管美学决策；pptx/pdf/remotion skill 管技术生成

---

## 依赖

skill 需要其他 skill 来生成文件，按需安装：

- `pptx` skill — 生成 `.pptx`
- `pdf` skill — 生成 `.pdf`
- `remotion` skill — 生成视频

---

## 开源协议

MIT
