# 技巧博客网站 — GitHub Pages 实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将当前空的博客项目改造为基于 Chirpy 主题的 Jekyll 教程博客网站，部署到 GitHub Pages

**Architecture:** 使用 Jekyll + Chirpy 主题（gem 包方式），内容通过 `_posts/` 目录下的 Markdown 文件管理，GitHub Actions 自动构建部署，Giscus（基于 GitHub Discussions）提供评论功能

**Tech Stack:** GitHub Pages, Jekyll, jekyll-theme-chirpy (gem), Giscus

---

### Task 1: 用 Chirpy Starter 替换项目骨架

**说明：** 当前项目只有 `idnex.html`（空壳）和一篇文章。我们从 Chirpy Starter 模板中获取核心文件，替代当前的骨架。该模板提供了 Jekyll + Chirpy 所需的标准目录结构和配置文件。

**Files:**
- Create: `Gemfile` — Ruby 依赖声明
- Create: `_config.yml` — 站点配置（初始从模板复制）
- Create: `_data/contact.yml` — 联系信息
- Create: `_tabs/about.md` — 关于页面
- Create: `_tabs/archives.md` — 归档页面
- Create: `_tabs/categories.md` — 分类页面
- Create: `_tabs/tags.md` — 标签页面
- Create: `assets/` — 默认资源文件目录
- Create: `.github/workflows/pages-deploy.yml` — GitHub Actions 部署配置
- Create: `tools/` — 部署工具脚本
- Delete: `idnex.html` — 替换为 Chirpy 的 `index.html`

- [ ] **Step 1: 克隆 chirpy-starter 到临时目录**

```bash
cd /tmp
git clone https://github.com/cotes2020/chirpy-starter.git _chirpy_temp
```

- [ ] **Step 2: 复制核心文件到项目目录**

```bash
cd /Users/liugensheng/PycharmProject/blog

# 复制 Gemfile
cp /tmp/_chirpy_temp/Gemfile ./

# 复制配置文件
cp /tmp/_chirpy_temp/_config.yml ./
cp -r /tmp/_chirpy_temp/_data ./_data

# 复制页面
cp -r /tmp/_chirpy_temp/_tabs ./_tabs

# 复制资源目录（CSS/JS/图片等）
cp -r /tmp/_chirpy_temp/assets ./assets

# 复制工具和部署配置
cp -r /tmp/_chirpy_temp/tools ./tools
mkdir -p .github/workflows
cp /tmp/_chirpy_temp/.github/workflows/pages-deploy.yml ./.github/workflows/

# 复制 index.html（Chirpy 的首页）
cp /tmp/_chirpy_temp/index.html ./

# 清理临时目录
rm -rf /tmp/_chirpy_temp
```

- [ ] **Step 3: 删除旧的 idnex.html**

```bash
rm idnex.html
```

- [ ] **Step 4: 安装依赖并验证本地构建**

```bash
cd /Users/liugensheng/PycharmProject/blog
bundle install
```

预期：`bundle install` 成功完成，安装 jekyll-theme-chirpy 及其依赖。

- [ ] **Step 5: 提交**

```bash
git add -A
git commit -m "feat: initialize Jekyll project with Chirpy theme

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 2: 配置 _config.yml（站点信息 + Giscus）

**说明：** 修改 `_config.yml`，填入你的博客信息、社交链接、Giscus 评论配置等。Giscus 的 `repo_id` 和 `category_id` 需要后续通过 [giscus.app](https://giscus.app) 获取，先留占位符。

**Files:**
- Modify: `_config.yml`

- [ ] **Step 1: 读取当前 _config.yml**

```bash
cat _config.yml | head -80
```

先查看模板默认内容，确认需要修改的部分。

- [ ] **Step 2: 修改 _config.yml 核心配置**

编辑 `_config.yml`，将以下关键字段替换为你的信息：

```yaml
# 站点信息
title: 技巧博客
tagline: 日常开发技巧记录
url: "https://68110923.github.io"
baseurl: "/blog"         # 因为仓库名是 blog

# 头像（可选，不设置会自动隐藏）
avatar: ""

# 时区
timezone: Asia/Shanghai

# 社交
social:
  name: Aedda
  github: 68110923
  email: ""    # 不公开邮箱就留空

# 文章默认设置
defaults:
  - scope:
      path: ""
      type: posts
    values:
      layout: post
      comments: true
      toc: true
      permalink: /posts/:title/

