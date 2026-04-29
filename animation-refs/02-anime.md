# Anime.js 动效参考

**本地库路径：** `frontend_design_repos/anime/dist/bundles/anime.umd.min.js`

```html
<script src="frontend_design_repos/anime/dist/bundles/anime.umd.min.js"></script>
```

**适用场景：** 复杂 Timeline 序列、SVG 路径绘制/变形、文字字符动画、交错动画、Canvas 粒子系统

---

## 效果 1：基础 CSS/DOM 动画

```js
import { animate, stagger } from 'animejs';

animate('.square', {
  x: 320,
  rotate: { from: -180 },
  duration: 1250,
  delay: stagger(65, { from: 'center' }),
  ease: 'inOutQuint',
  loop: true,
  alternate: true   // 往返
});
```

**常用属性：**
- `duration` — 时长（ms，默认 1000）
- `delay` — 延迟（支持 stagger 函数）
- `ease` — 缓动
- `loop` — 循环次数（`true` 或 `-1` 无限）
- `alternate` — 往返播放
- `composition: 'blend'` — 多动画叠加（不覆盖）

---

## 效果 2：Stagger 交错动画

```js
// 基础交错
animate('.card', { x: 100, delay: stagger(50) });

// 从中心交错
animate('.dot', { scale: 2, delay: stagger(50, { from: 'center' }) });

// 网格交错（grid = [行数, 列数]）
const grid = [5, 10];
animate('.dot', {
  x: stagger('-.175rem', { grid, from: index, axis: 'x' }),
  y: stagger('-.175rem', { grid, from: index, axis: 'y' }),
  delay: stagger(50, { grid, from: index }),
  duration: 500
});
```

---

## 效果 3：Timeline 时间轴序列

```js
import { createTimeline, stagger } from 'animejs';

const tl = createTimeline({
  defaults: { ease: 'inOutSine', duration: 800 }
})
.add('.hero-title', { opacity: [0, 1], y: [30, 0] }, 0)
.add('.hero-sub',   { opacity: [0, 1], y: [20, 0] }, 200)   // 200ms 后开始
.add('.hero-cta',   { opacity: [0, 1], y: [10, 0] }, 400)
.init();

// 常用时间位置标记
// .add(el, props, 0)        — 绝对时间（从 0 开始）
// .add(el, props, '+=500')  — 上一个结束后 500ms
// .add(el, props, '-=200')  — 上一个结束前 200ms 开始（重叠）
// .add(el, props, '<<')     — 与上一个同时开始
```

---

## 效果 4：Keyframes 多步骤关键帧

```js
animate('.card', {
  keyframes: [
    { scale: 0.625 },
    { scale: 1.125 },
    { scale: 1 }
  ],
  duration: 600
});

// 每帧独立配置
animate('.el', {
  keyframes: [
    { x: 0,   ease: 'outExpo',   duration: 400 },
    { x: 200, ease: 'inOutBack', duration: 600 },
    { x: 100, ease: 'spring',    duration: 300 }
  ]
});
```

---

## 效果 5：SVG 路径绘制（stroke 动画）

```js
import { svg, createTimeline, stagger } from 'animejs';

createTimeline({
  defaults: { ease: 'inOut(4)', duration: 2000, loop: true }
})
.add(svg.createDrawable('.path'), {
  draw: ['0.5 0.5', '0 1', '0.5 0.5'],  // [起点偏移 终点偏移]
  stroke: '#FF4B4B'
}, stagger([0, 8000], { start: 0 }));
```

---

## 效果 6：文字字符级动画（splitText）

```js
import { splitText, createTimeline, stagger } from 'animejs';

const split = splitText('h1', { lines: true });

createTimeline({
  defaults: { alternate: true, loop: true, duration: 1500, ease: 'inOutQuad' }
})
.add(split.lines, { color: { from: '#61C3FF' }, y: -10, scale: 1.1 }, stagger(100))
.add(split.words, { scale: [0.98, 1.04] }, stagger(100))
.init();

// split.revert() — 恢复原始 DOM
// 支持：split.chars / split.words / split.lines
```

---

## 效果 7：滚动联动（onScroll）

```js
import { createTimeline, onScroll } from 'animejs';

createTimeline({
  autoplay: onScroll({
    target: '.sticky-container',
    enter: 'top top',
    leave: 'bottom bottom',
    sync: 0.5   // 滚动同步系数
  })
})
.add('.stack', { rotateY: [-180, 0], ease: 'in(2)' }, 0);
```

---

## 效果 8：Spring 弹性缓动 + Draggable

