# 技巧博客

日常开发技巧记录博客，基于 GitHub Pages + Jekyll + Chirpy 主题搭建。

## 技术栈

- **托管平台**: GitHub Pages
- **静态站点生成器**: Jekyll
- **主题**: Chirpy
- **评论系统**: Giscus（基于 GitHub Discussions）

## 本地开发

**前置要求：** Ruby（>= 3.1）和 Bundler。

```bash
# 安装依赖
bundle install

# 本地预览
bundle exec jekyll s
```

访问 `http://127.0.0.1:4000/blog/` 即可预览。

## 文章规范

1. 在 `_posts/` 下创建分类目录（如 `日常技巧/`）方便文件管理
2. 文件名格式：`YYYY-MM-DD-标题.md`
3. 文件头部添加 YAML front matter，**分类（categories）由 front matter 定义**：

```yaml
---
title: 文章标题
date: YYYY-MM-DD HH:MM:SS +0800
categories: [分类名]
tags: [标签1, 标签2]
---
```

4. 推送 GitHub 后自动构建部署

## 文章分类

- 🛠 日常技巧 — 开发工具、效率技巧

> 更多分类按需添加：在 `_posts/` 下新建目录 + 在 front matter 设置 `categories` 字段即可。

## 部署

1. 在 GitHub 仓库 **Settings → Pages → Source** 选择 **GitHub Actions**
2. 推送至 `main` 分支，GitHub Actions 自动构建并部署到 GitHub Pages
