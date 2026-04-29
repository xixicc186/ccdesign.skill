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
| **DOM 状态无缝切换（Flip）** | GSAP Flip plugin | `01-gsap.md` 效果11 |
| **元素沿 SVG 路径运动** | GSAP MotionPathPlugin | `01-gsap.md` 效果12 |
| **拖拽 + 边界 + 碰撞检测** | GSAP Draggable | `01-gsap.md` 效果13 |
| **全屏翻页（滚轮/触摸）** | GSAP Observer | `01-gsap.md` 效果14 |
| **打字机 / 文字轮播** | GSAP TextPlugin | `01-gsap.md` 效果15 |
| **抖动/慢镜/完全自定义缓动** | GSAP RoughEase/SlowMo/CustomEase | `01-gsap.md` 效果16 |
| **滚动速度 skewY 倾斜感** | GSAP ScrollTrigger velocity | `01-gsap.md` 效果17 |
| **网格 stagger 从中心扩散** | GSAP grid stagger | `01-gsap.md` 效果18 |
| **SVG 形状变形（morphTo）** | anime.js svg.morphTo | `02-anime.md` 效果11 |
| **值域修改器（正弦漂移/取整/循环）** | anime.js modifier | `02-anime.md` 效果12 |
| **萤火虫/混合模式粒子** | anime.js composition:blend | `02-anime.md` 效果13 |
| **DOM 重排自动过渡动画** | anime.js createLayout | `02-anime.md` 效果14 |
| **有机感打字机缓动** | anime.js irregular easing | `02-anime.md` 效果15 |
| **弹性拖拽表盘/旋钮** | anime.js Draggable + Spring | `02-anime.md` 效果16 |
| **Timeline .set() 闪现序列** | anime.js Timeline | `02-anime.md` 效果17 |
| **齿轮/波浪循环旋转（modValue）** | lax.js modValue | `03-lax.md` 效果9 |
| **滚动 + 鼠标融合视差** | lax.js 多驱动器 | `03-lax.md` 效果10 |
| **速度感动态阴影（惯性）** | lax.js inertia + cssFn | `03-lax.md` 效果11 |
| **onUpdate 逐帧 DOM 控制** | lax.js onUpdate | `03-lax.md` 效果12 |
| **水平 Snap 滚动视差** | lax.js scrollX driver | `03-lax.md` 效果13 |
| **精确触发时机（可见比例）** | ScrollReveal viewFactor | `04-scrollreveal.md` 效果8 |
| **提前/延后触发偏移** | ScrollReveal viewOffset | `04-scrollreveal.md` 效果9 |
| **Y 轴翻转/百叶窗/纸落入场** | ScrollReveal 3D rotate | `04-scrollreveal.md` 效果10 |
| **多阶段 reveal 堆叠/重绑定** | ScrollReveal cleanup | `04-scrollreveal.md` 效果11 |
| **非 window 容器内滚动触发** | ScrollReveal container | `04-scrollreveal.md` 效果12 |
| **品质感缓动字符串组合** | ScrollReveal easing | `04-scrollreveal.md` 效果13 |

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
| `01-gsap.md` | ClipPath、文字飞入、pin、鼠标3D、导航隐显、视频切换、Timeline + **Flip、MotionPath、Draggable、Observer、TextPlugin、RoughEase/CustomEase、velocity skew、grid stagger** |
| `02-anime.md` | stagger、Timeline、SVG绘制、splitText、滚动联动、粒子 + **morphTo、modifier、blend fireflies、createLayout、irregular、Spring dial、.set() flash** |
| `03-lax.md` | 多层视差、鼠标视差、惯性、精灵图、onUpdate + **modValue循环、多驱动融合、速度阴影、逐帧DOM、水平Snap** |
| `04-scrollreveal.md` | 序列入场、方向入场、3D旋转、loop + **viewFactor精确触发、viewOffset偏移、复杂3D翻转、cleanup堆叠、自定义容器、品质缓动** |
| `05-animate-css.md` | 70+预制动画类名、JS动态触发、弹窗进出、按钮反馈、速度延迟控制 |
| `06-barba.md` | 淡入淡出、滑动切换、同步过渡、命名空间匹配、首次加载、全局钩子 |
| `07-vanilla.md` | 波纹、3D倾斜、光标、滚动入场、展开卡片、模糊加载、表单波浪、分割页、轮播、数字计数 |
