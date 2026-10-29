# 哈基蜂的小站

Astro + Firefly 博客，部署在 GitHub Pages（https://TopRain-bow.github.io/）。

- 改文章：`src/content/posts/*.md`
- 改配置：`src/config/*.ts`；玻璃样式在 `src/styles/liquid-glass.css`
- 构建：`pnpm install && pnpm build`（Node >= 22.23，pnpm 11.22）
- 部署：push 到 main，`.github/workflows/pages.yml` 自动构建并部署 dist
- 详细交接见 `交接说明.md`
