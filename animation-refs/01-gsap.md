# GSAP 动效参考

**本地库路径：** `frontend_design_repos/award-winning-website/node_modules/gsap/dist/`

```html
<script src="frontend_design_repos/award-winning-website/node_modules/gsap/dist/gsap.min.js"></script>
<script src="frontend_design_repos/award-winning-website/node_modules/gsap/ScrollTrigger.js"></script>
```

**适用场景：** 高端品牌网站、Awwwards 级别、clipPath 变形、scrub 滚动绑定、Timeline 序列、pin 钉选

---

## 效果 1：ClipPath 多边形 → 矩形（随滚动展开）

视频/图片从梯形裁剪形状随滚动还原为全屏矩形。

```js
gsap.registerPlugin(ScrollTrigger);

gsap.set('#video-frame', {
  clipPath: 'polygon(14% 0%, 72% 0%, 90% 90%, 0% 100%)',
  borderRadius: '0 0 40% 10%'
});

gsap.from('#video-frame', {
  clipPath: 'polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)',
  borderRadius: '0 0 0 0',
  ease: 'power1.inOut',
  scrollTrigger: {
    trigger: '#video-frame',
    start: 'center center',
    end: 'bottom center',
    scrub: true  // scrub = 动画进度绑定到滚动进度
  }
});
```

---

## 效果 2：文字逐词 3D 旋转飞入

每个词从"身体后方"旋转飞入，配合 ScrollTrigger 触发。

**CSS（必须先设置初始状态）：**
```css
.animated-word {
  opacity: 0;
  transform: translate3d(10px, 51px, -60px) rotateY(60deg) rotateX(-40deg);
  transform-origin: 50% 50% -150px; /* 关键：锚点在元素身后 */
  will-change: opacity, transform;
}
```

**JS：**
```js
const tl = gsap.timeline({
  scrollTrigger: {
    trigger: '.animated-title',
    start: '100 bottom',
    end: 'center bottom',
    toggleActions: 'play none none reverse'
  }
});

tl.to('.animated-word', {
  opacity: 1,
  transform: 'translate3d(0, 0, 0) rotateY(0deg) rotateX(0deg)',
  ease: 'power2.inOut',
  stagger: 0.02
});
```

**React 版本（带 context 清理）：**
```js
useEffect(() => {
  const ctx = gsap.context(() => {
    const tl = gsap.timeline({
      scrollTrigger: { trigger: containerRef.current, start: '100 bottom', end: 'center bottom', toggleActions: 'play none none reverse' }
    });
    tl.to('.animated-word', { opacity: 1, transform: 'translate3d(0,0,0) rotateY(0deg) rotateX(0deg)', ease: 'power2.inOut', stagger: 0.02 });
  }, containerRef);
  return () => ctx.revert();
}, []);
```

---

## 效果 3：图片蒙版展开 + Pin 钉选

图片从小圆形随滚动扩大到全屏，滚动时页面固定不动。

```js
useGSAP(() => {
  const tl = gsap.timeline({
    scrollTrigger: {
      trigger: '#clip',
      start: 'center center',
      end: '+=800 center',
      scrub: 0.5,
      pin: true,         // 固定页面
      pinSpacing: true   // 保留滚动空间
    }
  });
  tl.to('.mask-clip-path', {
    width: '100vw',
    height: '100vh',
    borderRadius: 0
  });
});
```

---

## 效果 4：鼠标跟随 3D 旋转（无需库）

```js
const handleMouseMove = (e) => {
  const rect = el.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;
  const rotateX = ((y - rect.height / 2) / rect.height) * -10;
  const rotateY = ((x - rect.width  / 2) / rect.width ) *  10;
  gsap.to(el, { duration: 0.3, rotateX, rotateY, transformPerspective: 500, ease: 'power1.inOut' });
};
const handleMouseLeave = () => gsap.to(el, { duration: 0.3, rotateX: 0, rotateY: 0, ease: 'power1.inOut' });
```

---

## 效果 5：元素视差悬浮（容器 + 内容反向）

