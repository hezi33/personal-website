# 技术方案文档 — 个人介绍网站

> **版本**: V1.0  
> **最后更新**: 2026-06-21  
> **状态**: 文档建设阶段  

---

## 1. 技术选型

### 1.1 总体架构

```
纯静态站点（Static Site）
├── 无构建工具、无框架
├── HTML5 + CSS3 + Vanilla JavaScript (ES6+)
└── 零运行时依赖（除字体 CDN）
```

### 1.2 选型理由

| 决策 | 选择 | 理由 |
|------|------|------|
| **框架** | 无框架，纯 HTML/CSS/JS | 单页内容有限，框架引入不必要的复杂度；HTML 文件可直接在浏览器打开 |
| **CSS 方案** | CSS 自定义属性（CSS Variables） | 原生支持、无构建步骤、继承 brand-dna 色板体系 |
| **字体** | Google Fonts CDN | 免费、CDN 加速、支持中英文字体子集化 |
| **动效** | IntersectionObserver + CSS transition | 原生 API、性能好、不依赖第三方库 |
| **模板** | 基于 `demo-landing.html` 改造 | 已有成熟模板，包含品牌变量和组件 |
| **部署** | GitHub Pages | 免费、HTTPS、自定义域名支持 |

---

## 2. CSS 架构

### 2.1 CSS 自定义属性（继承 brand-dna）

```css
:root {
  /* === 品牌三色 === */
  --blue: #2B7FD8;
  --blue-deep: #1E5BA8;
  --yellow: #F4D758;
  --yellow-soft: #FFF3CD;
  --red: #E84A5F;

  /* === 背景色 === */
  --cream: #fefcf6;
  --cream-dark: #faf6eb;

  /* === 文字色 === */
  --ink: #1A1A2E;
  --ink-light: #4A4A5A;
  --ink-faint: #8A8A9A;

  /* === 深色面板 === */
  --dark-panel: #151821;
}
```

### 2.2 样式组织

```
style 标签内（内联于 index.html）
├── 1. CSS 变量定义
├── 2. Reset（*, *::before, *::after）
├── 3. 基础元素（body, ::selection, .container）
├── 4. Nav 导航栏
├── 5. Hero 首屏
├── 6. 各 Section 样式（按页面顺序）
│   ├── Section 通用
│   ├── 关于我 Section
│   ├── 深色面板 Section
│   ├── 技能标签 Section
│   └── CTA Section
├── 7. Footer
├── 8. Scroll Reveal 动效
└── 9. 响应式媒体查询
```

### 2.3 Fluid Sizing 规范

所有尺寸使用 `clamp()` 实现流畅缩放，不打散为固定断点：

| 元素 | clamp 范围 |
|------|-----------|
| Hero 大标题 | `clamp(2.8rem, 7vw, 5.5rem)` |
| Section 标题 | `clamp(1.4rem, 3vw, 2.2rem)` |
| Section 间距 | `clamp(80px, 12vh, 160px)` |
| 卡片内 padding | `clamp(28px, 3vw, 40px)` |
| 内容最大宽度 | `1300px`（非 clamp，固定上限） |

---

## 3. 字体加载策略

### 3.1 Google Fonts 加载

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=...&display=swap" rel="stylesheet">
```

- 使用 `preconnect` 预热 DNS/TCP/TLS
- `display=swap` 确保文字在字体加载期间可见（FOIT → FOUT，体验更好）

### 3.2 字体栈

```css
/* 中文标题 */
font-family: 'Noto Serif SC', 'PingFang SC', 'STSong', 'Songti SC', serif;

/* 中文正文 */
font-family: 'Noto Sans SC', -apple-system, 'PingFang SC', 'Helvetica Neue', sans-serif;

/* 英文装饰标题 */
font-family: 'Fraunces', 'Georgia', 'Times New Roman', serif;

/* 代码/标签 */
font-family: 'Fira Code', 'SF Mono', 'Consolas', monospace;

/* 手写/注释 */
font-family: 'Caveat', 'KaiTi', 'STKaiti', cursive;
```

---

## 4. 动效方案

### 4.1 Scroll Reveal（IntersectionObserver）

```js
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      observer.unobserve(entry.target); // 只触发一次
    }
  });
}, { threshold: 0.12 }); // 元素进入视口 12% 时触发

