# pmbdlw.github.io

个人博客，基于 [GitHub Pages](https://pages.github.com/) + [Jekyll](https://jekyllrb.com/) 构建，主题为 [moonwalk](https://github.com/abhinavs/moonwalk)（极简、自带暗色模式）。

线上地址：<https://pmbdlw.github.io>

## 目录结构

```
_config.yml      站点配置（标题、主题、功能开关、Markdown 解析、插件）
index.md         首页（home 布局，自动列出全部文章）
tags.html        标签归档页，地址 /tags
404.html         404 页面
search.json      文章索引，供主题的站内搜索和链接悬停预览使用
_posts/          已发布的文章，文件名必须是 YYYY-MM-DD-标题.md
_drafts/         草稿，不会发布（见下文）
_data/
  home.yml       顶部导航和页脚的链接
_includes/       覆盖主题的两个文件（见「关于主题」）
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
toc: true          # 可选，自动生成目录
---

正文从这里开始，使用 GitHub 风格 Markdown。
```

要点：

- **文件名用 ASCII**，中文标题写在 front matter 的 `title` 里，这样生成的链接才干净（`/posts/英文短标题/`）。
- 目录用 front matter 的 `toc: true` 开启，由主题渲染成可折叠的「目录」块。**不要用 `[TOC]`**，kramdown 不认识它，会原样显示。
- 引用图片：把图片放进 `assets/img/`，文件名用 ASCII，然后 `![说明](/assets/img/图片名.png)`。
- 标签会自动汇总到 `/tags` 页面，不用手动维护。

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

主题用 `remote_theme: abhinavs/moonwalk` 加载，构建时自动从 GitHub 拉取，本仓库不保存主题文件。两个注意点：

1. **插件白名单**：moonwalk 自带的 `jekyll-soopr-seo-tag`、`jekyll-markdown-output`、`jekyll-llms-output` 不在 [GitHub Pages 白名单](https://pages.github.com/versions/)内，不能写进 `_config.yml`，否则构建失败。本仓库改用白名单内的 `jekyll-seo-tag` / `jekyll-feed` / `jekyll-sitemap`，主题布局里的 `{% raw %}{% seo %}{% endraw %}` 和 `{% raw %}{% feed_meta %}{% endraw %}` 由它们提供。
2. **`_data` 不会被继承**：remote_theme 只带 `_layouts` / `_includes` / `_sass` / `assets`，所以导航栏和页脚的数据必须由本仓库的 `_data/home.yml` 提供。

`_includes/` 下有两个文件是主题同名文件的本地覆盖版，为了把界面文字改成中文：

- `toc.html` —— 目录标题由 `Table of Contents` 改为「目录」
- `date_and_social_share.html` —— 日期改成中文格式；阅读时间改成按字数估算（上游按空格分词，中文整篇会被算成 1 个词）

升级主题后如果这两处显示异常，对比上游同名文件同步一次即可。外观开关（亮/暗、导航栏、阅读进度条、代码复制按钮等）都在 `_config.yml` 的 `theme_config` 里，每项都有注释。

## 本地预览（可选）

```bash
bundle install
bundle exec jekyll serve           # 打开 http://127.0.0.1:4000
bundle exec jekyll serve --drafts  # 同时预览草稿
```

## License

见 [LICENSE](LICENSE)。
