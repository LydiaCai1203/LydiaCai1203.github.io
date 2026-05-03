# Hugo 版本检查

## 错误信息
```
WARN  Module "github.com/LydiaCai1203/lydiacai1203.github.io" is not compatible with this Hugo version: Min 0.151.0 extended
WARN  Module "hugoplate" is not compatible with this Hugo version: Min 0.151.0
```

## 解决方案

### 1. 检查当前 Hugo 版本
```bash
hugo version
```

### 2. 如果版本低于 0.151.0 或不是 extended 版本

**macOS (使用 Homebrew):**
```bash
brew upgrade hugo
# 或者安装 extended 版本
brew install hugo
```

**或者下载 extended 版本:**
- 访问: https://github.com/gohugoio/hugo/releases
- 下载 `hugo_extended_*` 版本（不是普通版本）

### 3. 验证安装
```bash
hugo version
# 应该显示类似: hugo v0.151.0+extended
```

**注意**: 必须安装 **extended** 版本，因为 Hugoplate 主题需要它来处理 CSS/JS 资源。
