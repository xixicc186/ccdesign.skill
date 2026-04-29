# Lax.js 动效参考

**本地库路径：** `frontend_design_repos/lax.js/lib/lax.min.js`

```html
<script src="frontend_design_repos/lax.js/lib/lax.min.js"></script>
```

**适用场景：** 滚动视差、多层速度差、背景漂移、鼠标跟随视差、精灵图序列

---

## 基础用法

```js
window.onload = function () {
  lax.init();

  lax.addDriver('scrollY', () => window.scrollY);

  lax.addElements('.hero-bg', {
    scrollY: {
      translateY: [
        [0, 500],       // 驱动值范围（scrollY 从 0 到 500）
        [0, -100]       // 对应输出（translateY 从 0 到 -100px）
      ]
    }
  });
};
```

---

## 效果 1：元素随滚动入场 + 出场

```js
lax.addElements('.card', {
  scrollY: {
    translateX: [
      ['elInY', 'elCenterY', 'elOutY'],   // 进入视口 → 居中 → 离开
      [0, 'screenWidth/2', 'screenWidth'],
      { easing: 'easeInOutQuart' }
    ],
    opacity: [
      ['elInY', 'elCenterY', 'elOutY'],
      [0, 1, 0],
      { easing: 'easeInOutCubic' }
    ]
  }
});
```

**特殊值常量：**
- `elInY` — 元素进入视口时的 scrollY
- `elCenterY` — 元素中心对齐视口中心时的 scrollY
- `elOutY` — 元素离开视口时的 scrollY
- `screenWidth` / `screenHeight` — 视口尺寸
- `pageWidth` / `pageHeight` — 文档尺寸
- `index` — 元素在批次中的索引

---

## 效果 2：多层视差（背景 + 前景差速）

```js
lax.addDriver('scrollY', () => window.scrollY);

// 背景层（慢速移动）
lax.addElements('.parallax-bg', {
  scrollY: {
    translateY: [[0, 1000], [0, -150]]
  }
});

// 前景层（快速移动）
lax.addElements('.parallax-fg', {
  scrollY: {
    translateY: [[0, 1000], [0, -400]]
  }
});

// 内容层（固定不动，覆盖在上面）
lax.addElements('.parallax-content', {
  scrollY: { translateY: [[0, 1000], [0, 0]] }
});
```

---

## 效果 3：鼠标跟随视差

```js
lax.init();

document.addEventListener('mousemove', (e) => {
  lax.__cursorX = e.clientX;
  lax.__cursorY = e.clientY;
});

lax.addDriver('cursorX', () => lax.__cursorX || 0);
lax.addDriver('cursorY', () => lax.__cursorY || 0);

lax.addElements('.floating-el', {
  cursorX: {
    translateX: [[0, 'screenWidth'],  ['index * 20', 'index * -20']]
  },
  cursorY: {
    translateY: [[0, 'screenHeight'], ['index * 20', 'index * -20']]
  }
});
```

---

## 效果 4：惯性动画（物理感）

```js
// 启用惯性驱动
lax.addDriver('scrollY', () => window.scrollY, { inertiaEnabled: true });

lax.addElements('.card', {
  scrollY: {
    rotateX: [
      [0],
      [0],
      { inertia: -1 }  // 负数 = 反向惯性
    ],
    translateY: [
      [0],
      [0],
      { inertia: -1 }
    ],
    'box-shadow': [
      [0],
      [0],
      {
        inertia: -1,
        cssFn: (val) => `0px ${Math.abs(val)}px 30px rgba(0,0,0,0.2)`
      }
    ]
  }
});
```

---

## 效果 5：自定义 CSS 函数（复杂属性）

