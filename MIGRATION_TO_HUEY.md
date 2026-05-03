# 迁移到 Huey 主题指南

## ✅ 已完成的配置

1. **配置文件已更新** - `config.toml` 已设置为使用 `huey` 主题
2. **菜单配置** - 已配置导航菜单（首页、关于、简历、GitHub）
3. **内容保留** - 所有文章和资源都会自动保留：
   - ✅ `content/` 目录中的所有文章（26 篇文章）
   - ✅ `static/images/` 目录中的头像和图片资源
   - ✅ 所有配置信息

## 📦 接下来需要做的

### 1. 安装 Huey 主题

在终端中运行以下命令：

```bash
cd themes
git clone https://github.com/alloydwhitlock/huey.git
cd ..
```

### 2. 验证安装

```bash
ls themes/huey
```

应该能看到主题文件。

### 3. 查看主题示例配置（可选）

```bash
cat themes/huey/exampleSite/config.toml
```

可以参考示例配置来调整你的 `config.toml`。

### 4. 预览新主题

```bash
hugo server
```

然后在浏览器中打开 `http://localhost:1313` 查看效果。

## 📝 重要说明

### 关于 public 目录

- `public/` 目录是 Hugo 构建后的输出目录
- 当你运行 `hugo` 或 `hugo server` 时，Hugo 会重新生成这个目录
- **你的源文件都在 `content/` 和 `static/` 目录中，这些不会被删除**

### 关于头像和图片

- 头像文件在 `static/images/` 目录中（`avatar.png`, `avatar@2x.png` 等）
- 这些文件会自动复制到构建后的 `public/` 目录
- 如果 Huey 主题需要特定路径的头像，可以在安装主题后调整配置

### 关于文章

- 所有 26 篇文章都在 `content/post/` 目录中
- 文章使用 TOML 格式的 front matter（`+++`）
- **重要**：Huey 主题要求博客文章在 front matter 中添加 `type: blog`
- Huey 主题默认使用 `/archive/` 路径显示博客文章，但你的文章在 `/post/` 目录中
- 如果需要，可以：
  1. 将 `content/post/` 重命名为 `content/archive/`
  2. 或者在每篇文章的 front matter 中添加 `type: blog`

## 🔧 可能需要的调整

安装主题后，你可能需要：

1. **调整头像路径** - 如果主题需要特定路径的头像
2. **调整文章 front matter** - 如果主题需要不同的元数据格式
3. **自定义样式** - 如果需要调整颜色、字体等
4. **添加主题特定配置** - 根据 Huey 主题的文档添加特定参数

## 📚 参考资源

- Huey 主题页面：https://themes.gohugo.io/themes/huey/
- Hugo 文档：https://gohugo.io/documentation/

## ❓ 遇到问题？

如果安装或配置过程中遇到问题：

1. 检查主题是否正确安装：`ls themes/huey`
2. 查看 Hugo 错误信息：`hugo server --verbose`
3. 参考主题的 README 或示例配置