# 评论 — Giscus，repo_id 和 category_id 后续替换
comments:
  provider: giscus    # ⚠️ 注意：Chirpy v7.5 使用 provider，不是 active！
  giscus:
    repo: 68110923/blog
    repo_id: GET_FROM_GISCUS_APP
    category: Announcements
    category_id: GET_FROM_GISCUS_APP
    input_position: top
    lazy_load: true
    mapping: title
```

> ⚠️ `repo_id` 和 `category_id` 需要在 GitHub 开启 Discussions 后，通过 giscus.app 获取。后续 Task 5 中会完善。

- [ ] **Step 3: 提交**

```bash
git add _config.yml
git commit -m "feat: configure site info and Giscus comments

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 3: 迁移现有教程文章

**说明：** 将已有的 `ssh快捷使用.md` 按 Chirpy 规范移动到 `_posts/日常技巧/` 目录下，并添加 YAML front matter。

**Files:**
- Create: `_posts/日常技巧/2025-01-15-ssh快捷使用.md`
- Delete: `日常技巧/ssh快捷使用.md`

- [ ] **Step 1: 创建分类目录并移动文件**

```bash
mkdir -p _posts/日常技巧
cp "日常技巧/ssh快捷使用.md" "_posts/日常技巧/2025-01-15-ssh快捷使用.md"
```

- [ ] **Step 2: 为文章添加 YAML front matter**

在 `_posts/日常技巧/2025-01-15-ssh快捷使用.md` 文件最开头添加：

```yaml
---
title: SSH 快捷登录
date: 2025-01-15 10:00:00 +0800
categories: [日常技巧]
tags: [ssh, 服务器, 效率]
---
```

- [ ] **Step 3: 删除旧文件**

```bash
rm -r "日常技巧/"
```

- [ ] **Step 4: 提交**

```bash
git add -A
git commit -m "feat: migrate SSH tutorial to Chirpy post format

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 4: 编写 README.md

**说明：** 项目的 README 文件，介绍项目用途、技术栈、如何贡献等。

**Files:**
- Create: `README.md`

- [ ] **Step 1: 编写 README.md**

```markdown
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
```

- [ ] **Step 2: 提交**

```bash
git add README.md
git commit -m "docs: add README with project introduction

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Task 5: 推送到 GitHub 并配置 Giscus

**说明：** 将代码推送到 GitHub，启用仓库 Discussions，然后通过 giscus.app 获取配置 ID 并更新到 `_config.yml`。

**Files:**
- Modify: `_config.yml` — 更新 Giscus 的 `repo_id` 和 `category_id`

- [ ] **Step 1: 推送到 GitHub**

```bash
git push -u origin main
```

- [ ] **Step 2: 在 GitHub 仓库开启 Discussions**

1. 打开 https://github.com/68110923/blog
2. 进入 **Settings → General → Features**
3. 勾选 **Discussions**
4. 点击 **Set up discussions**，选择 **Announcements** 分类，点击 **Start discussion**

- [ ] **Step 3: 通过 giscus.app 获取配置 ID**

1. 打开 https://giscus.app
2. 在 **Repository** 输入：`68110923/blog`
3. 页面会自动生成 `repo_id` 和 `category_id`
4. 记下这两个值

- [ ] **Step 4: 更新 _config.yml 中的 Giscus 配置**

将 `repo_id` 和 `category_id` 替换为从 giscus.app 获取的真实值。

```yaml
comments:
  giscus:
    repo: 68110923/blog
    repo_id: <从giscus.app获取的真实值>
    category: Announcements
    category_id: <从giscus.app获取的真实值>
```

- [ ] **Step 5: 提交并推送最终配置**

```bash
git add _config.yml
git commit -m "chore: update Giscus IDs"
git push
```

---

### Task 6: 完成并验证

- [ ] **Step 1: 检查 GitHub Actions 构建状态**

1. 打开 https://github.com/68110923/blog/actions
2. 确认 Pages build and deploy workflow 成功完成
3. 如果失败，查看日志排查（通常是 `_config.yml` 语法问题）

- [ ] **Step 2: 访问站点确认**

1. 访问 `https://68110923.github.io/blog`
2. 确认首页正常显示
3. 点击 SSH 快捷使用文章，确认内容正确渲染
4. 确认 TOC 目录、深色/浅色切换正常
5. 确认文章底部出现 Giscus 评论区域

- [ ] **Step 3: 后续新增文章流程**

每次新增教程只需：
```bash
# 1. 在 _posts/下新建或放入 .md 文件
# 2. 确保 YAML front matter 正确
# 3. 推送
git add _posts/
git commit -m "feat: add new tutorial"
git push
# GitHub Actions 自动构建 → 自动更新
```
