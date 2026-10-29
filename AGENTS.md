# AGENTS.md —— 哈基蜂的小站（写作与维护规范）

> 给 AI 智能体看的操作手册。**按本文件做即可，不需要问人。**
> 面向人类的中文说明在 [docs/WRITING.md](docs/WRITING.md)。

## 1. 这个仓库是什么

这不是「主题模板仓库」，**这是正在跑的博客本体**。

| 事实 | 值 |
|---|---|
| 站点 | 哈基蜂的小站（二次元风格研究笔记） |
| 线上地址 | https://toprain-bow.github.io/ |
| 仓库 | `TopRain-bow/TopRain-bow.github.io`（公开，用户名站点） |
| 默认分支 | `main` —— **就是源码分支，没有单独的部署分支** |
| 技术栈 | Astro 7.3.2 + Firefly v6.16.8（Fuwari 二开）+ Svelte 5 |
| 部署 | 推 `main` → `.github/workflows/pages.yml` → Node 24 + pnpm 11.22.0 → `pnpm build` → GitHub Pages |
| 包管理器 | **只能用 pnpm**（`preinstall` 有 `only-allow pnpm`），Node >= 22.23 |

**push 到 `main` 就等于发布**，没有别的发布动作，也不需要手动上传构建产物。

## 2. 写一篇文章 = 加一个 md 文件

### 2.1 位置与文件名

- 目录：`src/content/posts/`
- 扩展名：`.md`（需要 MDX 组件时用 `.mdx`）
- **文件名就是网址**：`src/content/posts/my-first-post.md` → `https://toprain-bow.github.io/posts/my-first-post/`
- 文件名规则：**小写英文 + 连字符**，`^[a-z0-9]+(?:-[a-z0-9]+)*$`。不要用中文、空格、下划线、大写。
  （技术上中文文件名能构建，但会生成百分号编码的丑陋 URL，且分享/SEO 都吃亏。）
- 文件名发布后**视为不可变**：改名等于旧链接 404。

### 2.2 frontmatter 字段表

schema 定义在 `src/content.config.ts`（`postsCollection`）。**未知字段会被静默丢弃**（例如 `slug:` 写了也没用，URL 只看文件名）。

| 字段 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|
| `title` | string | **是** | — | 文章标题 |
| `published` | date | **是** | — | 发布日期，写 `2026-10-29`。加不加引号都行（schema 用了 `z.coerce.date()`） |
| `updated` | date | 否 | — | 更新日期 |
| `description` | string | 否 | `""` | 摘要，显示在列表卡片与搜索结果 |
| `tags` | string[] | 否 | `[]` | 标签，支持多个 |
| `category` | string | 否 | `""` | 分类，**自由字符串**，不用提前注册 |
| `draft` | boolean | 否 | `false` | `true` 时**生产构建不会输出该文章**（本地 dev 仍可见） |
| `image` | string | 否 | `""` | 封面图。放 `public/assets/images/` 下写 `/assets/images/xxx.webp` |
| `pinned` | boolean | 否 | `false` | 置顶 |
| `author` | string | 否 | `""` | 覆盖默认作者 |
| `sourceLink` | string | 否 | `""` | 转载来源 |
| `licenseName` / `licenseUrl` | string | 否 | `""` | 覆盖默认许可 |
| `comment` | boolean | 否 | `true` | 是否允许评论 |
| `password` / `passwordHint` | string | 否 | `""` | 加密文章 |
| `series` / `seriesOrder` | string / number | 否 | `""` | 系列文章归组与排序 |
| `lang` | string | 否 | `""` | 文章语言覆盖 |

**注意不存在「未来时间自动隐藏」**：`published` 写未来日期照样会发出来。要压着不发就用 `draft: true`。

### 2.3 最小可用模板

```markdown
---
title: 文章标题
published: 2026-10-29
description: 一句话摘要，会显示在列表卡片上
tags: [随想]
category: 生活
draft: false
---

正文从这里开始，正常写 Markdown。
```

### 2.4 分类约定

分类是自由字符串，但为了分类页不碎片化，**沿用已有分类，不要随手造新的**：

