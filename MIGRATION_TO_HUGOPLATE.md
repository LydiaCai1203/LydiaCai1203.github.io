# 迁移到 Hugoplate 主题指南

## ✅ 已完成的配置

1. **创建了 Hugoplate 配置文件**
   - `hugo.toml` - 主配置文件
   - `config/_default/module.toml` - 模块配置
   - `config/_default/params.toml` - 主题参数
   - `config/_default/menus.zh-cn.toml` - 中文菜单
   - `config/_default/languages.toml` - 语言配置
   - `data/social.json` - 社交链接

2. **配置已更新**
   - 网站标题：阿菜的骚话天地
   - 主章节：blog
   - 语言：中文（zh-cn）
   - 时区：Asia/Shanghai

## 📦 接下来需要做的

### 1. 重命名文章目录 ⚠️ 重要

Hugoplate 主题使用 `mainSections = ["blog"]`，所以**必须**将 `content/post` 重命名为 `content/blog`：

```bash
cd content
mv post blog
```

**注意**：我已经创建了 `content/blog/_index.md`，重命名后这个文件会自动在正确的位置。

### 2. 移除文章的 type = "blog"（可选）

Hugoplate 通过目录识别文章，不需要在 front matter 中指定 `type = "blog"`。如果你想清理，可以移除所有文章中的 `type = "blog"` 行。

### 3. 初始化 Hugo Modules

Hugoplate 使用 Hugo Modules，需要初始化：

```bash
hugo mod init github.com/LydiaCai1203/lydiacai1203.github.io
hugo mod get -u
```

### 4. 安装 Node.js 依赖（如果需要）

Hugoplate 使用 Tailwind CSS，可能需要 Node.js：

```bash
npm install
```

### 5. 预览新主题

```bash
hugo server
```

然后在浏览器中打开 `http://localhost:1313` 查看效果。

## 📝 重要说明

### 关于文章格式

- Hugoplate 支持 TOML 和 YAML front matter
- 你的文章使用 TOML 格式（`+++`），这是可以的
- 不需要 `type = "blog"`，因为主题通过目录识别

### 关于目录结构

- 文章应该在 `content/blog/` 目录下
- 关于页面在 `content/about.md`
- 静态资源在 `static/` 目录中

### 关于配置

- 主配置文件是 `hugo.toml`（不是 `config.toml`）
- 主题参数在 `config/_default/params.toml`
- 菜单在 `config/_default/menus.zh-cn.toml`

## 🔧 可能需要的调整

1. **Logo 和 Favicon** - 在 `config/_default/params.toml` 中配置
2. **社交链接** - 在 `data/social.json` 中配置
3. **主题颜色** - 在 `data/theme.json` 中配置（如果存在）
4. **文章图片** - 确保图片路径正确

## 📚 参考资源

- Hugoplate GitHub: https://github.com/zeon-studio/hugoplate
- Hugo 文档: https://gohugo.io/documentation/
