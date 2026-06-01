# 技巧博客

日常开发技巧记录博客，基于 GitHub Pages + Jekyll + Chirpy 主题搭建。

## 技术栈

- **托管平台**: GitHub Pages
- **静态站点生成器**: Jekyll
- **主题**: Chirpy
- **评论系统**: Giscus（基于 GitHub Discussions）

## 本地开发

```bash
# 安装依赖
bundle install

# 本地预览
bundle exec jekyll s
```

访问 `http://127.0.0.1:4000` 即可预览。

## 文章规范

1. 在 `_posts/` 下创建分类目录（如 `日常技巧/`）
2. 文件名格式：`YYYY-MM-DD-标题.md`
3. 文件头部添加 YAML front matter：

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

> 更多分类按需添加，只需在 `_posts/` 下新建目录即可。

## 部署

推送至 GitHub `main` 分支后，GitHub Actions 自动构建并部署到 GitHub Pages。
