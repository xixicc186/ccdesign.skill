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

---

## 效果 11：Flip — DOM 状态流体切换

任何 DOM 变化（重排、添加删除、类名切换）都能自动动画化，是"布局动画"的终极解决方案。

```html
<script src="frontend_design_repos/award-winning-website/node_modules/gsap/Flip.js"></script>
```

```js
gsap.registerPlugin(Flip);

// 1. 捕捉当前状态
const state = Flip.getState('.cards');

// 2. 改变 DOM（任意方式：重排、换容器、改 class）
grid.classList.toggle('list-view');
// 或：container2.appendChild(el);
// 或：el.classList.add('expanded');

// 3. 动画回新状态
Flip.from(state, {
  duration: 0.6,
  ease: 'power2.inOut',
  stagger: 0.05,
  absolute: true  // 更精确的位置控制
});

// 让一个元素 fit 另一个的位置和大小（点击展开效果）
Flip.fit('#card', '#hero-slot', {
  scale: true,
  duration: 0.5,
  ease: 'expo.inOut'
});
```

**适合场景：** 商品列表网格/列表切换、购物车弹出动画、看板卡片移动、筛选后的重排

---

## 效果 12：MotionPath — 沿路径运动

元素沿 SVG 路径或坐标数组飞行，支持自动旋转对齐方向。

```html
<script src="frontend_design_repos/award-winning-website/node_modules/gsap/MotionPathPlugin.js"></script>
```

```js
gsap.registerPlugin(MotionPathPlugin);

// 沿 SVG <path> 运动
gsap.to('.rocket', {
  duration: 3,
  motionPath: {
    path: '#flight-path',   // SVG path 元素的选择器
    autoRotate: true,       // 自动旋转朝向路径方向
    alignOrigin: [0.5, 0.5] // 元素中心对齐路径
  },
  ease: 'power1.inOut'
});

// 沿坐标数组运动（不需要 SVG）
gsap.to('.particle', {
  duration: 2,
  motionPath: {
    path: [
      { x: 0,   y: 0   },
      { x: 100, y: 150 },
      { x: 300, y: 50  },
      { x: 400, y: 200 }
    ],
    curviness: 1.5,   // 曲线平滑度（0=折线，2=非常弯曲）
    autoRotate: 45    // 自动旋转 + 额外偏移角度
  }
});
```

**适合场景：** 产品展示飞入动画、游戏角色移动、路线图可视化、Logo 演绎

---

## 效果 13：Draggable — 完整拖拽系统

带惯性投掷、边界约束、碰撞检测的拖拽。

```html
<script src="frontend_design_repos/award-winning-website/node_modules/gsap/Draggable.js"></script>
```

```js
gsap.registerPlugin(Draggable);

// 基础拖拽（带边界）
Draggable.create('.card', {
  type: 'x,y',
  bounds: '.container',
  edgeResistance: 0.65,
  onPress()   { gsap.to(this.target, { scale: 1.05, boxShadow: '0 20px 40px rgba(0,0,0,0.3)' }); },
  onRelease() { gsap.to(this.target, { scale: 1,    boxShadow: 'none' }); }
});

// 带惯性投掷（松手后继续滑动）
Draggable.create('.throwable', {
  inertia: true,
  onThrowComplete() { console.log('落点:', this.x, this.y); }
});

// 旋转拖拽（拨号盘、转盘）
Draggable.create('.dial', {
  type: 'rotation',
  snap: (v) => Math.round(v / 30) * 30,  // 磁吸到 30° 倍数
  onDrag() { updateDisplay(this.rotation); }
});

// 碰撞检测
if (Draggable.hitTest('.card', '.dropzone', '50%')) {
  // 卡片进入放置区超过 50% 时触发
}
```

**适合场景：** 拖拽排序、拨号盘、可配置仪表盘、卡片游戏

---

## 效果 14：Observer — 统一手势/滚动检测

统一处理鼠标/触摸/滚轮，非常适合全屏分页切换。

```html
<script src="frontend_design_repos/award-winning-website/node_modules/gsap/Observer.js"></script>
```

