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

---

## 效果 8：viewFactor 精确触发时机

`viewFactor: 0` 元素进入视口任意一像素就触发；`viewFactor: 0.5` 需要元素一半可见才触发。

```js
// 大图片：进入 30% 才触发，避免闪出
ScrollReveal().reveal('.hero-image', {
  viewFactor: 0.3,
  distance: '0px',
  scale: 0.9,
  opacity: 0,
  duration: 900,
  easing: 'cubic-bezier(0.16, 1, 0.3, 1)'
});

// 小 badge/tag：进入视口任意像素立刻触发
ScrollReveal().reveal('.badge', {
  viewFactor: 0,
  distance: '12px',
  origin: 'bottom',
  duration: 400,
  interval: 60
});
```

---

## 效果 9：viewOffset 提前 / 延后触发

`viewOffset.bottom` 为正值时，元素需要滚动更多才触发（延后）；为负值时提前触发。

```js
// 提前 150px 触发（元素还没进入视口就开始）
ScrollReveal().reveal('.anticipate', {
  viewOffset: { bottom: -150 },
  distance: '40px',
  origin: 'bottom',
  duration: 700
});

// 延后触发（元素进入视口 200px 后才动画）— 避免首屏一打开就全动
ScrollReveal().reveal('.delayed-section', {
  viewOffset: { bottom: 200 },
  distance: '30px',
  origin: 'bottom',
  duration: 600,
  opacity: 0
});
```

---

## 效果 10：复杂 3D 旋转组合

```js
// Y 轴翻转（扑克牌翻面感）
ScrollReveal().reveal('.flip-card', {
  rotate: { x: 0, y: 90, z: 0 },
  opacity: 0,
  distance: '0',
  duration: 800,
  easing: 'cubic-bezier(0.5, 0, 0, 1)'
});

// 从角落旋转展开（纸张掉落感）
ScrollReveal().reveal('.paper', {
  rotate: { x: 15, y: -20, z: 5 },
  distance: '30px',
  origin: 'top',
  opacity: 0,
  duration: 1000,
  easing: 'cubic-bezier(0.16, 1, 0.3, 1)'
});

// 绕 X 轴翻转（百叶窗效果 + interval 序列）
ScrollReveal().reveal('.blind-row', {
  rotate: { x: 90, y: 0, z: 0 },
  opacity: 0,
  distance: '0',
  duration: 600,
  interval: 80,
  easing: 'ease-out'
});
```

---

## 效果 11：cleanup + 多阶段 reveal 堆叠

`clean()` 清除元素的 reveal 绑定，可以用来做状态切换或重新设置动画参数。

```js
const sr = ScrollReveal();

// 第一阶段：轻入场
sr.reveal('.feature', {
  distance: '20px',
  origin: 'bottom',
  duration: 500,
  interval: 100
});

// 用户点击"展开详情"后，给同一批元素叠加新效果
document.querySelector('.expand-btn').addEventListener('click', () => {
  // 清除旧绑定
  sr.clean('.feature');

  // 重新绑定更夸张的动画
  sr.reveal('.feature', {
    distance: '0',
    scale: 1.05,
    rotate: { z: 2 },
    opacity: 0,
    duration: 700,
    interval: 80,
    reset: true   // 允许多次触发
  });
});
```

---

## 效果 12：自定义容器（非 window 滚动）

当内容在一个 `overflow: scroll` 的 div 内滚动时，需要指定容器。

```js
// 把 SR 绑到侧边栏滚动容器
const sidebarSR = ScrollReveal({
  container: document.querySelector('.sidebar-scroll'),
  duration: 400,
  distance: '15px',
  origin: 'bottom'
});

sidebarSR.reveal('.sidebar-item', { interval: 60 });

// 嵌套滚动区域（如 modal 内）
document.querySelector('.open-modal').addEventListener('click', () => {
  const modalSR = ScrollReveal({
    container: document.querySelector('.modal-body'),
    viewFactor: 0.1,
    duration: 500
  });
  modalSR.reveal('.modal-section', { interval: 100 });
});
```

---

## 效果 13：组合缓动字符串

SR 的 `easing` 接受任意合法 CSS 缓动，善用这一点做品质感。

```js
const premium = {
  easing: 'cubic-bezier(0.16, 1, 0.3, 1)',   // expo out（Expo.easeOut）
  duration: 900,
  distance: '24px',
  origin: 'bottom',
  opacity: 0
};

const snappy = {
  easing: 'cubic-bezier(0.34, 1.56, 0.64, 1)',  // back out（超出再回弹）
  duration: 600,
  scale: 0.85,
  distance: '0'
};

const soft = {
  easing: 'cubic-bezier(0.25, 0.46, 0.45, 0.94)',  // quart out
  duration: 700,
  distance: '16px',
  origin: 'left'
};

ScrollReveal().reveal('.headline',     { ...premium, delay: 100 });
ScrollReveal().reveal('.cta-button',   { ...snappy,  delay: 300, interval: 80 });
ScrollReveal().reveal('.sidebar-item', { ...soft,    interval: 50 });
```