```js
// filter: hue-rotate
lax.addElements('.colorful-bg', {
  scrollY: {
    filter: [
      [0, 1000],
      [0, 360],
      { cssFn: (val) => `hue-rotate(${val % 360}deg)` }
    ]
  }
});

// 动态 box-shadow
lax.addElements('.floating-card', {
  scrollY: {
    'box-shadow': [
      ['elInY+200', 'elCenterY', 'elOutY-200'],
      [50, 0, 50],
      {
        easing: 'easeInOutQuint',
        cssFn: (val) => `${val}px ${val}px ${val}px rgba(0,0,0,0.5)`
      }
    ]
  }
});
```

---

## 效果 6：响应式断点映射

```js
// 根据屏幕宽度使用不同的值
lax.addElements('.responsive-el', {
  scrollY: {
    translateX: [
      ['elInY', 'elCenterY', 'elOutY'],
      {
        500:  [10, 20, 50],   // 屏幕宽 < 500
        900:  [30, 40, 60],   // 500 - 900
        1400: [50, 70, 100]   // > 900
      }
    ]
  }
});
```

---

## 效果 7：精灵图序列动画

```js
const frameWidth = 370;
const frameCount = 12;

lax.addElements('.sprite', {
  scrollY: {
    'background-position': [
      [0, 1e9],
      [0, 1e9],
      {
        cssFn: (val) => {
          const frame = Math.floor(val / 10) % frameCount;
          return `-${frame * frameWidth}px 0px`;
        }
      }
    ]
  }
});
```

---

## 效果 8：onUpdate 自定义回调

```js
lax.addElements('#counter', {}, {
  onUpdate: function (driverValues, domElement) {
    const scrollY = driverValues.scrollY[0];
    const count = Math.floor(scrollY / 5);
    domElement.textContent = count;
    domElement.classList.toggle('highlight', scrollY > 500);
  }
});
```

---

## 支持的 CSS 属性

| 属性 | 说明 |
|------|------|
| `opacity` | 透明度 |
| `translateX/Y/Z` | 平移 |
| `rotate/rotateX/Y` | 旋转 |
| `scale/scaleX/Y` | 缩放 |
| `skewX/Y` | 倾斜 |
| `blur` | 模糊（需 cssFn） |
| `hue-rotate` | 色相旋转（需 cssFn） |
| 任意 CSS 属性 | 需要 cssFn 处理 |

## 缓动函数

```
easeInQuad    easeOutQuad    easeInOutQuad
easeInCubic   easeOutCubic   easeInOutCubic
easeInQuart   easeOutQuart   easeInOutQuart
easeInQuint   easeOutQuint   easeInOutQuint
easeOutBounce easeInBounce
easeOutBack   easeInBack
```

---

## 效果 9：modValue 循环旋转（齿轮 / 波浪）

**modValue** 让输出值取模循环，适合做无限旋转齿轮、循环波浪。

```js
// 齿轮随滚动无限旋转
lax.addElements('.gear-big', {
  scrollY: {
    rotate: [
      [0, 1e9],
      [0, 360],
      { modValue: 360 }   // 超过 360 自动折回 0，无缝循环
    ]
  }
});

// 反向小齿轮（啮合感）
lax.addElements('.gear-small', {
  scrollY: {
    rotate: [
      [0, 1e9],
      [0, -540],          // 更快反转，体现齿比
      { modValue: 360 }
    ]
  }
});

// 波浪位移循环
lax.addElements('.wave-item', {
  scrollY: {
    translateY: [
      [0, 1e9],
      [0, 80],
      {
        modValue: 80,
        cssFn: (val, el) => {
          const offset = parseInt(el.dataset.waveOffset || 0);
          return (val + offset) % 80 - 40;   // 相位偏移制造波浪
        }
      }
    ]
  }
});
```

---

## 效果 10：多驱动器融合（滚动 + 鼠标组合视差）

同一元素同时响应 scrollY 和 cursorX/Y，制造立体深度感。

