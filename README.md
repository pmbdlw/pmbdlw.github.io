# pmbdlw.github.io

个人博客，基于 [GitHub Pages](https://pages.github.com/) + [Jekyll](https://jekyllrb.com/) 构建，主题为官方支持主题 [jekyll-theme-modernist](https://github.com/pages-themes/modernist)。

线上地址：<https://pmbdlw.github.io>

## 目录结构

```
_config.yml      站点配置（标题、主题、Markdown 解析、插件）
index.md         首页，自动列出全部文章
404.html         404 页面
_layouts/
  post.html      文章布局（标题、日期、返回首页）
_posts/          文章目录，文件名必须是 YYYY-MM-DD-标题.md
assets/img/      图片资源
```

## 写一篇新文章

在 `_posts/` 下新建文件，文件名格式为 `YYYY-MM-DD-英文短标题.md`（日期和短横线不能省略，否则 Jekyll 不会把它识别为文章）：

```markdown
---
title: "文章标题"
date: 2026-01-01
categories: 开发规范
tags: [标签一, 标签二]
---

正文从这里开始，使用 GitHub 风格 Markdown。

* 目录
{:toc}
```

要点：

- **文件名用 ASCII**，中文标题写在 front matter 的 `title` 里，这样生成的链接才干净（`/posts/英文短标题/`）。
- 自动生成目录用 `* 目录` + `{:toc}`，**不要用 `[TOC]`**，kramdown 不认识后者，会原样显示。
- 引用图片：把图片放进 `assets/img/`，文件名用 ASCII，然后 `![说明](/assets/img/图片名.png)`。

提交并推送到 `master` 分支后，GitHub Pages 会自动重新构建，大约 1 分钟后生效。

## 本地预览（可选）

```bash
bundle install
bundle exec jekyll serve
# 打开 http://127.0.0.1:4000
```

## License

见 [LICENSE](LICENSE)。
