# ScrollReveal 动效参考

**本地库路径：** `frontend_design_repos/scrollreveal/dist/scrollreveal.min.js`

```html
<script src="frontend_design_repos/scrollreveal/dist/scrollreveal.min.js"></script>
```

**适用场景：** 克制极简网站的元素入场（notion/linear/stripe 风格）、长页面分块内容展示、无需 scrub 的基础滚动触发

---

## 基础用法

```js
// 最简用法
ScrollReveal().reveal('.headline');

// 带配置
ScrollReveal().reveal('.card', {
  delay: 200,
  distance: '20px',
  duration: 800,
  origin: 'bottom',  // 'top' | 'bottom' | 'left' | 'right'
  opacity: 0,
  scale: 0.9,
  easing: 'ease-out'
});
```

---

## 效果 1：序列入场（元素一个接一个）

```js
ScrollReveal().reveal('.feature-card', {
  interval: 150,   // 每个元素延迟 150ms
  distance: '30px',
  origin: 'bottom',
  duration: 600
});
```

---

## 效果 2：从不同方向入场

```js
const sr = ScrollReveal({ duration: 700, ease: 'cubic-bezier(0.5, 0, 0, 1)' });

// 从左右两侧同时滑入
sr.reveal('.left-content',  { origin: 'left',  distance: '50px', delay: 0   });
sr.reveal('.right-content', { origin: 'right', distance: '50px', delay: 100 });

// 从底部淡入（默认效果）
sr.reveal('.section-title', { origin: 'bottom', distance: '20px', opacity: 0 });
```

---

## 效果 3：3D 旋转入场

```js
ScrollReveal().reveal('.card', {
  rotate: { x: 0, y: 30, z: 0 },  // Y 轴旋转
  distance: '0',
  duration: 800,
  opacity: 0
});
```

---

## 效果 4：缩放入场

```js
ScrollReveal().reveal('.logo', {
  scale: 0.5,
  duration: 600,
  ease: 'cubic-bezier(0.5, 0, 0, 1)'
});
```

---

## 效果 5：loop 循环（离开视口后重置）

```js
ScrollReveal().reveal('.counter-box', {
  reset: true,   // 元素离开视口后重置，再次进入时重新动画
  duration: 500,
  origin: 'bottom',
  distance: '20px'
});
```

---

## 效果 6：移动端禁用

```js
ScrollReveal().reveal('.heavy-animation', {
  mobile: false,   // 移动端不触发（性能考虑）
  distance: '40px',
  duration: 800
});
```

---

## 效果 7：回调 + 动态内容

```js
ScrollReveal().reveal('.item', {
  beforeReveal: (el) => el.classList.add('is-visible'),
  afterReveal:  (el) => console.log('revealed', el)
});

// AJAX 加载新内容后重新扫描
fetch('/more').then(() => {
  document.body.insertAdjacentHTML('beforeend', newHtml);
  ScrollReveal().sync();
});
```

---

## 完整配置参数

```js
ScrollReveal().reveal('.el', {
  delay: 0,                                      // 延迟（ms）
  distance: '0',                                 // 位移距离（'20px', '5%'）
  duration: 600,                                 // 时长（ms）
  easing: 'cubic-bezier(0.5, 0, 0, 1)',         // 缓动
  interval: 0,                                   // 序列间隔（ms）
  opacity: 0,                                    // 起始透明度
  origin: 'bottom',                              // 方向
  rotate: { x: 0, y: 0, z: 0 },                // 初始旋转
  scale: 1,                                      // 初始缩放
  reset: false,                                  // 是否可重复触发
  mobile: true,                                  // 移动端启用
  desktop: true,                                 // 桌面端启用
  viewFactor: 0.0,                               // 需可见比例（0-1）
  viewOffset: { top: 0, right: 0, bottom: 0, left: 0 }
});
```

---

## 与 ScrollReveal vs GSAP 的选择

| 场景 | 用 ScrollReveal | 用 GSAP |
|------|----------------|---------|
| 克制极简网站入场 | ✓ | — |
| 需要 scrub（滚动绑定进度） | — | ✓ |
| 需要 pin（滚动时固定） | — | ✓ |
| clipPath 变形 | — | ✓ |
| 基础滚动触发入场 | ✓ | 可以 |
| 快速原型 | ✓ | — |
