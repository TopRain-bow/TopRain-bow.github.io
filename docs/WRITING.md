# 怎么写文章（中文说明）

写文章有两个入口，产物完全一样——都是仓库里 `src/content/posts/` 下多一个 `.md` 文件。
提交后 GitHub Actions 自动构建，约 1–2 分钟上线。

- **网页写**：浏览器 / 手机上直接写，写完点提交 → 见第 1 节
- **本地写**：适合长文、要本地预览 → 见第 2 节

---

## 1. 网页写（推荐日常用）

### 1.1 打开

https://toprain-bow.github.io/write/

### 1.2 第一次：粘一个 GitHub 令牌

浏览器里的静态页面没有权限往 GitHub 写东西，必须带一个属于你的凭据。**只粘一次**，令牌只存在你自己浏览器的 localStorage 里，不上传到任何地方。

1. 页面上点「创建令牌」，会打开 GitHub 的令牌创建页，权限已经预填成 `Contents: Read and write`
2. `Repository access` 选 **Only select repositories** → 勾 `TopRain-bow.github.io`
3. 点最下面 **Generate token**，复制 `github_pat_...`
4. 粘回写作页面，点「保存并连接」

> 令牌只在登录时用一次。清浏览器缓存 / 换设备 / 无痕模式下要重新粘一次。
> 令牌丢了就回 GitHub 重新生成一个，旧的删掉即可。

### 1.3 写一篇

1. 填标题
2. **网址别名**会自动从标题生成；中文标题会退化成 `post-20261029-1530` 这种，建议自己改成英文，例如 `my-first-post`。
   它就是网址：`/posts/my-first-post/`，**发布后别再改**。
3. 选分类（生活 / 技术 / 学习 / 折腾 / 随想）
4. 标签用逗号分隔，例如 `随笔, Astro`
5. 摘要填一两句，显示在列表卡片上
6. 正文写 Markdown
7. 点 **「提交并发布」**

页面会直接写进仓库 `main`，然后给你「打开文章」和「看构建进度」两个链接。

### 1.4 不想粘令牌？

点 **「不用令牌：在 GitHub 打开」**。它会把写好的内容塞进 GitHub 的「新建文件」页面，你在那边点一次 `Commit changes` 就发布了。
缺点：正文太长（几万字）时链接会被 GitHub 拒绝，这种情况用「提交并发布」。

### 1.5 手机上

同一个网址，手机浏览器打开即可，表单会自动适应窄屏。令牌需要先复制到手机剪贴板再粘贴。

### 1.6 加新分类

分类下拉的选项写在 `public/write/index.html` 里的 `<select id="fCategory">`。
要加就加一行 `<option value="新分类">新分类</option>`，提交后刷新页面就有。

---

## 2. 本地写

```bash
# 首次
pnpm install

# 新建一篇（会自动生成骨架和文件名）
pnpm new-post my-first-post

# 想在浏览器里边改边看
pnpm dev        # http://localhost:4321

# 改完先本地确认能构建
pnpm build
```

然后编辑 `src/content/posts/my-first-post.md`。

### 2.1 文件长这样

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

### 2.2 字段说明

| 字段 | 必填 | 说明 |
|---|---|---|
| `title` | 是 | 标题 |
| `published` | 是 | 日期，`2026-10-29` 这样写，加不加引号都行 |
| `description` | 否 | 摘要 |
| `tags` | 否 | 标签，`[随笔, Astro]` 这种写法 |
| `category` | 否 | 分类，一个字符串 |
| `draft` | 否 | `true` 时不会发布 |
| `image` | 否 | 封面图路径 |
| `pinned` | 否 | 置顶 |
| `series` / `seriesOrder` | 否 | 系列文章 |
| `password` | 否 | 给文章加密 |

文件名就是网址：`my-first-post.md` → `/posts/my-first-post/`。
所以**文件名只用小写英文和连字符**，别用中文，别在发布后改名。

### 2.3 发布

```bash
git add -A
git commit -m "post: add my-first-post"
git push origin main
```

等 1–2 分钟，Actions 会自动构建部署。

---

## 3. 发布后确认（别只看 Actions 显示绿灯）

打开 https://toprain-bow.github.io/posts/你的文件名/ ，能正常打开、标题对，才算真的上线。

构建记录在这里看：https://github.com/TopRain-bow/TopRain-bow.github.io/actions

---

## 4. 常见问题

**文章写了但站点上没有？**
- 勾了「存为草稿」
- 网址别名拼错，或者没提交成功（页面有报错信息）
- Actions 还在跑（1–2 分钟），或者构建失败了

**提示令牌无效 / 没有写权限？**
令牌需要满足两条：一是 `Repository access` 里勾了这个仓库，二是 `Permissions → Contents` 是 `Read and write`。
改完权限要重新生成一个新令牌（改权限不会让旧令牌生效）。

**网页页面完全打不开？**
`/write/` 是纯静态页面，不需要外部 CDN，打不开基本就是站点本身在重新部署，等一两分钟。

**改了文件名，旧链接 404？**
正常，文件名就是网址。要保留旧链接就别改名。

**想让 AI 帮你写？**
把仓库地址给智能体就行，写作规范写在仓库根目录的 [AGENTS.md](../AGENTS.md) 里。
