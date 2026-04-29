# 纯 CSS/JS 效果参考（无需库）

来源：`50projects50days` + `JavaScript30` 项目实战提炼

**适用场景：** 不想引入额外库的轻量效果、快速原型、通用交互模式

---

## 效果 1：按钮波纹（Ripple Effect）

```css
button { position: relative; overflow: hidden; }

.ripple-circle {
  position: absolute;
  background: rgba(255,255,255,0.4);
  width: 100px; height: 100px;
  border-radius: 50%;
  transform: translate(-50%, -50%) scale(0);
  animation: ripple 0.5s ease-out forwards;
  pointer-events: none;
}
@keyframes ripple {
  to { transform: translate(-50%, -50%) scale(3); opacity: 0; }
}
```

```js
document.querySelectorAll('button').forEach(btn => {
  btn.addEventListener('click', (e) => {
    const circle = document.createElement('span');
    circle.className = 'ripple-circle';
    circle.style.left = `${e.clientX - btn.offsetLeft}px`;
    circle.style.top  = `${e.clientY - btn.offsetTop }px`;
    btn.appendChild(circle);
    setTimeout(() => circle.remove(), 600);
  });
});
```

---

## 效果 2：卡片 3D 倾斜（无 GSAP）

```js
function addTilt(selector, strength = 8) {
  document.querySelectorAll(selector).forEach(el => {
    el.style.transition = 'transform 0.05s linear';
    el.addEventListener('mousemove', e => {
      const { left, top, width, height } = el.getBoundingClientRect();
      const x = (e.clientX - left) / width  - 0.5;
      const y = (e.clientY - top ) / height - 0.5;
      el.style.transform =
        `perspective(700px) rotateX(${y * -strength}deg) rotateY(${x * strength}deg) scale3d(.98,.98,.98)`;
    });
    el.addEventListener('mouseleave', () => {
      el.style.transition = 'transform 0.4s ease';
      el.style.transform = '';
    });
  });
}
addTilt('.feature-card');
```

---

## 效果 3：自定义跟随光标

```css
#cursor {
  position: fixed;
  width: 20px; height: 20px;
  border: 2px solid var(--accent, #fff);
  border-radius: 50%;
  pointer-events: none;
  z-index: 9999;
  transition: width .2s, height .2s, background .2s;
}
body:has(a:hover) #cursor,
body:has(button:hover) #cursor {
  width: 40px; height: 40px;
  background: rgba(255,255,255,0.1);
}
```

```js
const cursor = document.createElement('div');
cursor.id = 'cursor';
document.body.appendChild(cursor);

let cx = 0, cy = 0, tx = 0, ty = 0;
document.addEventListener('mousemove', e => { tx = e.clientX; ty = e.clientY; });
(function loop() {
  cx += (tx - cx) * 0.15;
  cy += (ty - cy) * 0.15;
  cursor.style.left = cx - 10 + 'px';
  cursor.style.top  = cy - 10 + 'px';
  requestAnimationFrame(loop);
})();
```

---

## 效果 4：滚动触发入场（基础版）

```css
.reveal { opacity: 0; transform: translateY(30px); transition: opacity .5s ease, transform .5s ease; }
.reveal.visible { opacity: 1; transform: none; }

/* 左右两侧入场 */
.reveal-left  { opacity: 0; transform: translateX(-30px); transition: all .5s ease; }
.reveal-right { opacity: 0; transform: translateX( 30px); transition: all .5s ease; }
.reveal-left.visible, .reveal-right.visible { opacity: 1; transform: none; }
```

```js
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) entry.target.classList.add('visible');
  });
}, { threshold: 0.1 });

document.querySelectorAll('.reveal, .reveal-left, .reveal-right').forEach(el => observer.observe(el));
```

---

## 效果 5：展开卡片（Expanding Cards）

```css
.panels { display: flex; height: 80vh; }

.panel {
  flex: 0.5;
  background-size: cover; background-position: center;
  border-radius: 20px; cursor: pointer;
  transition: flex 700ms ease-in;
  position: relative; overflow: hidden;
}
.panel.active { flex: 5; }

.panel h3 { position: absolute; bottom: 20px; left: 20px; opacity: 0; }
.panel.active h3 { opacity: 1; transition: opacity 0.3s ease 0.4s; }
```

