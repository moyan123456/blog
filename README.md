# moyan的网站

个人博客与数字花园，用来记录代码、工具和折腾日常。

**🌐 线上地址：** [https://moyan123456.github.io](https://moyan123456.github.io)

![站点预览](public/og.png)

> 基于 [astro-devosfera](https://github.com/0xdres/astro-devosfera)（AstroPaper 的深度定制版）构建，已做界面汉化与个性化改造。

---

## 目录

1. [特性](#-特性)
2. [技术栈](#-技术栈)
3. [本地开发](#-本地开发)
4. [常用命令](#-常用命令)
5. [目录结构](#-目录结构)
6. [写文章](#-写文章)
7. [相册功能](#-相册功能)
8. [配置说明](#️-配置说明)
9. [部署](#-部署)
10. [本项目做的定制](#-本项目做的定制)
11. [致谢与许可](#-致谢与许可)

---

## ✨ 特性

- **类型安全的 Markdown / MDX**，构建时静态生成，速度快、响应式、可访问性好
- **完整 SEO**：meta 标签、Open Graph、sitemap、RSS、JSON-LD 结构化数据
- **明暗主题**切换，跟随系统
- **动态 OG 图**：分享到社交平台时自动显示标题卡片
- **全局搜索**：`Ctrl + K` 唤起，基于 Pagefind 静态索引，支持键盘导航
- **图片相册**：`/galleries`，构建时自动生成 srcset / WebP / 懒加载，带原生灯箱
- **代码高亮**：Shiki，支持行高亮、diff 标记、文件名显示
- **文章目录**：正文里写 `## Table of contents` 自动生成
- **纯静态部署**：推代码即发布，零服务器成本

## 🧰 技术栈

| 项目 | 说明 |
| :--- | :--- |
| 框架 | [Astro 6](https://astro.build/) |
| 样式 | Tailwind CSS 4 |
| 内容 | Markdown / MDX，`astro:content` 内容集合 |
| 搜索 | [Pagefind](https://pagefind.app/)（构建时生成索引） |
| 代码高亮 | Shiki |
| 日期处理 | dayjs（含 UTC / 时区插件，中文 locale） |
| 部署 | GitHub Pages + GitHub Actions |

## 🚀 本地开发

**环境要求：** Node.js 20+ 和 pnpm

```bash
# 1. 克隆仓库
git clone https://github.com/moyan123456/moyan123456.github.io.git
cd moyan123456.github.io

# 2. 安装依赖
pnpm install

# 3. 启动开发服务器
pnpm run dev
# → http://localhost:4321
```

### 搜索功能的本地调试

Pagefind 的搜索索引**只在生产构建时生成**，所以 `pnpm run dev` 下搜索是空的。想本地测搜索：

```bash
pnpm run build
pnpm run preview
```

## 🧞 常用命令

| 命令 | 作用 |
| :--- | :--- |
| `pnpm install` | 安装依赖 |
| `pnpm run dev` | 启动本地开发服务器（`localhost:4321`） |
| `pnpm run build` | 生产构建（`astro check` + 构建 + 生成 Pagefind 索引） |
| `pnpm run preview` | 预览生产构建结果 |
| `pnpm run format` | 用 Prettier 格式化代码 |
| `pnpm run lint` | 用 ESLint 检查代码 |

> `pnpm run build` 内部会执行 `pagefind --site dist && cp -r dist/pagefind public/`，搜索索引最终落在 `public/pagefind/`。

## 📁 目录结构

```
/
├── public/                  # 静态资源（og.png、favicon、pagefind 索引等）
├── src/
│   ├── assets/              # 本地字体、SVG 图标、logo
│   ├── components/          # 可复用组件（Header、Footer、Card…）
│   ├── data/
│   │   ├── blog/            # 文章 .md / .mdx
│   │   └── galleries/       # 相册（一个相册一个文件夹）
│   ├── layouts/             # 页面布局
│   ├── pages/               # 路由
│   ├── styles/              # 全局样式
│   └── utils/               # 工具函数
├── astro.config.ts          # Astro 配置
└── src/config.ts            # 站点配置（标题、简介、时区、功能开关）
```

## 📝 写文章

在 `src/data/blog/` 下新建 `.md` 或 `.mdx` 文件：

```markdown
---
title: "文章标题"
pubDatetime: 2026-11-07T20:00:00+08:00   # 必填，带时区的 ISO 8601
description: "一句话简介，用于 SEO 和列表卡片"
tags: ["随笔", "折腾"]
featured: false    # 是否在首页「精选」展示
draft: false       # 草稿，生产环境不显示
---

正文内容……
```

**MDX**：可以直接写 JSX 组件，无需 import 即可使用 `<GalleryEmbed>`。

**自动目录**：在正文里单独写一行 `## Table of contents`，会根据标题自动生成目录。

**代码块标注**（Shiki transformer 语法）：

```
// [!code highlight]      → 高亮该行
// [!code ++]             → 标记为新增行
// [!code --]             → 标记为删除行
// fileName: file.ts      → 在代码块上方显示文件名
```

## 🖼️ 相册功能

1. 在 `src/data/galleries/<相册别名>/` 建文件夹
2. 放入 `index.md`（相册元信息）和图片文件
3. 文件名加数字前缀（`01-`、`02-`…）可控制显示顺序
4. 访问路径：`/galleries/<相册别名>`

也可以在任何 `.mdx` 文章里内嵌相册，**无需 import**：

```mdx
<GalleryEmbed slug="my-trip-to-tokyo" />
```

可选参数：`limit`（`0` 表示全部）、`cols`（`2 | 3 | 4`）、`showLink`（`true/false`）。

## ⚙️ 配置说明

站点配置集中在 `src/config.ts` 的 `SITE` 常量里，常用的有：

| 字段 | 说明 |
| :--- | :--- |
| `website` | 站点地址，用于 canonical / sitemap / OG |
| `title` | 站点标题，显示在导航和浏览器标签 |
| `desc` | 站点简介 |
| `author` | 作者名 |
| `timezone` | 默认时区，当前 `Asia/Shanghai` |
| `lang` | HTML 语言，当前 `zh-CN` |
| `postPerIndex` / `postPerPage` | 首页 / 列表页每页文章数 |
| `showArchives` / `showGalleries` | 是否显示归档 / 相册入口 |
| `introAudio` | 首页音频播放器（当前已关闭） |
| `dynamicOgImage` | 是否自动生成文章分享图 |

### 社交链接

社交链接通过**环境变量**注入，不在代码里硬编码，避免泄露个人信息。

| 变量 | 作用 |
| :--- | :--- |
| `PUBLIC_SOCIAL_GITHUB` | GitHub 主页链接 |
| `PUBLIC_SOCIAL_X` | X / Twitter |
| `PUBLIC_SOCIAL_LINKEDIN` | LinkedIn |
| `PUBLIC_SOCIAL_EMAIL` | 联系邮箱（渲染成 `mailto:` 链接） |
| `PUBLIC_EDIT_POST_URL` | 「编辑本文」按钮的跳转地址 |

**未设置某个变量 = 自动隐藏对应图标**，不会报错也不会出现空链接。

本地开发时：

```bash
cp .env.example .env
# 在 .env 里填自己的值
```

线上（GitHub Pages）在 `.github/workflows/deploy.yml` 的构建步骤里配置：

```yaml
      - name: Build site
        run: pnpm build
        env:
          PUBLIC_SOCIAL_EMAIL: "你的邮箱@example.com"
```

## 🌍 部署

本项目部署在 **GitHub Pages**，完全自动化：

1. 推送到 `main` 分支
2. GitHub Actions 自动运行 `.github/workflows/deploy.yml`
3. 依次执行：安装依赖 → `astro check` → 构建 → 生成 Pagefind → 上传产物 → 发布

```bash
git add -A
git commit -m "写新文章"
git push
```

约 1–2 分钟上线，进度在仓库的 **Actions** 标签页可见。

> 注意：仓库名是 `moyan123456.github.io`，属于**用户站点**，所以访问路径就是根路径 `/`，无需配置 `base`。

## 🎨 本项目做的定制

- 界面全站**中文化**（导航、按钮、页脚、搜索、分页、面包屑、日期格式等）
- Logo 改为 `{T}` 风格，品牌字 **MoYan**
- 首页文案与终端提示改为个人化内容
- 页脚、分享、社交提示文案中文化
- 自定义社交分享图 `public/og.png`
- 时区改为 `Asia/Shanghai`，日期格式为「2026年11月7日」
- 移除主题自带的示例文章与示例相册
- 关闭主题自带的海外电台播放器

## 🙏 致谢与许可

- 原始主题：[astro-devosfera](https://github.com/0xdres/astro-devosfera) by [0xdres](https://github.com/0xdres)
- 上游主题：[AstroPaper](https://github.com/satnaing/astro-paper) by [Sat Naing](https://satnaing.dev)

基于 MIT 许可证发布，详见 [LICENSE](LICENSE)。