```js
import { createDraggable, createSpring } from 'animejs';

const draggable = createDraggable('.card', {
  container: () => [0, containerW, containerH, 0],
  x: { snap: (val) => Math.round(val / 100) * 100 }, // 磁吸网格
  y: false,
  velocityMultiplier: 4,
  containerFriction: 1
});

// Spring 缓动（用于释放后的弹回）
animate('.el', {
  x: 0,
  ease: createSpring({ damping: 8, mass: 1, stiffness: 100 })
});
```

---

## 效果 9：Canvas 粒子系统（对象动画）

```js
import { animate, stagger, createTimer } from 'animejs';

const particles = Array.from({ length: 100 }, () => ({ x: 0, y: 0, radius: 1 }));
const canvas = document.querySelector('canvas');
const ctx = canvas.getContext('2d');

function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  particles.forEach(p => {
    ctx.beginPath();
    ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
    ctx.fill();
  });
}

// 鼠标移动时粒子跟随
document.addEventListener('mousemove', (e) => {
  animate(particles, {
    x: e.clientX,
    y: e.clientY,
    radius: () => Math.random() * 6 + 2,
    delay: stagger(40),
    duration: stagger(120, { start: 750 }),
    ease: 'inOut',
    composition: 'blend',  // 叠加不覆盖
    onUpdate: draw
  });
});
```

---

## 效果 10：打字机效果（irregular 缓动）

```js
import { createTimeline, stagger, eases } from 'animejs';
const steps = 20;

createTimeline({
  playbackEase: eases.irregular(steps, 2)  // 模拟真实打字节奏
})
.set('.char', { opacity: [0, 1] }, stagger(50))
.init();
```

---

## 缓动函数速查

```
// 标准
inSine / outSine / inOutSine
inQuad / outQuad / inOutQuad
inCubic / outCubic / inOutCubic
inQuart / outQuart / inOutQuart
inQuint / outQuint / inOutQuint
inExpo / outExpo / inOutExpo

// Power（动态强度）
in(2) / out(3) / inOut(4)

// 弹性
spring({ damping, mass, stiffness, velocity })
bounce({ elasticity })

// 特殊
steps(8)            — 步进
irregular(n, p)     — 不规则（打字机）
easeOutBack         — 超出后回弹
```

---

## 效果 11：svg.morphTo — SVG 形状变形

在两个 SVG 路径之间平滑过渡，适合 Logo 动画、图标切换。

```js
import { createTimeline, svg } from 'animejs';

// 基础形变：从 path1 变形到 path2
createTimeline()
  .add('#shape', {
    d: svg.morphTo('#target-shape', 0.33),  // 第二参数：精度 0-1
    duration: 800,
    ease: 'inOutQuad'
  })
  .init();

// 多阶段连续变形（Logo 动画）
createTimeline({ loop: true, alternate: true })
  .add('#logo-path', {
    d: [
      { to: svg.morphTo('#state-1'), duration: 600 },
      { to: svg.morphTo('#state-2'), duration: 600 },
      { to: svg.morphTo('#state-3'), duration: 600 }
    ],
    ease: 'inOutSine'
  })
  .init();
```

**适合场景：** Logo 动画、图标 hover 变形、数据可视化图形切换

---

## 效果 12：Modifier — 每帧实时转换值

在动画运行的每一帧对当前值做自定义变换，实现正弦漂移、无限轮播、整数计数等。

```js
import { animate, createTimeline, utils } from 'animejs';

// 正弦波漂移（飘浮感）
const tl = createTimeline();
tl.add('.float-el', {
  x: utils.random(-200, 200),
  modifier: (x) => x + Math.sin(tl.currentTime * 0.002) * 80,
  duration: 3000,
  ease: 'inOut(2)'
});

// 数字计数器（只显示整数）
animate('#counter', {
  count: [0, 9999],
  modifier: utils.round(0),
  duration: 2000,
  ease: 'outExpo'
});

// 无限轮播（值超出边界自动折回）
const carousel = createAnimatable('.track', { x: 0 });
animate('.track', {
  x: -totalWidth,
  duration: 8000,
  ease: 'linear',
  loop: true,
  modifier: (x) => utils.wrap(x, -totalWidth / 2, 0)
});
```

**适合场景：** 浮动装饰元素、计数器动画、无限横向滚动

---

## 效果 13：composition: 'blend' — 多动画叠加（萤火虫效果）

多个动画同时作用于同一属性时，叠加而不是覆盖，产生群体有机运动感。

