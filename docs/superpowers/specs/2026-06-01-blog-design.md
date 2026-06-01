# 技巧博客网站 — GitHub Pages 设计文档

## 概述

基于 GitHub Pages + Jekyll + Chirpy 主题的教程博客网站。用户编写 Markdown 教程文章，推送 GitHub 后自动构建展示，无需手动维护文章列表。

## 技术栈

- **托管平台**: GitHub Pages（免费静态托管）
- **静态站点生成器**: Jekyll（GitHub Pages 原生支持）
- **主题**: Chirpy（热门 Jekyll 博客主题）
- **评论系统**: Giscus（基于 GitHub Discussions）

## 项目目录结构

```
blog/
├── _posts/                    # 📁 所有教程文章
│   ├── 日常技巧/
│   │   └── YYYY-MM-DD-标题.md
│   ├── 服务器运维/             # （后续扩展）
│   │   └── YYYY-MM-DD-标题.md
│   └── ...                     # （后续扩展更多分类）
├── _config.yml                 # ⚙️ Chirpy 核心配置
├── _data/
│   └── locales/zh-CN.yml       # 🇨🇳 中文语言包（如有需要）
├── assets/                     # 静态资源
├── pages/                      # 分类/标签/关于等页面
├── Gemfile                     # Ruby 依赖
├── .github/
│   └── workflows/              # GitHub Actions 构建配置
│       └── pages-deploy.yml
└── README.md                   # 项目介绍
```

## 文章格式规范

### 文件名
```
YYYY-MM-DD-标题.md
```
例如：`2025-01-15-ssh快捷使用.md`

### YAML Front Matter
```yaml
---
title: SSH 快捷登录
date: 2025-01-15 10:00:00 +0800
categories: [日常技巧]
tags: [ssh, 服务器, 效率]
---
```

### 分类扩展
新增分类只需：
1. 在 `_posts/` 下新建对应的目录
2. 文章 front matter 中修改 `categories`
3. Chirpy 自动识别并生成分类导航

### 正文
原 Markdown 内容完全无需改动，Jekyll 自动渲染。

## 主题配置

### Chirpy 内置功能
- 深色/浅色模式切换
- 自动生成文章目录（TOC）
- 标签/分类导航
- 搜索功能
- RSS Feed

### _config.yml 核心配置
```yaml
title: 技巧博客
tagline: 日常开发技巧记录
url: "https://<用户名>.github.io"
baseurl: "/blog"
avatar: /assets/img/avatar.png
favicon: /assets/img/favicon.ico

social:
  name: <用户名>
  github: <GitHub用户名>

defaults:
  - scope:
      path: ""
      type: posts
    values:
      layout: post
      comments: true
      toc: true

comments:
  provider: giscus
  giscus:
    repo: <用户名>/blog
    repo_id: <由giscus.app生成>
    category: Announcements
    category_id: <由giscus.app生成>
```

## 部署流程

### 1. 初始化
使用 Chirpy Starter 模板或手动搭建 Jekyll 项目。

### 2. GitHub 设置
- 仓库 Settings → Pages → Source 选择 **GitHub Actions**
- 仓库 Settings → General → Features → 开启 **Discussions**

### 3. Giscus 配置
在 [giscus.app](https://giscus.app) 完成配置，获取 `repo_id` 和 `category_id`。

### 4. 内容更新流程
```
写 MD 文章 → 放到 _posts/对应目录 → git add/commit/push → 
GitHub Actions 自动构建 → GitHub Pages 自动更新
```

## 无需手动维护的部分
- ❌ 文章列表 — Chirpy 自动扫描 `_posts/` 生成
- ❌ 分类导航 — 根据 `categories` 自动归类
- ❌ 标签索引 — 根据 `tags` 自动生成标签页
- ❌ 搜索索引 — Chirpy 内置搜索引擎自动索引全站内容
- ❌ 文章更新 — 修改 MD 文件重新推送即覆盖
- ❌ 文章删除 — 删除 MD 文件推送即下架
