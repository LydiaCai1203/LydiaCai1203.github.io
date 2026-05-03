# 三观已坏 - 个人博客

基于 Hugo + Hugoplate 主题的个人博客网站。

## 📝 如何发布新文章

### 1. 创建新文章

在 `content/blog/` 目录下创建新的 Markdown 文件，例如：

```bash
content/blog/我的新文章.md
```

### 2. 文章格式

文章需要使用 TOML front matter（以 `+++` 开头和结尾），格式如下：

```markdown
+++
title = "文章标题"
description = "文章描述（可选）"
tags = [
    "标签1",
    "标签2",
]
date = "2024-01-01"
categories = [
    "分类",
]
type = "blog"
+++

这里是文章正文内容，使用 Markdown 格式编写。

## 二级标题

### 三级标题

- 列表项
- 列表项

**粗体** *斜体*

[链接文字](https://example.com)
```

### 3. 文章 Front Matter 说明

- `title`: 文章标题（必填）
- `description`: 文章描述，会显示在文章列表中（可选）
- `tags`: 标签数组，用于分类和搜索
- `date`: 发布日期，格式：`YYYY-MM-DD`
- `categories`: 分类数组
- `type`: 必须设置为 `"blog"`，这样文章才会显示在博客列表中

### 4. 本地预览

在发布前，可以先在本地预览：

```bash
# 启动开发服务器
npm run dev
```

然后在浏览器中打开 `http://localhost:1313` 查看效果。

> **注意**：本地预览使用 `localhost:1313`，生产环境网站地址是 `https://lydiacai1203.github.io/`

### 5. 构建和发布

#### 方法一：使用 npm 脚本（推荐）

```bash
# 构建网站
npm run build
```

构建完成后，`public` 目录会包含所有静态文件。

> **重要**：如果修改了文章的分类、标签或删除了文章，需要重新运行 `npm run build` 来更新 `public` 目录，旧的分类和标签才会被移除。

#### 方法二：直接使用 Hugo

```bash
# 生成 TailwindCSS
node themes/hugoplate/scripts/themeGenerator.js

# 构建网站
hugo --gc --minify
```

### 6. 部署到 GitHub Pages

#### 如果使用 GitHub Actions 自动部署

1. 将代码推送到 GitHub：
   ```bash
   git add .
   git commit -m "添加新文章：文章标题"
   git push
   ```

2. GitHub Actions 会自动构建并部署到 `gh-pages` 分支

#### 如果手动部署

1. 构建网站后，将 `public` 目录的内容推送到 `gh-pages` 分支：
   ```bash
   cd public
   git init
   git add .
   git commit -m "更新网站"
   git branch -M gh-pages
   git remote add origin https://github.com/LydiaCai1203/LydiaCai1203.github.io.git
   git push -u origin gh-pages
   ```

2. 或者使用 `hugo deploy` 命令（需要先配置）

## 🛠️ 开发环境

### 前置要求

- **Hugo**: 需要 0.151.0 或更高版本的 extended 版本
- **Node.js**: v22+ （用于 TailwindCSS）
- **npm**: 用于安装依赖

### 安装依赖

```bash
# 安装 Node.js 依赖
npm install

# 初始化 Hugo Modules（如果需要）
hugo mod get -u
```

### 常用命令

```bash
# 开发模式（自动重新加载）
npm run dev

# 构建生产版本
npm run build

# 预览生产版本
npm run preview
```

## 📁 项目结构

```
.
├── content/              # 内容目录
│   ├── blog/            # 博客文章
│   │   ├── _index.md    # 博客列表页
│   │   └── *.md         # 文章文件
│   └── about.md         # 关于页面
├── layouts/              # 自定义布局（覆盖主题）
├── assets/               # 资源文件（CSS、JS）
├── static/               # 静态文件（图片等）
├── config/               # 配置文件
├── themes/               # 主题目录
├── public/               # 构建输出目录（不要手动编辑）
└── hugo.toml            # Hugo 主配置文件
```

## 🎨 自定义配置

### 修改网站信息

编辑 `hugo.toml`：
- `title`: 网站标题
- `baseURL`: 网站地址

编辑 `config/_default/params.toml`：
- `logo_text`: Logo 文字
- `description`: 网站描述

### 修改菜单

编辑 `config/_default/menus.zh-cn.toml` 来修改导航菜单。

## 📝 文章示例

查看 `content/blog/` 目录下的现有文章作为参考。

## ⚠️ 注意事项

1. **文章必须放在 `content/blog/` 目录下**
2. **Front matter 中的 `type` 必须设置为 `"blog"`**
3. **日期格式必须是 `YYYY-MM-DD`**
4. **构建前需要先运行 TailwindCSS 生成器**（`npm run build` 会自动处理）
5. **不要直接编辑 `public` 目录**，这是构建输出目录
6. **修改分类或标签后，必须重新构建**（`npm run build`）才能更新分类页面

## 🐛 常见问题

### 文章不显示

- 检查 `type = "blog"` 是否设置
- 检查文件是否在 `content/blog/` 目录下
- 检查日期格式是否正确

### 样式不显示

- 确保运行了 `npm run build` 或 `npm run dev`
- 检查 TailwindCSS 是否正常生成

### 本地预览正常，但发布后有问题

- 检查 `baseURL` 配置是否正确
- 确保 `public` 目录已正确构建
- 检查 GitHub Pages 设置

### 旧的分类或标签还在显示

- **需要重新构建网站**：运行 `npm run build`
- `public` 目录是构建输出，修改文章后必须重新构建才会更新
- 旧的分类页面会一直存在，直到重新构建

## 📚 参考资源

- [Hugo 文档](https://gohugo.io/documentation/)
- [Hugoplate 主题](https://github.com/zeon-studio/hugoplate)
- [Markdown 语法](https://www.markdownguide.org/)

---

**祝你写作愉快！** ✨
