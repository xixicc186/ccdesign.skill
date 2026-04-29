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
