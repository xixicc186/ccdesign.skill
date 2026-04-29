# Animate.css 动效参考

**本地库路径：** `frontend_design_repos/animate.css/animate.min.css`

```html
<link rel="stylesheet" href="frontend_design_repos/animate.css/animate.min.css">
```

**适用场景：** 快速原型、按钮点击反馈、通知弹窗进出、不需要定制的预制动画

---

## 基础用法

```html
<!-- 添加两个类名即可 -->
<div class="animate__animated animate__fadeInUp">内容</div>

<!-- 带延迟 -->
<div class="animate__animated animate__bounceIn animate__delay-1s">内容</div>

<!-- 无限循环 -->
<div class="animate__animated animate__pulse animate__infinite">心跳</div>
```

---

## 动画类名速查

### 进入动画（Entrances）
```
animate__fadeIn          animate__fadeInUp        animate__fadeInDown
animate__fadeInLeft      animate__fadeInRight
animate__bounceIn        animate__bounceInUp      animate__bounceInDown
animate__slideInUp       animate__slideInDown
animate__slideInLeft     animate__slideInRight
animate__zoomIn          animate__zoomInUp        animate__zoomInDown
animate__rotateIn        animate__flipInX         animate__flipInY
animate__backInUp        animate__backInDown
animate__jackInTheBox    animate__rollIn
animate__lightSpeedInRight
```

### 退出动画（Exits）
```
animate__fadeOut         animate__fadeOutUp       animate__fadeOutDown
animate__fadeOutLeft     animate__fadeOutRight
animate__bounceOut       animate__bounceOutUp
animate__slideOutUp      animate__slideOutDown
animate__zoomOut         animate__zoomOutUp
animate__hinge           animate__rollOut
animate__lightSpeedOutRight
```

### 注意力动画（Attention）
```
animate__bounce          animate__flash           animate__pulse
animate__rubberBand      animate__shakeX          animate__shakeY
animate__headShake       animate__swing           animate__tada
animate__wobble          animate__jello           animate__heartBeat
```

### 翻转（Flippers）
```
animate__flip    animate__flipInX    animate__flipInY
animate__flipOutX   animate__flipOutY
```

---

## 效果 1：JS 动态触发动画

```js
// 添加动画，结束后移除类（可重复触发）
function animateEl(el, animation) {
  return new Promise(resolve => {
    el.classList.add('animate__animated', `animate__${animation}`);
    el.addEventListener('animationend', () => {
      el.classList.remove('animate__animated', `animate__${animation}`);
      resolve();
    }, { once: true });
  });
}

// 用法
await animateEl(document.querySelector('.btn'), 'bounce');
await animateEl(document.querySelector('.modal'), 'fadeInUp');
```

---

## 效果 2：通知弹窗进出

```js
async function showNotification(el) {
  el.style.display = 'block';
  await animateEl(el, 'fadeInDown');
  await new Promise(r => setTimeout(r, 3000));  // 显示 3 秒
  await animateEl(el, 'fadeOutUp');
  el.style.display = 'none';
}
```

---

## 效果 3：按钮点击反馈

```js
document.querySelectorAll('.btn').forEach(btn => {
  btn.addEventListener('click', () => animateEl(btn, 'rubberBand'));
});
```

---

## 效果 4：滚动触发入场

```js
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('animate__animated', 'animate__fadeInUp');
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.1 });

document.querySelectorAll('.section-content').forEach(el => observer.observe(el));
```

---

## 速度和延迟控制

```css
/* CSS 变量控制全局时长 */
:root { --animate-duration: 1s; }

/* 修改特定元素 */
.fast-bounce { --animate-duration: 0.5s; }

/* 修饰符类 */
.animate__faster  /* 0.5× 速度 */
.animate__fast    /* 0.8× 速度 */
.animate__slow    /* 2× 速度 */
.animate__slower  /* 3× 速度 */

/* 延迟修饰符 */
.animate__delay-1s  .animate__delay-2s  .animate__delay-3s
.animate__delay-4s  .animate__delay-5s
```

```js
// JS 修改单个元素速度
el.style.setProperty('--animate-duration', '0.3s');

// 全局降速（无障碍支持）
document.documentElement.style.setProperty('--animate-duration', '2s');
```