```js
document.querySelectorAll('.panel').forEach(panel => {
  panel.addEventListener('click', () => {
    document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
    panel.classList.add('active');
  });
});
```

---

## 效果 6：加载模糊渐进清晰

```js
const mapRange = (val, inMin, inMax, outMin, outMax) =>
  ((val - inMin) * (outMax - outMin)) / (inMax - inMin) + outMin;

let progress = 0;
const interval = setInterval(() => {
  progress++;
  if (progress > 100) { clearInterval(interval); return; }
  bg.style.filter   = `blur(${mapRange(progress, 0, 100, 30, 0)}px)`;
  text.style.opacity = mapRange(progress, 0, 100, 1, 0);
}, 30);
```

---

## 效果 7：Hero 标题 CSS 入场（零依赖兜底）

```css
.hero-title .line { display: block; overflow: hidden; }
.hero-title .line-inner {
  display: block;
  animation: line-reveal 0.9s cubic-bezier(0.16, 1, 0.3, 1) both;
}
.hero-title .line:nth-child(1) .line-inner { animation-delay: 0.2s; }
.hero-title .line:nth-child(2) .line-inner { animation-delay: 0.32s; }
.hero-title .line:nth-child(3) .line-inner { animation-delay: 0.44s; }
@keyframes line-reveal {
  from { transform: translateY(105%); opacity: 0; }
  to   { transform: translateY(0);    opacity: 1; }
}
.hero-subtitle { animation: fade-up 0.8s ease 0.6s both; }
.hero-cta      { animation: fade-up 0.7s ease 0.75s both; }
@keyframes fade-up {
  from { opacity: 0; transform: translateY(18px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

---

## 效果 8：表单标签波浪动画

```js
document.querySelectorAll('.form-control label').forEach(label => {
  label.innerHTML = label.innerText.split('')
    .map((char, i) => `<span style="transition-delay:${i * 50}ms">${char}</span>`)
    .join('');
});
```

```css
.form-control label span {
  display: inline-block;
  transition: 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}
.form-control input:focus ~ label span {
  color: var(--accent);
  transform: translateY(-30px);
}
```

---

## 效果 9：分割式登陆页悬停

```css
.split-container { position: relative; width: 100vw; height: 100vh; }
.side { position: absolute; width: 50%; height: 100%; transition: width 0.5s ease; overflow: hidden; }
.side.left  { left: 0; }
.side.right { right: 0; }

.hover-left  .left  { width: 75%; }
.hover-left  .right { width: 25%; }
.hover-right .left  { width: 25%; }
.hover-right .right { width: 75%; }
```

```js
const container = document.querySelector('.split-container');
document.querySelector('.side.left') .addEventListener('mouseenter', () => container.classList.add('hover-left'));
document.querySelector('.side.left') .addEventListener('mouseleave', () => container.classList.remove('hover-left'));
document.querySelector('.side.right').addEventListener('mouseenter', () => container.classList.add('hover-right'));
document.querySelector('.side.right').addEventListener('mouseleave', () => container.classList.remove('hover-right'));
```

---

## 效果 10：鼠标移动文字阴影

```js
const hero = document.querySelector('.hero');
const text = hero.querySelector('h1');
const walk = 500;

hero.addEventListener('mousemove', function(e) {
  const { offsetWidth: w, offsetHeight: h } = hero;
  let { offsetX: x, offsetY: y } = e;
  if (this !== e.target) { x += e.target.offsetLeft; y += e.target.offsetTop; }
  const xWalk = Math.round((x / w * walk) - (walk / 2));
  const yWalk = Math.round((y / h * walk) - (walk / 2));
  text.style.textShadow = `${xWalk}px ${yWalk}px 0 rgba(255,0,255,0.7), ${xWalk * -1}px ${yWalk}px 0 rgba(0,255,255,0.7)`;
});
```

---

## 效果 11：Hoverboard 鼠标悬停色块

```js
const colors = ['#e74c3c', '#8e44ad', '#3498db', '#e67e22', '#2ecc71'];

