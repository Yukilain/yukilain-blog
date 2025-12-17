# Mizuki 博客主题项目总结

## 1. 项目概述

Mizuki 是一个现代化、功能丰富的静态博客模板，使用 [Astro](https://astro.build) 构建，具有高级功能和精美的设计。它提供了完整的博客功能，包括文章管理、特殊页面（如动漫追踪、友人帐、日记等）、评论系统、搜索功能等。

### 技术栈

- **框架**: Astro 5.15.3
- **语言**: TypeScript 5.9.2
- **样式**: Tailwind CSS
- **构建工具**: pnpm >= 9
- **部署**: 支持 Vercel、Netlify、GitHub Pages、Cloudflare Pages 等静态托管平台

## 2. 核心功能特点

### 🎨 设计与界面
- 现代化响应式设计，适配所有设备
- 亮色/暗色主题切换，支持系统偏好检测
- 自定义主题颜色和动态横幅轮播
- 使用 Swup 实现流畅的页面过渡动画
- 精美的排版，使用 JetBrains Mono 字体

### 🔍 内容与搜索
- 基于 Pagefind 的高级搜索功能
- 增强的 Markdown 支持，包括语法高亮
- 自动滚动的交互式目录
- RSS 订阅生成
- 阅读时间估算
- 文章分类和标签系统

### 📱 特殊页面
- **动漫追踪页** - 记录动漫观看进度和评分
- **友人帐页面** - 展示朋友网站的精美卡片
- **日记页面** - 分享生活瞬间，类似社交媒体
- **归档页面** - 文章的时间线视图
- **关于页面** - 可自定义的个人介绍

### 🛠 技术特性
- 基于 Expressive Code 的增强代码块
- 使用 KaTeX 渲染的数学公式支持
- 图片优化，集成 PhotoSwipe 画廊
- SEO 优化，包括站点地图和元标签
- 性能优化，包括懒加载和缓存
- 集成 Twikoo 评论系统

## 3. 技术架构

### 项目结构

```
├── public/            # 静态资源目录
├── src/              # 源代码目录
│   ├── components/   # 组件
│   ├── content/      # 内容
│   ├── data/         # 数据
│   ├── layouts/      # 布局
│   ├── pages/        # 页面
│   ├── styles/       # 样式
│   ├── types/        # 类型定义
│   ├── utils/        # 工具函数
│   └── config.ts     # 主配置文件
├── scripts/          # 脚本
└── package.json      # 项目依赖
```

### 组件架构

Mizuki 使用模块化的组件架构，主要组件包括：

- **布局组件**: Layout.astro, MainGridLayout.astro
- **导航组件**: Navbar.astro, Footer.astro
- **内容组件**: PostCard.astro, PostMeta.astro, PostPage.astro
- **特殊组件**: ArchivePanel.svelte, Search.svelte, MobileTOC.svelte
- **评论组件**: Twikoo 集成

### 配置系统

项目使用统一的配置文件 `src/config.ts`，支持：

- 站点信息配置
- 主题颜色和样式配置
- 横幅图片配置
- 社交链接配置
- 功能页面配置
- 组件布局配置

## 4. 快速开始

### 安装步骤

1. **克隆仓库**
   ```bash
   git clone https://github.com/matsuzaka-yuki/mizuki.git
   cd mizuki
   ```

2. **安装依赖**
   ```bash
   # 安装 pnpm（如果尚未安装）
   npm install -g pnpm
   
   # 安装项目依赖
   pnpm install
   ```

3. **配置博客**
   - 编辑 `src/config.ts` 自定义博客设置
   - 更新站点信息、主题颜色、横幅图片和社交链接
   - 配置功能页面

4. **启动开发服务器**
   ```bash
   pnpm dev
   ```
   博客将在 `http://localhost:4321` 可用

### 常用命令

| 命令                    | 操作                                   |
|:---------------------------|:-----------------------------------------|
| `pnpm install`             | 安装依赖                     |
| `pnpm dev`                 | 启动本地开发服务器 `localhost:4321` |
| `pnpm build`               | 构建生产版本到 `./dist/`       |
| `pnpm preview`             | 本地预览构建版本  |
| `pnpm check`               | 运行 Astro 错误检查                 |
| `pnpm format`              | 使用 Biome 格式化代码                   |
| `pnpm lint`                | 检查并修复代码问题                |
| `pnpm new-post <filename>` | 创建新博客文章                   |

## 5. 目录结构详解

### public/
存放静态资源，包括图片、字体、JavaScript 文件等。

- **assets/**: 图片资源，包括桌面和移动横幅、动漫图片等
- **favicon/**: 网站图标
- **images/**: 相册、设备、日记等页面使用的图片
- **js/**: 前端 JavaScript 文件

### src/
源代码目录，包含所有核心功能实现。

- **components/**: 可复用组件
- **content/**: 博客内容，包括文章和特殊页面配置
- **data/**: 特殊页面的数据，如动漫列表、项目列表等
- **layouts/**: 页面布局模板
- **pages/**: 页面路由和实现
- **styles/**: CSS 和 Stylus 样式文件
- **types/**: TypeScript 类型定义
- **utils/**: 工具函数
- **config.ts**: 主配置文件

### scripts/
辅助脚本，用于自动化任务。

- **compress-fonts.js**: 压缩字体文件
- **init-content-repo.js**: 初始化内容仓库
- **new-post.js**: 创建新博客文章
- **sync-content.js**: 同步内容

## 6. 配置说明

### 主配置文件 (src/config.ts)

主配置文件包含站点的所有核心设置：

```typescript
export const siteConfig: SiteConfig = {
  title: "Your Blog Name",
  subtitle: "Your Blog Description",
  lang: "en", // 语言
  themeColor: {
    hue: 210, // 主题色调 0-360
    fixed: false, // 是否固定主题颜色
  },
  banner: {
    enable: true,
    src: ["assets/banner/1.webp"], // 横幅图片
    carousel: {
      enable: true,
      interval: 0.8, // 轮播间隔（秒）
    },
  },
  // 更多配置...
};
```

### 内容管理

- **创建新文章**: 使用 `pnpm new-post <filename>` 命令
- **编辑文章**: 修改 `src/content/posts/` 目录下的文件
- **自定义特殊页面**: 编辑 `src/content/spec/` 目录下的文件

### 文章 Frontmatter

文章使用 YAML Frontmatter 配置元数据：

```yaml
---
title: My First Blog Post
published: 2023-09-09
description: This is the first post of my new blog.
image: ./cover.jpg
tags: [tag1, tag2]
category: Frontend
draft: false
pinned: false
lang: en      # 仅当文章语言与站点语言不同时设置
---
```

## 7. Markdown 扩展

Mizuki 支持标准 GitHub Flavored Markdown 之外的增强功能：

### 增强写作
- **标注**: 使用 `> [!NOTE]`, `> [!TIP]`, `> [!WARNING]` 等创建精美的注释框
- **数学公式**: 使用 `$inline$` 和 `$$block$$` 语法编写 LaTeX 数学公式
- **代码高亮**: 具有行号和复制按钮的高级语法高亮
- **GitHub 卡片**: 使用 `::github{repo="user/repo"}` 嵌入仓库卡片

### 视觉元素
- **图片画廊**: 自动集成 PhotoSwipe 用于图片查看
- **可折叠部分**: 创建可展开的内容块
- **自定义组件**: 使用特殊指令增强内容

## 8. 部署方法

Mizuki 支持部署到任何静态托管平台：

### Vercel
1. 连接 GitHub 仓库到 Vercel
2. Vercel 会自动检测 Astro 项目并配置构建
3. 点击部署

### Netlify
1. 从 GitHub 导入项目
2. 配置构建命令：`pnpm build`
3. 配置发布目录：`dist`
4. 点击部署

### GitHub Pages
1. 使用包含的 GitHub Actions 工作流
2. 配置 GitHub Pages 来源为 `gh-pages` 分支
3. 推送代码后自动部署

### Cloudflare Pages
1. 连接 GitHub 仓库
2. 配置构建命令：`pnpm build`
3. 配置发布目录：`dist`
4. 点击部署

## 9. 性能与优化

### 已实现的优化

- **组件懒加载**: 按需加载组件，提高初始加载速度
- **图片优化**: 使用 WebP 格式，支持懒加载
- **代码分割**: 按页面分割代码，减少不必要的加载
- **缓存策略**: 配置适当的缓存头
- **减少第三方依赖**: 仅包含必要的依赖

### 建议的优化

- **CDN 加速**: 使用 CDN 分发静态资源
- **压缩资源**: 确保所有资源都经过压缩
- **监控性能**: 使用 Lighthouse 定期检查性能
- **减少重绘**: 优化 CSS，减少不必要的重绘和回流

## 10. 贡献指南

欢迎贡献！您可以通过以下方式参与项目：

1. Fork 仓库
2. 创建特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 打开 Pull Request

## 11. 许可证与致谢

### 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

### 致谢

- 基于原始的 [Fuwari](https://github.com/saicaca/fuwari) 模板
- 使用 [Astro](https://astro.build) 和 [Tailwind CSS](https://tailwindcss.com) 构建
- 灵感来自 [Yukina](https://github.com/WhitePaper233/yukina) - 一个精美优雅的博客模板
- 图标来自 [Iconify](https://iconify.design/)

## 12. 更新日志摘要

### v6.5 更新
- 代码块可折叠功能
- 仓库分离支持
- 全新的前端布局管理系统
- 网站 URL 配置
- 布局切换功能
- 性能优化和 bug 修复

### v6.0 更新
- 完全重构多个页面（动漫、时间线、项目、技能、相册、朋友、日记、关于）
- 页面切换功能，支持 SEO 优化
- 新的网格文章列表布局
- 涟漪效果管理模块

### v5.0 更新
- 集成 Pio Live2D 角色
- 高度可配置的设置
- 使用 Swup 的无缝导航

## 13. 未来发展方向

Mizuki 项目仍在积极开发中，未来可能的发展方向包括：

- 更多主题和布局选项
- 增强的社交媒体集成
- 更好的移动端体验
- 更多第三方服务集成
- 改进的性能和可访问性

---

如果您觉得这个项目有帮助，请考虑给它一个星标！⭐

[**🖥️ 在线演示**](https://mizuki.mysqil.com/)
[**📝 文档**](https://docs.mizuki.mysqil.com/)