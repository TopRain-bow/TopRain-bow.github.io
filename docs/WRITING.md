# 怎么写文章（中文说明）

这个站有两种写文章的方式，产物完全一样——都是仓库里 `src/content/posts/` 下多一个 `.md` 文件。
推上去后 GitHub Actions 自动构建，约 1–2 分钟上线。

- **网页后台**：适合在浏览器、手机上随手写，不用装任何东西 → 见第 1 节
- **本地写**：适合长文、要插图片、要本地预览 → 见第 2 节

---

## 1. 网页后台（推荐日常用）

### 1.1 打开

https://toprain-bow.github.io/admin/

### 1.2 第一次：拿一个 GitHub 访问令牌（PAT）

后台需要你的授权才能把文章提交到仓库。令牌只保存在你自己浏览器的 localStorage 里，不会上传到别处。

1. 打开 https://github.com/settings/personal-access-tokens/new
2. **Token name**：填 `Sveltia CMS`
3. **Expiration**：按需，90 天或自定义
4. **Repository access** → 选 **Only select repositories** → 勾选 `TopRain-bow.github.io`
5. **Permissions** → 展开 **Repository permissions** → 找到 **Contents** → 改成 **Read and write**
   （只有这一项是必须的；其它保持 No access）
6. 页面最下面 **Generate token**，复制那串 `github_pat_...`
7. 回到 `/admin/`，粘贴进去登录

> 令牌只在登录时用一次。浏览器清缓存 / 换设备 / 换浏览器后要重新登录一次。
> 令牌丢了不用慌：回 GitHub 重新生成一个，旧的删掉即可。

### 1.3 写一篇

1. 左侧点 **文章** → 右上角 **新建文章**
2. 填表：
   - **标题**
   - **网址别名**（必填）：小写英文 + 连字符，例如 `my-first-post`。**这就是网址**：`/posts/my-first-post/`。发布后不要改。
   - **发布日期**：默认今天
   - **分类**：下拉选（生活 / 技术 / 学习 / 折腾 / 随想）
   - **标签**：可加多个
   - **摘要**：一两句话，显示在列表卡片上
   - **封面图**：可空；上传后自动放到 `public/assets/images/posts/`
   - **草稿**：勾上就不会发布
   - **正文**：Markdown，右侧有实时预览
3. 右上角 **Publish** → 直接提交到 `main` → 触发构建

### 1.4 手机上

同一个网址，手机浏览器打开即可，界面会自适应。
登录同样用上面的 PAT（手机上粘贴令牌，建议先复制到手机剪贴板）。

### 1.5 加新分类

分类下拉的选项写死在 `public/admin/config.yml` 的 `category.options` 里。
要加新分类，在那个列表里加一行，提交后刷新后台就有了。

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
| `published` | 是 | 日期，`2026-10-29` 这样写 |
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
- `draft: true` 了
- 文件名拼错，或者没推到 `main`
- Actions 还在跑（1–2 分钟），或者构建失败了

**改了文件名，旧链接 404？**
正常，文件名就是网址。要保留旧链接就保留旧文件，另写一篇新的。

**后台登录后又要求登录？**
清理浏览器数据、换设备都会这样，重新用 PAT 登录一次即可。

**忘记 PAT 了？**
去 GitHub 吊销旧的、生成新的，重新登录。

**想让 AI 帮你写？**
把仓库地址给智能体就行，写作规范写在仓库根目录的 [AGENTS.md](../AGENTS.md) 里。