for (let i = 0; i < 500; i++) {
  const sq = document.createElement('div');
  sq.classList.add('square');
  sq.addEventListener('mouseover', () => {
    const color = colors[Math.floor(Math.random() * colors.length)];
    sq.style.background = color;
    sq.style.boxShadow  = `0 0 2px ${color}, 0 0 10px ${color}`;
  });
  sq.addEventListener('mouseout', () => {
    sq.style.background = '#1d1d1d';
    sq.style.boxShadow  = '0 0 2px #000';
  });
  container.appendChild(sq);
}
```

---

## 效果 12：Toast 通知系统

```js
function showToast(message, type = 'info', duration = 3000) {
  const toast = document.createElement('div');
  toast.className = `toast toast-${type}`;
  toast.textContent = message;
  toast.style.cssText = `
    position: fixed; bottom: ${10 + toastsCount * 60}px; right: 10px;
    padding: 12px 24px; border-radius: 6px; background: #333; color: #fff;
    animation: toast-in 0.3s ease; z-index: 9999;
  `;
  document.body.appendChild(toast);
  setTimeout(() => { toast.style.animation = 'toast-out 0.3s ease'; setTimeout(() => toast.remove(), 300); }, duration);
}
```

```css
@keyframes toast-in  { from { transform: translateX(100%); opacity: 0; } to { transform: none; opacity: 1; } }
@keyframes toast-out { from { transform: none; opacity: 1; } to { transform: translateX(100%); opacity: 0; } }
```

---

## 效果 13：进度步骤指示器

```js
let step = 1;
const circles = document.querySelectorAll('.step-circle');
const progress = document.querySelector('.progress-bar');

function update() {
  circles.forEach((c, i) => c.classList.toggle('active', i < step));
  progress.style.width = `${((step - 1) / (circles.length - 1)) * 100}%`;
}

document.querySelector('.next-btn').addEventListener('click', () => { if (step < circles.length) { step++; update(); } });
document.querySelector('.prev-btn').addEventListener('click', () => { if (step > 1) { step--; update(); } });
```

```css
.progress-bar { height: 4px; background: var(--accent); width: 0%; transition: width 0.4s ease; }
.step-circle  { border: 3px solid #ddd; transition: border-color 0.4s; }
.step-circle.active { border-color: var(--accent); }
```

---

## 效果 14：图片轮播（自动 + 手动）

```js
const imgs = document.querySelector('.carousel-track');
const items = imgs.querySelectorAll('img');
let idx = 0;
let interval = setInterval(next, 3000);

function go(n) {
  idx = (n + items.length) % items.length;
  imgs.style.transform = `translateX(${-idx * 100}%)`;
}
function next() { go(idx + 1); }
function prev() { go(idx - 1); }

document.querySelector('.next').addEventListener('click', () => { go(idx + 1); resetInterval(); });
document.querySelector('.prev').addEventListener('click', () => { go(idx - 1); resetInterval(); });
function resetInterval() { clearInterval(interval); interval = setInterval(next, 3000); }
```

```css
.carousel-track { display: flex; transition: transform 0.5s ease-in-out; }
.carousel-track img { flex-shrink: 0; width: 100%; }
```

---

## 效果 15：数字递增计数动画

```js
function countUp(el, target, duration = 1500) {
  const start = Date.now();
  const update = () => {
    const elapsed = Date.now() - start;
    const progress = Math.min(elapsed / duration, 1);
    el.textContent = Math.floor(progress * target).toLocaleString();
    if (progress < 1) requestAnimationFrame(update);
  };
  requestAnimationFrame(update);
}

// 配合 IntersectionObserver 触发
const observer = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      countUp(entry.target, parseInt(entry.target.dataset.target));
      observer.unobserve(entry.target);
    }
  });
});
document.querySelectorAll('[data-target]').forEach(el => observer.observe(el));
```

---

## 工具函数

```js
// 范围映射（值域转换）
const mapRange = (val, inMin, inMax, outMin, outMax) =>
  ((val - inMin) * (outMax - outMin)) / (inMax - inMin) + outMin;

// 防抖（scroll 事件用）
const debounce = (fn, wait = 20) => {
  let t; return (...args) => { clearTimeout(t); t = setTimeout(() => fn(...args), wait); };
};

// RAF 节流（mousemove 用）
let rafPending = false;
el.addEventListener('mousemove', (e) => {
  if (rafPending) return;
  rafPending = true;
  requestAnimationFrame(() => { handleMove(e); rafPending = false; });
});
```
