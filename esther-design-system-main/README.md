# Esther's Design Skill

一套给 AI 看的个人品牌设计系统。

把审美写成操作手册,AI 每次帮我做页面时必须翻这本手册,不能自由发挥。**限制 AI 的自由度 = 保证输出质量。**

---

## Demo

用这套系统生成的真实页面:

### 📖 教程型 - 分享会页面

信息清晰、步骤明确、有节奏的单页科普/教程。

🔗 [在线预览](https://esthersjw.github.io/cola-ob-sharing/cola-ob-sharing.html)

---

### 🎪 活动页 / Landing

视觉冲击、深浅面板交替、强节奏感的活动邀请页。

🔗 [在线预览](https://esthersjw.github.io/esther-design-system/demo-landing.html)

---

### 📱 App 型 / 功能型

功能优先、交互感、信息密度高的应用型页面。

🔗 [在线预览](https://hiesther.me/tutorials/personal-dashboard/)

---

### 📕 小红书图文卡片

3:4 比例、字大、手机可读、一键导出 PNG 的图文卡片。

🔗 [在线预览](https://hiesther.me/tutorials/esther-design-system/demo-readme-cards.html)

---

## 核心逻辑

```
SKILL.md(流程 - AI 按什么步骤干活)
    ↓
brand-dna.md + references/*(规范 - 能用什么不能用什么)
    ↓
assets/template-*.html(起点 - 从模板改,不从零写)
```

- AI 不能随便发明布局 → 只能从 15 种里选
- AI 不能随便用颜色 → 只能用三色 + 扩展规则
- AI 不能随便写样式 → 必须从组件库里选
- AI 做完要自检 → 对照 checklist 逐条过,P0 不过就打回

---

## 文件结构

```
esther-design-system/
├── SKILL.md                    ← 7步工作流(大脑)
├── brand-dna.md                ← 品牌基因:颜色/字体/气质/禁忌
├── assets/                     ← 模板骨架(起点)
│   ├── template-tutorial.html      教程页模板
│   ├── template-landing.html       活动页模板
│   ├── template-app.html           App型模板
│   ├── template-cards.html         小红书卡片模板
│   ├── html2canvas.min.js          卡片导出依赖
│   └── avatar.jpg                  IP头像
└── references/                 ← 规则和零件(知识库)
    ├── layouts.md                  15种布局模式(附完整代码)
    ├── components.md               组件库(20+组件,完整HTML+CSS)
    ├── checklist.md                质量检查清单(P0/P1/P2)
    ├── scene-tutorial.md           教程场景规范
    ├── scene-landing.md            活动页场景规范
    ├── scene-app.md                App型场景规范
    └── scene-cards.md              小红书卡片场景规范
```

---

## 7 步工作流

AI 每次做设计必须按这个顺序走:

| # | 做什么 | 为什么 |
|---|--------|--------|
| 1 | 问 5 个问题(类型/受众/几屏/素材/约束) | 不自作主张 |
| 2 | 读 brand-dna + 对应场景文件 | 先学规矩再动手 |
| 3 | 从 assets/ 复制对应模板 | 从半成品开始,不从零写 |
| 4 | 从 layouts.md 选 3-5 种布局 | 每个 section 不能一样 |
| 5 | 从 components.md 选组件 | 禁止用 HTML 默认样式 |
| 6 | 对照 checklist 自检 | P0 不过就打回 |
| 7 | 交付 HTML 文件 | 浏览器打开就能看 |

---

## 品牌基因速览

### 三色

| 颜色 | 色值 | 比例 | 对应 |
|------|------|------|------|
| 蓝 | `#2B7FD8` | 60% | 头发 |
| 黄 | `#F4D758` | 30% | 裙子 |
| 红 | `#E84A5F` | 10% | 蝴蝶结/鞋 |

### 字体

| 用途 | 字体 |
|------|------|
| 中文标题 | 汇文明朝体 / Noto Serif SC |
| 中文正文 | Noto Sans SC |
| 英文装饰 | Fraunces italic |
| 手写/注释 | Caveat |
| 代码/终端 | Fira Code |

### 气质关键词

可爱但有品质 · 手绘蜡笔感 · 有温度 · **不像 AI** · 一看就是"不二的"

### 禁忌

蓝紫渐变 · glassmorphism · neon · bounce 动画 · Inter/Roboto · 所有 section 居中 · HTML 默认样式 · 看起来像 AI 生成的通用模板

---

## 组件库

30+ 经过验证的可复用组件，每个都是写好的 HTML+CSS 代码，AI 直接复制使用：

🔗 [组件库全览](https://hiesther.me/tutorials/esther-design-system/components-preview.html)

**卡片** — 杂志裁切 / 编号主导 / 标签主导 / 侧边Icon / 引用块风格

**引用块** — 极简竖线 / 杂志大引号 / 手写批注 / 荧光笔高亮 / 终端命令风

**代码面板** — 暗色终端 / 亮色 snippet / diff 对比 / 多 Tab

**更多** — 流程步骤条 · Do/Don’t 对比 · Pull Quote · 对话气泡 · 导航栏 · 日历网格 · 书卡 · 机票卡 · 住宿卡 · Tab 切换 · 手风琴 · 翻转卡片 · 头像集群 · 对比表 · Modal 滚动阅读 ……

---

## 质量检查

**P0(必须全过)**

品牌三色比例 · 无禁忌元素 · 无 HTML 默认样式 · 暖底背景 · 衬线+无衬线混搭 · 响应式 · 每 section 布局不同 · clamp() fluid sizing · 截图发 Twitter 不会被说"又是 AI 做的"

**P1(应过)**

至少一个视觉惊喜 section · 字号对比极端 · Scroll Reveal 动效 · 大装饰数字/英文

**P2(加分)**

图片溢出容器 · 深色面板打破节奏 · 装饰元素克制 · prefers-reduced-motion

---

## 怎么用

发这个链接给你的 AI Agent(推荐使用 [Cola](https://colaos.ai)):

```
https://github.com/esthersjw/esther-design-system
```

然后跟它说:

> 帮我把这个设计系统克隆下来,并且引导我调整成我自己的设计风格和 IP 头像。

核心不是这些文件本身,是**你的审美判断力**。文件只是把你的判断写成了 AI 能执行的规则。

---

## 关于

Made by **Esther不二** - 在 AI 时代认真生活的女生

这套设计系统由 [Cola](https://colaos.ai) 协助构建。Cola 是首个有灵魂的操作系统。

灵感来源:归藏 op7418 的 PPT Design Skill
