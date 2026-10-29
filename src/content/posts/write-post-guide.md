---
title: 这样写文章就够了
published: 2026-10-28
description: 在流萤里写文章的方法：一个 Markdown 文件，头部写标题、日期、标签和分类，正文直接写 Markdown。
tags: [教程]
category: 技术
slug: write-post-guide
---

## 写文章

在 `src/content/posts/` 里新建一个 Markdown 文件，头部写标题、日期、标签和分类，正文直接写 Markdown。

```markdown
---
title: 文章标题
published: 2026-10-28
description: 一句话简介
tags: [标签]
category: 分类
---

正文直接写 Markdown。
```

## 本地预览和生成

```bash
# 本地预览
pnpm dev

# 生成静态文件
pnpm build
# 生成结果在 dist/ 里，直接用任何静态服务器就能打开
```

## 代码块

```python
print("hello, firefly")
```

## 图片

图片建议走图床或对象存储，不要全塞进 Git 仓库，仓库会越变越大。

## 评论

静态站本身没有评论，后面可以接 Waline，读者不用注册 GitHub 也能留言。
