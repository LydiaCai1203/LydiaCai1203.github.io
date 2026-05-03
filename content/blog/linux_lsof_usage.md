+++
title = "Linux lsof 命令使用"
description = "工具文档"
tags = [
    "2021",
    "技术",
]
date = "2021-04-21"
categories = [
    "技术",
]
type = "blog"
+++



```markdown
为了以后查阅方便所做。
                                                        ------- 菜大头
```

#### lsof 简介

```markdown
`lsof` 是 `list open files` 的简称。意思是 列出系统中打开的文件。由于 Linux 中 `everything is file`。所有的对象都可以看作是文件，`lsof` 就可以知道用户和进程都操作了哪些文件，也可以查看系统中网络的使用情况、设备信息。
```

- `lsof | head -n 20`
  - 会列出系统中所有打开的前20个文件，每个文件一行
  - FD
    - cwd: 当前工作目录
    - txt: 应用文本(代码、数据)
    - mem: 内存映射文件
    - mmap: 内存映射设备
  - TYPE
    - REG: 普通文件
    - DIR: 文件夹
- `lsof -p 1190`
  - 打开 1190 进程打开的所有文件
- `lsof -u caiqingjing` or `lsof -u ^caiqingjing`
  - 打开某个用户打开的文件 or 除了某个用户..
- `lsof file_path`
  - 查看某个 文件/目录 被哪些进程打开
- `lsof +d dir_path`
  - 列出访问某个目录的所有进程
- `lsof -c nginx`
  - 列出某个命令使用的文件信息

#### 使用 lsof 查看网络信息

- `lsof -i`
  - 列出所有的网络连接信息
- `lsof -i TCP`
  - `-i` 后面跟协议类型，只显示 TCP 网络协议的连接信息
- `lsof -i :80`
  - 查看 80 端口被哪个进程使用
- `lsof -i @172.16.1.14` or `lsof -i @172.16.1.14:22`
  - 查看连接到某个主机的网络情况
- `lsof -i -s TCP:LISTEN` or `lsof -i -s TCP:ESTABLISHED`
  - 列出当前机器监听的端口

#### 组合用法

- `lsof -a -p 12345 -i -s TCP:LISTEN`
  - `lsof` 的过滤参数是可以组合使用的，默认情况是 or 逻辑，列出 12345 进程监听的所有网络连接

[linux lsof 命令使用指南(https://cizixs.com/2017/05/16/linux-lsof-primer/)


