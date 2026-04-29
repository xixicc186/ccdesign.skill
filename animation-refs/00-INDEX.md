# 动效参考索引

**使用方法：** 先在本文件找到你需要的效果类型 → 确定用哪个库 → 打开对应 MD 文档找具体实现代码。

---

## 快速选择：按效果类型

| 想要的效果 | 推荐方案 | 文档 |
|-----------|---------|------|
| **Hero 标题入场**（零依赖兜底） | 纯 CSS `@keyframes` | `07-vanilla.md` 效果7 |
| **文字逐词 3D 旋转飞入** | GSAP Timeline | `01-gsap.md` 效果2 |
| **文字字符/行级动画** | anime.js splitText | `02-anime.md` 效果6 |
| **打字机效果** | 纯 JS `setTimeout` | `07-vanilla.md` 效果15 |
| **滚动触发入场**（克制极简） | ScrollReveal | `04-scrollreveal.md` |
| **滚动触发入场**（序列交错） | ScrollReveal `interval` | `04-scrollreveal.md` 效果1 |
| **scrub 滚动绑定进度** | GSAP ScrollTrigger | `01-gsap.md` 效果1,3 |
| **pin 滚动时固定元素** | GSAP ScrollTrigger | `01-gsap.md` 效果3 |
| **clipPath 多边形变矩形** | GSAP ScrollTrigger | `01-gsap.md` 效果1 |
| **图片蒙版展开全屏** | GSAP pin + clipPath | `01-gsap.md` 效果3 |
| **多层视差（背景差速）** | lax.js | `03-lax.md` 效果2 |
| **鼠标跟随视差** | lax.js cursorX/Y 驱动 | `03-lax.md` 效果3 |
| **元素惯性物理感** | lax.js inertia | `03-lax.md` 效果4 |
| **卡片 3D 倾斜**（鼠标跟随） | 纯 JS | `07-vanilla.md` 效果2 |
| **卡片 3D 倾斜**（GSAP 增强） | GSAP | `01-gsap.md` 效果4 |
| **自定义光标跟随** | 纯 JS RAF | `07-vanilla.md` 效果3 |
| **按钮波纹效果** | 纯 CSS + JS | `07-vanilla.md` 效果1 |
| **按钮文字切换动画** | CSS transform + 双层 | SKILL.md Step 6（Button.jsx） |
| **导航栏滚动隐显** | GSAP / 纯 JS | `01-gsap.md` 效果6 |
| **展开卡片（flex）** | 纯 CSS transition | `07-vanilla.md` 效果5 |
| **分割式登陆页** | 纯 CSS | `07-vanilla.md` 效果9 |
| **图片轮播** | 纯 JS | `07-vanilla.md` 效果14 |
| **视频切换缩放** | GSAP | `01-gsap.md` 效果7 |
| **SVG 路径绘制** | anime.js svg.createDrawable | `02-anime.md` 效果5 |
| **复杂 Timeline 序列** | anime.js createTimeline | `02-anime.md` 效果3 |
| **stagger 交错动画** | anime.js / GSAP | `02-anime.md` 效果2 |
| **网格 stagger（批量）** | anime.js grid stagger | `02-anime.md` 效果2 |
| **Spring 弹性缓动** | anime.js createSpring | `02-anime.md` 效果8 |
| **Canvas 粒子系统** | anime.js composition:'blend' | `02-anime.md` 效果9 |
| **滚动联动（anime 版）** | anime.js onScroll | `02-anime.md` 效果7 |
| **精灵图序列** | lax.js cssFn | `03-lax.md` 效果7 |
| **hue-rotate 滤镜视差** | lax.js cssFn | `03-lax.md` 效果5 |
| **页面切换过渡（SPA）** | barba.js | `06-barba.md` |
| **CSS 预制动画（快速）** | animate.css | `05-animate-css.md` |
| **进度步骤指示器** | 纯 JS | `07-vanilla.md` 效果13 |
| **数字递增计数** | 纯 JS RAF | `07-vanilla.md` 效果15 |
| **Toast 通知** | 纯 JS + CSS | `07-vanilla.md` 效果12 |
| **模糊加载渐进清晰** | 纯 JS mapRange | `07-vanilla.md` 效果6 |
| **鼠标文字阴影** | 纯 JS | `07-vanilla.md` 效果10 |
| **Hoverboard 色块** | 纯 JS | `07-vanilla.md` 效果11 |

---

## 按库选择（风格导向）

| 网站风格 | 主力库 | 辅助库 |
|---------|-------|-------|
| 高端游戏/Awwwards 级别 | **GSAP** + ScrollTrigger | ScrollReveal 补充入场 |
| 极简克制（notion/linear/stripe） | **ScrollReveal** | 纯 CSS |
| 动效优先（framer/raycast/cursor） | **GSAP** + ScrollTrigger | lax.js |
| 活泼创意（figma/lovable/clay） | **animate.css** + ScrollReveal | 纯 JS |
| 滚动视差为主 | **lax.js** | ScrollReveal |
| 复杂 Timeline/SVG 动画 | **anime.js** | — |
| 多页面网站过渡 | **barba.js** | GSAP |
| 快速原型（无库） | 纯 CSS/JS | animate.css |

---

## 库文件路径

```
GSAP:          frontend_design_repos/award-winning-website/node_modules/gsap/dist/gsap.min.js
               frontend_design_repos/award-winning-website/node_modules/gsap/ScrollTrigger.js
anime.js:      frontend_design_repos/anime/dist/bundles/anime.umd.min.js
lax.js:        frontend_design_repos/lax.js/lib/lax.min.js
ScrollReveal:  frontend_design_repos/scrollreveal/dist/scrollreveal.min.js
animate.css:   frontend_design_repos/animate.css/animate.min.css
barba.js:      npm install @barba/core（无本地 dist，需 npm）
```

---

## 各文档内容速览

| 文件 | 内容 |
|------|------|
| `01-gsap.md` | ClipPath变形、文字飞入、pin钉选、鼠标3D、导航隐显、视频切换、进度条ticker、Timeline |
| `02-anime.md` | 基础动画、stagger、Timeline、Keyframes、SVG绘制、文字split、滚动联动、粒子、打字机 |
| `03-lax.md` | 视差入场出场、多层视差、鼠标视差、惯性动画、cssFn复杂属性、断点响应、精灵图 |
| `04-scrollreveal.md` | 序列入场、方向入场、3D旋转、缩放、loop重复、移动端禁用、AJAX后重扫描 |
| `05-animate-css.md` | 70+预制动画类名、JS动态触发、弹窗进出、按钮反馈、速度延迟控制 |
| `06-barba.md` | 淡入淡出、滑动切换、同步过渡、命名空间匹配、首次加载、全局钩子 |
| `07-vanilla.md` | 波纹、3D倾斜、光标、滚动入场、展开卡片、模糊加载、表单波浪、分割页、轮播、数字计数 |
