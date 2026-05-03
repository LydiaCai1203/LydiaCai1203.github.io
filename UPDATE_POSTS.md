# 更新文章以适配 Huey 主题

## 需要做的更改

根据 [Huey 主题文档](https://github.com/alloydwhitlock/huey)，博客文章需要在 front matter 中添加 `type: blog`。

### 当前状态

- 所有文章在 `content/post/` 目录中
- 文章使用 TOML 格式的 front matter（`+++`）

### 需要添加的内容

每篇文章的 front matter 需要添加：

```toml
type = "blog"
```

### 示例

**修改前：**
```toml
+++
title = "聚类算法原理"
description = "工具文档"
tags = [
    "2024",
    "技术",
]
date = "2024-08-07"
categories = [
    "技术",
]
+++
```

**修改后：**
```toml
+++
title = "聚类算法原理"
description = "工具文档"
tags = [
    "2024",
    "技术",
]
date = "2024-08-07"
categories = [
    "技术",
]
type = "blog"
+++
```

## 批量更新脚本（可选）

如果需要批量更新所有文章，可以使用以下命令（在项目根目录运行）：

```bash
# 为所有 post 目录下的 .md 文件添加 type = "blog"
find content/post -name "*.md" -exec sed -i '' '/^+++$/a\
type = "blog"
' {} \;
```

**注意**：上面的命令可能需要在 macOS 上调整。更安全的方法是手动检查几篇文章，确认格式正确后再批量处理。

## 关于页面

`content/about.md` 已经更新为符合 Huey 主题的要求：
- 添加了 `type: pages`
- 添加了 `layout: page`
- 添加了 `menu: nav` 和 `weight: 30`
- 添加了 `fa_icon: "fas fa-user"`（可选，显示图标）