```js
import { animate, stagger, createTimer } from 'animejs';

// 萤火虫效果：鼠标附近的粒子持续漂浮
const pointer = { x: window.innerWidth / 2, y: window.innerHeight / 2 };
document.addEventListener('mousemove', e => { pointer.x = e.clientX; pointer.y = e.clientY; });

createTimer({
  frameRate: 8,  // 每秒触发 8 次，降低 CPU
  onUpdate: () => {
    const count = particleEls.length;
    particleEls.forEach((el, i) => {
      const angle = (i / count) * Math.PI * 2;
      const radius = 80 + Math.random() * 60;
      animate(el, {
        x: Math.cos(angle) * radius + pointer.x,
        y: Math.sin(angle) * radius + pointer.y,
        scale: 0.5 + Math.random() * 0.5,
        composition: 'blend',  // 与上一帧动画叠加，不覆盖
        duration: utils.random(800, 1600),
        ease: 'inOut(3)'
      });
    });
  }
});
```

**适合场景：** 粒子系统、光晕效果、群体运动

---

## 效果 14：createLayout — DOM 变化自动动画

DOM 结构改变（元素增删、重排）时，自动生成过渡动画，类似 GSAP Flip。

```js
import { createLayout, stagger, utils } from 'animejs';

const layout = createLayout('.grid', {
  duration: 800,
  ease: 'inOutExpo',
  // 新元素进场
  enterFrom: {
    opacity: 0,
    scale: 0.5,
    duration: 500,
    delay: 200
  },
  // 旧元素退场
  leaveTo: {
    opacity: 0,
    y: () => utils.random(-100, 100),
    rotate: () => utils.random(-20, 20),
    duration: 400,
    delay: stagger([0, 150], { from: 'random' })
  }
});

// 用法：改变 DOM 前 record，改变后 animate
filterBtn.addEventListener('click', () => {
  layout.record();          // 记录当前位置
  applyFilter();            // 任意 DOM 操作
  layout.animate();         // 自动生成过渡
});
```

**适合场景：** 可筛选列表、购物车增删、看板拖拽后的重排

---

## 效果 15：irregular easing — 模拟有机手感

生成带受控随机性的缓动，让动画看起来"有人味"，打字机效果必备。

```js
import { createTimeline, easings, stagger } from 'animejs';

// 打字机效果（不规则间隔模拟真实打字）
const chars = document.querySelectorAll('.char');
createTimeline({
  playbackEase: easings.irregular(chars.length, 0.6)
  // irregular(步数, 随机强度 0-1)
  // 随机强度 0 = 完全均匀；1 = 完全随机
})
.set(chars, { opacity: [0, 1] }, stagger(80))
.init();

// 手绘感 SVG 路径动画
const drawables = svg.createDrawable('.hand-drawn-line');
createTimeline({
  playbackEase: easings.irregular(20, 0.4)
})
.add(drawables, { draw: ['0 0', '0 1'], duration: 2000 })
.init();
```

**适合场景：** 打字机效果、手写感动画、模拟真实物理的 UI

---

## 效果 16：Draggable + Spring 弹性投掷

拖拽后松手，元素像有弹性一样飞出并弹回。

```js
import { createDraggable, createSpring } from 'animejs';

createDraggable('.card', {
  x: { snap: 0 },      // 松手后 x 归零
  y: { snap: 0 },      // 松手后 y 归零
  releaseEase: createSpring({ stiffness: 120, damping: 8 }),  // 弹性回弹
  velocityMultiplier: 2,   // 放大投掷速度感
  onGrab()    { gsap.to(this.target, { scale: 1.1 }); },
  onRelease() { gsap.to(this.target, { scale: 1   }); }
});

// 带边界约束的弹性拖拽
createDraggable('.slider-thumb', {
  x: { mapTo: [0, trackWidth] },
  y: false,
  onDrag: (self) => updateValue(self.x / trackWidth)
});
```

**适合场景：** 可抛掷卡片、弹性滑块、拖拽反馈

---

## 效果 17：Timeline .set() 闪现序列

`.set()` 是零时长的瞬间变化，配合 stagger 可做闪光、逐一出现等效果。

```js
import { createTimeline, stagger } from 'animejs';

// 文字字符逐一闪现（比 opacity 渐变更有力）
createTimeline()
.set('.char', { opacity: 0 })
.set('.char', {
  opacity: 1,
  color: ['#fff', 'var(--accent)'],
  delay: stagger(40)
})
.add('.char', {
  color: 'var(--text)',
  duration: 400,
  delay: stagger(40)
}, stagger(40));
```