- 已有：`生活`、`技术`、`学习`、`折腾`、`随想`
- 确实需要新分类时：同时更新 `public/admin/config.yml` 里 `category` 字段的 `options` 列表，保持网页后台的下拉选项和实际数据一致。

### 2.5 三种写入入口（产物完全一样，都是那个 md 文件）

| 入口 | 适合谁 | 怎么做 |
|---|---|---|
| 本地写 | 有开发环境 | `pnpm new-post my-first-post` 生成骨架 → 编辑 → `git push` |
| 网页后台 `/admin` | 人类在浏览器/手机上 | https://toprain-bow.github.io/admin/ → PAT 登录 → 新建 → 填表 → Publish |
| GitHub 网页 | 应急、手机 | 在 GitHub 上 `src/content/posts/` → Add file → 贴模板 → Commit |

**智能体不要用 `/admin`**，那是给人用的浏览器界面。直接写文件 + commit 最省事。

## 3. 发布与验证流程（固定，不要自创）

```bash
# 1. 写/改文章
# 2. 本地校验（有环境时强烈建议）
pnpm install          # 首次；之后可跳过
pnpm build            # 构建成功才算改对
# 3. 提交并发布
git add -A
git commit -m "post: add my-first-post"
git push origin main
# 4. 等 Actions（约 1–2 分钟）
gh run list --repo TopRain-bow/TopRain-bow.github.io --limit 3
# 5. 线上实测复核（必做）
curl -sS -o NUL -w "%{http_code}\n" https://toprain-bow.github.io/posts/my-first-post/
```

**「Actions 显示 Success」不等于「文章上线了」。** 必须以线上 HTTP 200 + 页面里能看到标题为准。
历史上出过一次事故：构建任务失败但没人复核，线上一直停在旧版。

## 4. 排错

| 现象 | 原因 | 处理 |
|---|---|---|
| Actions 卡在 `pnpm install` | 依赖或 registry 问题 | 看 run 日志；`.npmrc` 里是 registry 配置 |
| `astro build` 报 schema 校验错 | frontmatter 类型不对（如 `tags` 写成字符串） | 对照 §2.2 修正 |
| 文章 404 但构建成功 | `draft: true`，或文件名写错 | 检查 draft 与文件名 |
| 页面 CSS/图片 404 | `base` 或路径写成绝对路径 | 静态资源引用以 `/` 开头指向 `public/` |
| push 被拒 `Recursive`/`Connection reset` | 到 github.com 的网络抖动 | **重试**，不要怀疑配置 |

不要为了修样式去改 `src/styles/liquid-glass.css` 以外的大面积主题 CSS。

## 5. 不要做的事

- 不要改线上 HTML 产物：仓库里没有构建产物，线上是 Actions 生成的，改不到也不该改。
- 不要提交 `node_modules/`、`dist/`、`.astro/`（已在 `.gitignore`）。
- 不要提交任何 token、密码、`.env`。VNDB 的 `apiToken` 必须留空。
- 不要改 `siteConfig.site_url`（`https://TopRain-bow.github.io`）和 `astro.config.mjs` 的 `base: "/"`——这是用户名站点，路径就在根上。
- 不要动 DNS、不要动 `time.yuangao.icu`（那是另一个旧副本）。
- 不要改站点标题/副标题/作者/壁纸模式这些既定配置，除非用户明确要求。

## 6. 主题开发（次要）

只在需要改主题行为时才展开本节。

- 结构：`src/pages`（路由）、`src/layouts`、`src/components`、`src/config`、`src/utils`、`src/plugins`、`src/styles`
- 配置：`src/config/*.ts`，经 `src/config/index.ts` 聚合；类型在 `src/types`
- 命令：`pnpm dev`（4321）、`pnpm check`、`pnpm type-check`、`pnpm lint`、`pnpm format`、`pnpm build`
- 风格：Biome，tab 缩进、双引号；组件 PascalCase，配置 `xxxConfig.ts`，工具 kebab-case
- 提交信息：Conventional Commits（`feat:` / `fix:` / `post:` / `chore:`）
- 构建管线：LQIP → VNDB 封面 → astro build → 裁剪 pio 资源 → 字体子集 → 内联脚本压缩 → Pagefind
