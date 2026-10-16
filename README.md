# pmbdlw.github.io

个人博客，基于 [GitHub Pages](https://pages.github.com/) + [Jekyll](https://jekyllrb.com/) 构建，主题为官方支持主题 [jekyll-theme-modernist](https://github.com/pages-themes/modernist)。

线上地址：<https://pmbdlw.github.io>

## 目录结构

```
_config.yml      站点配置（标题、主题、Markdown 解析、插件）
index.md         首页，自动列出全部文章和分类
404.html         404 页面
_layouts/
  post.html      文章布局（标题、日期、分类、返回首页）
_posts/          已发布的文章，文件名必须是 YYYY-MM-DD-标题.md
_drafts/         草稿，不会发布（见下文）
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

## 草稿：`_drafts/`

`_drafts/` 是 Jekyll 的内置约定 —— **这个目录里的文件不会出现在线上站点**，即使提交推送到 GitHub 也一样。适合写一半先放着的文章。

和 `_posts/` 的区别：

| | `_posts/` | `_drafts/` |
|---|---|---|
| 文件名 | 必须带日期：`2026-01-01-title.md` | 不用带日期：`title.md` |
| front matter 的 `date` | 必须有 | 不用写 |
| 是否发布 | 是 | **否** |

### 写草稿

```bash
# 文件名不用日期前缀
vim _drafts/my-half-done-idea.md
```

front matter 只要标题就够了：

```markdown
---
title: "还没写完的想法"
---

随便写……
```

### 预览草稿

草稿默认连本地也不显示，要加 `--drafts`：

```bash
bundle exec jekyll serve --drafts
```

预览时 Jekyll 会用文件的修改时间当作日期。

### 发布草稿

把文件从 `_drafts/` 移到 `_posts/`，并在文件名前加上日期、在 front matter 里补上 `date`：

```bash
git mv _drafts/my-half-done-idea.md _posts/2026-01-01-my-half-done-idea.md
# 然后编辑 front matter，加上 date: 2026-01-01
```

### 另一种草稿方式

如果文章已经在 `_posts/` 里但暂时不想发布，加一行 front matter 即可，不用来回搬文件：

```markdown
---
title: "暂时不发"
date: 2026-01-01
published: false
---
```

> 注意：`date` 写成未来的日期也会导致文章不发布（Jekyll 默认跳过未来日期的文章）。想定时发布可以利用这一点，但别拿它当草稿机制用 —— 日期一到就自动上线了。

## 关于主题

用的是 GitHub Pages [官方支持主题](https://pages.github.com/themes/)，在 `_config.yml` 里一行 `theme:` 就能切换，构建最稳。

如果以后想换成官方列表之外的第三方主题，要用 `remote_theme: 作者/主题名`，并注意两个坑：

1. **插件白名单**：第三方主题的 `_config.yml` 往往带了 GitHub Pages [不允许的插件](https://pages.github.com/versions/)，照抄会导致构建失败，只能改用白名单内的等价插件。
2. **老版本 Sass**：GitHub Pages 的构建环境固定在 Jekyll 3.10 + Ruby Sass 3.7.4（2019 年起已停止维护）。现代主题的样式表常用 `@use`、`clamp(1rem + 2vw, …)` 这类新语法，老 Sass 编译不了 —— 有的直接报错让构建失败，有的（比如 `@use`）不报错但原样输出，导致线上样式全部丢失。**本地用新版 Jekyll 构建通过不代表线上能用**，换主题后一定要打开线上页面确认。

真要用这类主题，可靠做法是改用 GitHub Actions 自己跑 Jekyll 4 构建再部署，不走 Pages 自带的构建。

## 本地预览（可选）

```bash
bundle install
bundle exec jekyll serve           # 打开 http://127.0.0.1:4000
bundle exec jekyll serve --drafts  # 同时预览草稿
```

## License

见 [LICENSE](LICENSE)。