```js
lax.init();
lax.addDriver('scrollY', () => window.scrollY);
lax.addDriver('cursorX', () => lax.__cx || window.innerWidth / 2);
lax.addDriver('cursorY', () => lax.__cy || window.innerHeight / 2);

document.addEventListener('mousemove', e => {
  lax.__cx = e.clientX;
  lax.__cy = e.clientY;
});

lax.addElements('.depth-card', {
  scrollY: {
    translateY: [['elInY', 'elOutY'], [60, -60], { easing: 'easeOutQuad' }],
    scale: [['elInY', 'elCenterY'], [0.85, 1]]
  },
  cursorX: {
    rotateY: [[0, 'screenWidth'], [-8, 8]]   // 鼠标左右 → 卡片前后倾斜
  },
  cursorY: {
    rotateX: [[0, 'screenHeight'], [5, -5]]  // 鼠标上下 → 卡片俯仰
  }
});
```

---

## 效果 11：惯性 + 动态阴影（速度感）

滚动速度越快，阴影越大；停止后弹回，赋予物理感。

```js
lax.addDriver('scrollY', () => window.scrollY, { inertiaEnabled: true });

lax.addElements('.speed-card', {
  scrollY: {
    translateY: [[0], [0], { inertia: -0.5 }],
    'box-shadow': [
      [0],
      [0],
      {
        inertia: -1,
        cssFn: (val) => {
          const abs = Math.abs(val);
          const blur = Math.min(abs * 2, 60);
          const spread = Math.min(abs * 0.5, 15);
          return `0px ${val}px ${blur}px -${spread}px rgba(0,0,0,0.4)`;
        }
      }
    ],
    scaleX: [
      [0],
      [1],
      {
        inertia: -0.5,
        cssFn: (val) => 1 + Math.abs(val) * 0.002   // 高速时轻微横向压扁
      }
    ]
  }
});
```

---

## 效果 12：onUpdate 动态内容（数字 + DOM 联动）

onUpdate 在每一帧都调用，可直接操作 DOM，不受 CSS 属性限制。

```js
// 随滚动递增的数字计数器
lax.addElements('.live-counter', {}, {
  onUpdate(driverValues, el) {
    const scrollY = driverValues.scrollY[0];
    const elTop = el.getBoundingClientRect().top + window.scrollY;
    const progress = Math.max(0, Math.min(1, (scrollY - elTop + 300) / 400));
    const target = parseInt(el.dataset.target || 100);
    el.textContent = Math.floor(progress * target).toLocaleString();
  }
});

// 进度条宽度（非 CSS 属性无法直接驱动时用 onUpdate）
lax.addElements('.custom-progress', {}, {
  onUpdate(driverValues, el) {
    const scrollY = driverValues.scrollY[0];
    const pct = Math.min(scrollY / (document.body.scrollHeight - window.innerHeight) * 100, 100);
    el.style.width = pct + '%';
    el.style.background = `hsl(${pct * 1.2}, 70%, 55%)`;  // 颜色随进度变化
  }
});
```

---

## 效果 13：水平滚动视差（横向 Snap）

配合 CSS `scroll-snap` 实现水平 story 切换，背景差速漂移。

```html
<div class="h-scroll-wrap">
  <section class="h-slide" data-lax-anchor="self">...</section>
  <section class="h-slide" data-lax-anchor="self">...</section>
</div>
```

```css
.h-scroll-wrap {
  display: flex;
  overflow-x: scroll;
  scroll-snap-type: x mandatory;
  -webkit-overflow-scrolling: touch;
}
.h-slide {
  flex-shrink: 0;
  width: 100vw; height: 100vh;
  scroll-snap-align: start;
  position: relative; overflow: hidden;
}
```

```js
lax.addDriver('scrollX', () => {
  return document.querySelector('.h-scroll-wrap').scrollLeft;
});

// 注册水平滚动更新（lax 默认只监听 window，需手动触发）
document.querySelector('.h-scroll-wrap').addEventListener('scroll', () => {
  lax.update(performance.now());
});

lax.addElements('.slide-bg', {
  scrollX: {
    translateX: [
      [0, 'screenWidth * 3'],
      [0, -'screenWidth * 0.4']   // 背景比前景慢 0.4x
    ]
  }
});
```