```js
gsap.registerPlugin(Observer);

// 全屏分页翻页（上下滑动切换）
let currentSection = 0;
const sections = document.querySelectorAll('.section');

Observer.create({
  type: 'wheel,touch,pointer',
  wheelSpeed: -1,
  onDown: () => goTo(currentSection - 1),
  onUp:   () => goTo(currentSection + 1),
  tolerance: 10,
  preventDefault: true
});

function goTo(index) {
  if (index < 0 || index >= sections.length) return;
  gsap.to(sections[currentSection], { yPercent: -100, duration: 1, ease: 'power2.inOut' });
  gsap.from(sections[index],        { yPercent:  100, duration: 1, ease: 'power2.inOut' });
  currentSection = index;
}
```

**适合场景：** 全屏分页网站、移动端滑动交互、禁用默认滚动的自定义体验

---

## 效果 15：TextPlugin — 打字机 / 文字替换

文字逐字显示，或平滑替换为新内容。

```html
<script src="frontend_design_repos/award-winning-website/node_modules/gsap/TextPlugin.js"></script>
```

```js
gsap.registerPlugin(TextPlugin);

// 打字机效果
gsap.to('#tagline', {
  duration: 2,
  text: 'Design that moves people.',
  ease: 'none'
});

// 逐词显示（更自然）
gsap.to('#subtitle', {
  duration: 3,
  text: { value: 'Build once. Ship everywhere.', delimiter: ' ' }
});

// 数字滚动计数器
gsap.to('#counter', {
  duration: 2,
  text: '1,000,000',
  ease: 'power2.out'
});

// 文字循环切换（配合 Timeline）
const words = ['Fast.', 'Beautiful.', 'Yours.'];
const tl = gsap.timeline({ repeat: -1 });
words.forEach(word => {
  tl.to('#word', { duration: 0.5, text: word })
    .to('#word', { duration: 0.3, text: '', delay: 1 });
});
```

**适合场景：** Hero 标语动画、加载提示文字、数据展示计数

---

## 效果 16：RoughEase / SlowMo — 抖动与变速缓动

EasePack 提供两个特别有创意的缓动：

```js
// RoughEase：抖动/颤抖（设备震动、紧张感）
gsap.to('.warning', {
  duration: 0.5,
  x: 10,
  ease: 'rough({ points: 30, strength: 2, taper: "out" })'
});

// SlowMo：开始慢 → 快 → 结束慢（舞台感）
gsap.to('.spotlight', {
  duration: 3,
  x: 400,
  ease: 'slow(0.7, 0.7, false)'
  // slow(linearRatio, power, yoyo)
  // linearRatio: 中间匀速段比例
});

// CustomEase：完全自定义曲线（从贝塞尔或 SVG 路径）
gsap.registerPlugin(CustomEase);
CustomEase.create('myBounce', 'M0,0 C0.14,0 0.242,0.438 0.272,0.561 0.313,0.728 0.354,0.963 0.362,1 0.37,0.985 0.414,0.873 0.455,0.811 0.51,0.726 0.573,0.753 0.586,0.762 0.683,0.812 0.717,1 0.726,1 0.74,1 0.75,1 1,1');
gsap.to('.ball', { duration: 1.5, y: 200, ease: 'myBounce' });
```

**适合场景：** RoughEase → 错误提示抖动、游戏受击；SlowMo → 舞台入场、产品揭幕

---

## 效果 17：ScrollTrigger 滚动速度 skewY

滚动越快，元素倾斜越大，停下来时弹回，产生极强的速度感。

```js
let proxy = { skew: 0 };
const skewSetter = gsap.quickSetter('img', 'skewY', 'deg');
const clamp = gsap.utils.clamp(-20, 20);

ScrollTrigger.create({
  onUpdate: (self) => {
    const skew = clamp(self.getVelocity() / 300);
    if (proxy.skew !== skew) {
      proxy.skew = skew;
      gsap.to(proxy, {
        skew: 0,
        duration: 0.8,
        ease: 'power3',
        overwrite: true,
        onUpdate: () => skewSetter(proxy.skew)
      });
    }
  }
});
gsap.set('img', { transformOrigin: 'right center', force3D: true });
```

---

## 效果 18：stagger grid — 网格从中心爆开

```js
gsap.from('.grid-item', {
  scrollTrigger: { trigger: '.grid', start: 'top 80%' },
  duration: 0.6,
  scale: 0,
  opacity: 0,
  stagger: {
    amount: 0.8,
    grid: 'auto',    // 自动检测网格行列
    from: 'center'   // 从中心向外扩散（也可以是 'edges', 'random'）
  }
});
```
