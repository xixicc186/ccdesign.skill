# Barba.js 动效参考

**本地库路径：** `frontend_design_repos/barba/packages/core/`（源码参考，实际使用需 npm install）

```bash
npm install @barba/core
```

**适用场景：** 多页面网站的页面切换过渡（SPA 效果）、作品集网站、需要流畅跳转感的营销站

---

## HTML 结构要求

```html
<body>
  <div data-barba="wrapper">
    <!-- 导航等全局内容放在这里 -->

    <main data-barba="container" data-barba-namespace="home">
      <!-- 页面内容 -->
    </main>
  </div>
</body>
```

---

## 效果 1：基础淡入淡出切换

```js
import barba from '@barba/core';

barba.init({
  transitions: [{
    name: 'fade',
    async leave(data) {
      await gsap.to(data.current.container, { opacity: 0, duration: 0.5 }).then();
    },
    async enter(data) {
      await gsap.from(data.next.container, { opacity: 0, duration: 0.5 }).then();
    }
  }]
});
```

---

## 效果 2：滑动切换（新页面从右侧滑入）

```js
barba.init({
  transitions: [{
    name: 'slide',
    async leave(data) {
      await gsap.to(data.current.container, {
        x: '-100%', opacity: 0, duration: 0.6, ease: 'power2.inOut'
      }).then();
    },
    async enter(data) {
      gsap.set(data.next.container, { x: '100%' });
      await gsap.to(data.next.container, {
        x: 0, duration: 0.6, ease: 'power2.inOut'
      }).then();
    }
  }]
});
```

---

## 效果 3：同步过渡（旧出新入同时）

```js
barba.init({
  transitions: [{
    name: 'sync-fade',
    sync: true,   // 关键：leave 和 enter 同时运行

    async leave(data) {
      return gsap.to(data.current.container, {
        opacity: 0, y: -20, duration: 0.4
      });
    },
    async enter(data) {
      gsap.set(data.next.container, { opacity: 0, y: 20 });
      return gsap.to(data.next.container, {
        opacity: 1, y: 0, duration: 0.4, delay: 0.2
      });
    }
  }]
});
```

---

## 效果 4：页面专属过渡（按命名空间匹配）

```js
barba.init({
  transitions: [
    // home → about：向上滑出
    {
      from: { namespace: 'home' },
      to:   { namespace: 'about' },
      async leave(data) {
        await gsap.to(data.current.container, { y: '-100%', duration: 0.6 }).then();
      },
      async enter(data) {
        gsap.set(data.next.container, { y: '100%' });
        await gsap.to(data.next.container, { y: 0, duration: 0.6 }).then();
      }
    },
    // 默认过渡（其他页面）
    {
      name: 'default',
      async leave(data) {
        await gsap.to(data.current.container, { opacity: 0, duration: 0.3 }).then();
      },
      async enter(data) {
        await gsap.from(data.next.container, { opacity: 0, duration: 0.3 }).then();
      }
    }
  ]
});
```

---

## 效果 5：首次加载动画（once）

```js
barba.init({
  transitions: [{
    name: 'initial',
    once(data) {
      // 首次进入页面的动画
      gsap.from(data.next.container, { opacity: 0, y: 30, duration: 0.8, ease: 'power2.out' });
    },
    async leave(data) { /* ... */ },
    async enter(data) { /* ... */ }
  }]
});
```

---

## 效果 6：全局钩子（每次切换都触发）

```js
barba.hooks.before(() => {
  // 每次切换前：显示加载条、禁用点击等
  document.querySelector('.page-loader').classList.add('active');
});

barba.hooks.after(() => {
  // 每次切换后：隐藏加载条、重新初始化第三方库等
  document.querySelector('.page-loader').classList.remove('active');
  ScrollReveal().sync();   // 重新初始化 ScrollReveal
});

barba.hooks.enter((data) => {
  window.scrollTo(0, 0);  // 切换后滚到顶部
});
```

---

## 过渡生命周期顺序

```
beforeOnce → once → afterOnce  （仅首次加载）

before → beforeLeave → leave → afterLeave
       → beforeEnter → enter → afterEnter → after  （每次切换）
```

---

## 注意事项

- 页面内第三方库（GSAP、ScrollReveal 等）的 `init()` 需在 `afterEnter` 中重新调用
- 使用 `sync: true` 时 leave 和 enter 并行，容器会同时存在于 DOM 中
- 导航链接需在 `data-barba="wrapper"` 内，否则不会触发 Barba 拦截
