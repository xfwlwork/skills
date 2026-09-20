---
name: black-white-minimal-html
description: "Use when writing black-white minimal line-style HTML."
version: 1.0.0
author: Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [html, css, minimalist, monochrome, editorial, diagrams]
---

# 黑白线条极简 HTML 风格

仅在用户对 HTML 的描述适配以下方向时使用：极简、开发者工具、技术报告、研究笔记、架构说明、黑白、线框、编辑式排版、克制的信息可视化。

用户明确要求其他风格时，优先满足用户要求；此风格作为参考库，不覆盖用户的品牌规范、色彩、交互和版式指令。

## 视觉语言

- 画布：纯白或近白 `#fff`，黑色正文 `#101010`，浅灰 `#f4f4f2` 作为弱层级。
- 结构：1–2px 黑色边框、水平分割线、矩形卡片、细箭头；避免圆润大卡片、厚阴影、渐变与装饰插画。
- 背景：可选极浅网格（约 28px 间距、5% 黑色透明度），只作技术纸张质感。
- 字体：大标题采用粗体无衬线；正文和数据采用等宽字体。中英文混排选择系统安全字体栈。
- 信息层级：章节编号、短 eyebrow、巨型主标题、紧凑导航、明确小标题。内容密度由留白和规则线控制。
- 图表：优先内联 SVG。使用方框、线条、箭头、单色条形、标签与注释；禁止依靠色彩区分类别。

## 页面骨架

1. `masthead`：小型元信息 + 巨型标题 + 右侧规格 stamp。
2. 粘连或横向滚动的锚点导航。
3. 各节采用 `border-bottom` 分隔，统一上下留白。
4. 核心解释使用并列线框卡片。
5. 流程、架构、特征提取优先使用 SVG 并确保移动端可横向滚动。
6. 媒体使用黑色框或细线框，保留来源和简短图注。
7. 末尾保留来源与边界说明。

## CSS 起点

```css
:root { --ink:#101010; --paper:#fff; --muted:#6a6a6a; --wash:#f4f4f2; }
body { background:var(--paper); color:var(--ink); font:15px/1.6 ui-monospace, Menlo, Consolas, "Noto Sans SC", monospace; }
.section { padding:50px 0; border-bottom:1px solid var(--ink); }
.box { border:1px solid var(--ink); padding:20px; background:rgba(255,255,255,.82); }
.fill { background:var(--ink); color:#fff; }
```

## 可用性与验证

- 响应式：小屏幕将多栏收为单栏；宽 SVG 容器提供横向滚动。
- 对比度：黑白文字、边界与图表标签须保持清晰。
- 媒体：使用真实 URL 或本地可交付文件，标明来源；视频提供 `controls` 和回退文本。
- HTML 生成后应实际使用浏览器渲染并截图检查文字重叠、图表裁切、媒体加载状态和异常空白。
- 交付 HTML 文件，并在需要时提供 PNG 预览。
