# 安装 TailwindCSS 依赖

## 问题
Hugoplate 主题需要 TailwindCSS CLI 来处理 CSS 文件。

## 解决方案

### 1. 安装 Node.js 依赖

在项目根目录运行：

```bash
npm install
```

这会安装所有必要的依赖，包括：
- `@tailwindcss/cli` - TailwindCSS 命令行工具
- `tailwindcss` - TailwindCSS 核心
- 其他相关依赖

### 2. 运行开发服务器

安装完成后，使用以下命令运行：

```bash
npm run dev
```

这会：
1. 自动运行 TailwindCSS 生成器（监听文件变化）
2. 启动 Hugo 开发服务器

### 3. 或者手动运行

如果你想分别运行：

```bash
# 终端 1: 生成 TailwindCSS（监听模式）
node themes/hugoplate/scripts/themeGenerator.js --watch

# 终端 2: 运行 Hugo 服务器
hugo server
```

### 4. 构建生产版本

```bash
npm run build
```

## 注意

- 确保已安装 Node.js (v22+)
- 首次运行 `npm install` 可能需要一些时间
- 如果遇到权限问题，可能需要使用 `sudo`（不推荐）或修复 npm 权限
