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
