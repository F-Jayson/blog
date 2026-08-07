---
title: "从 Hexo 到 GitHub Pages：我的博客是如何发布的"
date: 2026-08-07 19:33:03
updated: 2026-08-07 19:33:03
description: 记录这个博客使用 Hexo、Butterfly 与 GitHub Pages 完成构建和自动发布的过程。
categories:
  - 技术
tags:
  - Hexo
  - GitHub Pages
  - Butterfly
cover: 'https://blog-jayson.com/blog-jayson/img/user-assets/5/msj0a53j-0cda798fac3704a0.webp'
---

这个博客使用 Hexo 生成静态页面，以 Butterfly 作为主题，并通过 GitHub Actions 自动发布到 GitHub Pages。本文记录它的基本结构和发布流程，也作为博客中的第一篇示例文章。

<!-- more -->

## 为什么选择静态博客

静态博客不需要长期运行的服务器或数据库。文章以 Markdown 文件保存在 Git 仓库中，每次提交后重新生成 HTML、CSS 和 JavaScript，再将生成结果发布到 GitHub Pages。

这种方式有几个直接的好处：

- 文章和配置都能通过 Git 追踪修改历史。
- 部署过程可以完全交给 GitHub Actions。
- 页面由静态文件组成，访问路径清晰，维护成本较低。
- 本地环境和线上环境使用同一套依赖与构建命令。

## 项目使用的工具

当前博客由以下部分组成：

| 工具 | 用途 |
| --- | --- |
| Hexo | 将 Markdown 文章生成静态网站 |
| Butterfly | 提供页面布局、导航、搜索和文章样式 |
| GitHub Actions | 安装依赖并执行自动构建 |
| GitHub Pages | 托管生成后的静态文件 |

项目部署在仓库的子路径 `/blog/` 下，因此 Hexo 的站点地址需要保持一致：

```yaml
url: https://f-jayson.github.io/blog
root: /blog/
```

如果 `root` 配置错误，页面本身可能可以打开，但样式、脚本和图片会指向错误的地址。

## 本地预览与构建

首次拉取仓库后，先安装锁文件中记录的依赖：

```bash
npm ci
```

启动本地服务器：

```bash
npm run server
```

修改文章或配置后，可以执行一次完整构建：

```bash
npm run clean
npm run build
```

Hexo 会将生成结果写入 `public/`。该目录属于构建产物，不需要提交到主分支。

## 自动发布流程

推送到 `main` 分支后，GitHub Actions 会依次完成以下步骤：

1. 检出仓库代码。
2. 使用指定版本的 Node.js。
3. 通过 `npm ci` 安装确定版本的依赖。
4. 运行 `npm run build` 生成静态文件。
5. 上传 `public/` 并部署到 GitHub Pages。

日常发布一篇文章只需要提交 Markdown 文件：

```bash
git add source/_posts
git commit -m "add new post"
git push origin main
```

构建和部署状态可以在仓库的 Actions 页面查看。如果构建失败，应先检查文章 front-matter、YAML 缩进和依赖锁文件。

## 一篇文章的基本信息

每篇文章顶部的 front-matter 用于描述标题、日期、分类和标签：

```yaml
---
title: 文章标题
date: 2026-08-07 19:33:03
categories:
  - 技术
tags:
  - Hexo
description: 文章摘要
---
```

分类适合表达文章所属的主要领域，标签则适合记录文章涉及的具体技术。填写这些信息后，Hexo 会同步更新分类页、标签页、搜索索引、站点地图和 RSS。

## 后续计划

- 持续整理博客导航和文章分类。
- 为技术文章补充可复现的代码与运行环境。
- 压缩图片资源，减少首屏加载体积。
- 在发布前执行本地构建和链接检查。

博客的源代码保存在 [F-Jayson/blog](https://github.com/F-Jayson/blog)。后续文章会继续记录实际开发中遇到的问题、解决过程和可以复用的经验。