```js
const handleMouseMove = ({ clientX, clientY, currentTarget }) => {
  const rect = currentTarget.getBoundingClientRect();
  const xOffset = clientX - (rect.left + rect.width  / 2);
  const yOffset = clientY - (rect.top  + rect.height / 2);
  if (isHovering) {
    gsap.to(sectionRef.current, { x: xOffset, y: yOffset, rotationY: xOffset / 2, rotationX: -yOffset / 2, transformPerspective: 500, duration: 1, ease: 'power1.out' });
    gsap.to(contentRef.current, { x: -xOffset, y: -yOffset, duration: 1, ease: 'power1.out' });
  }
};
```

---

## 效果 6：导航栏滚动隐显

```js
// 监听滚动方向
let lastScrollY = 0;
window.addEventListener('scroll', () => {
  const isNavVisible = window.scrollY < lastScrollY || window.scrollY === 0;
  lastScrollY = window.scrollY;
  gsap.to(navEl, { y: isNavVisible ? 0 : -100, opacity: isNavVisible ? 1 : 0, duration: 0.2 });
});
```

---

## 效果 7：视频切换缩放（点击触发）

```js
// 新视频从中心放大到全屏
gsap.set('#next-video', { visibility: 'visible' });
gsap.to('#next-video', {
  transformOrigin: 'center center',
  scale: 1, width: '100%', height: '100%',
  duration: 1, ease: 'power1.inOut',
  onStart: () => nextVdRef.current.play()
});
// 当前视频缩小消失
gsap.from('#current-video', {
  transformOrigin: 'center center',
  scale: 0, duration: 1.5, ease: 'power1.inOut'
});
```

---

## 效果 8：进度条逐帧更新（gsap.ticker）

```js
const anim = gsap.to(progressSpan, {
  onUpdate: () => {
    const progress = Math.ceil(anim.progress() * 100);
    gsap.to(progressSpan, { width: `${progress}%`, backgroundColor: 'white' });
  },
  onComplete: () => gsap.to(progressSpan, { backgroundColor: '#afafaf' })
});

// 将视频播放时间映射到动画进度
const animUpdate = () => anim.progress(video.currentTime / video.duration);
gsap.ticker.add(animUpdate);
// 清理：gsap.ticker.remove(animUpdate)
```

---

## 效果 9：3D 模型视图切换（Timeline）

```js
export const animateWithGsapTimeline = (timeline, rotationRef, rotationState, from, to, props) => {
  timeline.to(rotationRef.current.rotation, { y: rotationState, duration: 1, ease: 'power2.inOut' });
  timeline.to(from, { ...props, ease: 'power2.inOut' }, '<'); // '<' = 与上一个同时开始
  timeline.to(to,   { ...props, ease: 'power2.inOut' }, '<');
};
```

---

## 效果 10：滚动触发基础工具函数

```js
export const animateWithGsap = (target, animationProps, scrollProps) => {
  gsap.to(target, {
    ...animationProps,
    scrollTrigger: {
      trigger: target,
      toggleActions: 'restart reverse restart reverse',
      start: 'top 85%',
      ...scrollProps
    }
  });
};

// 使用示例
animateWithGsap('.g_grow', { scale: 1, opacity: 1, ease: 'power1' }, { scrub: 5.5 });
animateWithGsap('.g_text', { y: 0, opacity: 1, ease: 'power2.inOut', duration: 1 });
```

---

## ScrollTrigger 常用配置速查

```js
// 基础触发
scrollTrigger: { trigger: '#el', start: 'top 80%', toggleActions: 'play pause reverse restart' }

// scrub 滚动联动（动画进度 = 滚动进度）
scrollTrigger: { trigger: '#el', start: 'center center', end: 'bottom center', scrub: true }

// pin 钉选（滚动时固定元素）
scrollTrigger: { trigger: '#el', start: 'center center', end: '+=800 center', scrub: 0.5, pin: true, pinSpacing: true }

// 慢速联动（5.5秒延迟过渡）
scrollTrigger: { trigger: '#el', scrub: 5.5 }
```

## 常用 Easing

- `power1.inOut` — 轻微（视频、导航）
- `power2.inOut` — 中等（文字、旋转）
- `power1.out` — 快速起始（视差）
- `ease-in-out` — 通用