document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
```

### 4.2 CSS Transition

```css
.reveal {
  opacity: 0;
  transform: translateY(32px);
  transition: opacity 0.7s cubic-bezier(0.16, 1, 0.3, 1),
              transform 0.7s cubic-bezier(0.16, 1, 0.3, 1);
}
.reveal.visible {
  opacity: 1;
  transform: none;
}
/* Stagger 延迟 */
.reveal-d1 { transition-delay: 0.1s; }
.reveal-d2 { transition-delay: 0.2s; }
.reveal-d3 { transition-delay: 0.3s; }
.reveal-d4 { transition-delay: 0.4s; }
.reveal-d5 { transition-delay: 0.5s; }
```

### 4.3 Nav 滚动效果

```js
window.addEventListener('scroll', () => {
  document.querySelector('nav').classList.toggle('scrolled', window.scrollY > 40);
});
```

### 4.4 动效禁止清单

- ❌ `bounce` / `elastic` 等夸张缓动
- ❌ `animate width/height`（触发 reflow）
- ❌ 无限循环动画（除轨道旋转等明确场景）
- ❌ `@keyframes` 中大量属性变化

---

## 5. 响应式方案

### 5.1 断点策略

| 断点 | 宽度 | 行为 |
|------|------|------|
| **Desktop** | > 900px | 完整多列布局 |
| **Tablet** | ≤ 900px | 三列→单列、间距缩小 |
| **Mobile** | ≤ 600px | 字号微缩、Nav 链接简化、CTA 按钮全宽 |

### 5.2 响应式原则

- **Adapt, don't amputate** — 移动端不隐藏内容，重新排列
- 图片/卡片网格用 `grid-template-columns` 改变列数
- CTA 按钮移动端 `width: 100%`
- 移动端 Hero 区域缩小 `min-height`

### 5.3 无障碍

```css
@media (prefers-reduced-motion: reduce) {
  .reveal {
    opacity: 1;
    transform: none;
    transition: none;
  }
}
```

---

## 6. 目录结构

```
personal-website/
├── index.html                     ← V1 主页面（单页个人介绍）
├── CLAUDE.md                      ← AI 开发铁律
├── DATA.yaml                      ← 个人信息数据
├── assets/
│   ├── avatar.jpg                 ← 头像图片（占位）
│   └── og-image.jpg               ← 社交分享预览图（后续）
├── docs/
│   ├── PRD.md                     ← 产品需求文档
│   ├── TECHNICAL_DESIGN.md        ← 技术方案文档（本文档）
│   └── PROCESS.md                 ← 进度追踪文档
├── portfolio/
│   └── index.html                 ← V2 作品集页（未来）
├── blog/
│   └── index.html                 ← V3 博客页（未来）
├── shared/
│   ├── brand.css                  ← 共享品牌样式（多页时抽取，未来）
│   └── components.css             ← 共享组件样式（多页时抽取，未来）
└── esther-design-system-main/     ← Esther 设计系统参考（只读）
    ├── brand-dna.md
    ├── references/
    └── assets/
```

### 6.1 V1 单文件策略

V1 阶段所有 CSS 内联于 `index.html` 的 `<style>` 标签内——避免额外的 HTTP 请求，且文件尚不复杂。

V2/V3 多页时，抽取 `shared/brand.css` 和 `shared/components.css` 供各页面共用。

---

## 7. 部署方案

### 7.1 GitHub Pages

```
仓库: <username>.github.io（用户主页仓库）
或: <username>.github.io/<repo>（项目仓库）

部署方式: Push 到 main 分支 → 自动部署
自定义域名: 可选，通过 CNAME 文件配置
```

### 7.2 部署检查清单

- [ ] 所有资源使用相对路径
- [ ] Google Fonts CDN 可正常访问
- [ ] `index.html` 在浏览器直接打开可正常显示
- [ ] 响应式在手机/平板正常
- [ ] 所有链接有效
- [ ] `<meta>` 标签完整（description, viewport, charset）

---

## 8. 扩展设计

### 8.1 从单页到多页的迁移路径

```
V1: index.html（内联所有样式）
    ↓ 当需要第二个页面时
V2: 抽取 shared/brand.css + shared/components.css
    index.html 和 portfolio/index.html 均引用 shared/*.css
    ↓ 当需要第三个页面时
V3: 考虑是否需要统一的 nav.js / reveal.js 脚本抽取
```

### 8.2 多页共享机制

| 共享内容 | 方式 |
|----------|------|
| CSS 变量（色板/字体） | `shared/brand.css` |
| 组件样式 | `shared/components.css` |
| Nav 栏 | 各页面复制 HTML 片段（或后续用 JS include） |
| Scroll Reveal | 各页面复制 JS 片段（或 `shared/reveal.js`） |
| 个人信息 | 从 `DATA.yaml` 读取（或编译时注入） |

---

## 9. 技术禁忌

| 类别 | 禁止 |
|------|------|
| **框架** | React / Vue / Tailwind / Bootstrap 等 CSS/JS 框架 |
| **构建** | Webpack / Vite / npm / node_modules |
| **CSS** | `!important` 滥用、固定 px 布局、`position: absolute` 复杂定位 |
| **JS** | jQuery、第三方动效库（AOS/GSAP 等）、inline event handler |
| **字体** | Inter / Roboto / Arial（overused） |
| **整体** | 看起来像 Vercel/Next.js 模板生成的页面 |
