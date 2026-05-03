+++
title = "Linux 内存管理"
description = "技术"
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



#### free usage

```markdown
(venv367) caiqingjing@ip-127.0.0.1:~$ free -h
              总计         已用        空闲        共享         缓冲/缓存     可用
内存：         31G        7.1G        2.7G        105M         21G         23G交换：          0B          0B          0B

1. 关注 可用 列，估计有多少内存可用于启动新的程序而无须交换。
2. 理论上，used - buffers/cache 表示应用程序实际使用的内存？；free+buffers/cache 表示理论上可以被使用的内存。free 仅仅表示未被使用的内存大小。仅仅看 free 来判断是不够准确的。
3. 当可用内存接近 0 或 非常小时，已用内存接近总容量 时，意味这需要使用到交换磁盘了。可以使用 `grep -i kill /var/log/messages *` 或 `dmesg | grep oom-killer` 检查内存溢出日志消息。
```

#### 缓存（cache）

```markdown
read_cache:读文件时，通常需要将硬盘里的数据读入内存，但是硬盘和内存的读取速度相差太多，因此 linux 会将数据读入 cache 中，然后从 cache 中读取需要的数据。当一个缓存中的数据被多次读取，实际上就减少了该数据从慢速设备中读取的次数。因此 cache 中的数据是随机访问的特性。write_cache:与 read_cache 对应，它的目标应该是减少写入慢速设备的次数。使用 cache 的话，可以多次写入 cache, 但是只写入一次硬盘。
```

#### 缓冲（buffer）

```markdown
read_buffer: 每当 buffer 满或者主动 `flush` 时会触发一次读取，对于小数据可以减少读取次数，对于大数据可以控制单次读取的数据量。通常来说，先入 buffer 的数据会被先读取，所有的 buffer 数据几乎一定会被读取，拥有顺序访问的特性。write_buffer:同上，也是需要 buffer 满或者主动 `flush` 时才会触发写入。如果每次写入的数据量相对固定。因为如果一次写入 4k 对与某个设备来说效率最高的话，buffer 一定是 4k。小数据积攒到 4k 写入，大数据分割成 4k 的碎片写入，这是 write_buffer 的用处。
```

#### 交换内存（swapping）

```markdown
当主存(RAM) 不足以临时存储多个程序时，我们从 RAM 中取出一些程序，通过 swap out 的机制将它们存储到硬盘中。当 RAM 可用时，我们再次将程序从硬盘交换到 RAM, 这就是 swap in。Linux 中的交换空间是物理内存的两倍。优点：
1. 借助交换空间，我们可以在同一 RAM 中管理许多进程.
2. 交换空间帮助我们创建虚拟内存。
3. 交换空间是经济实惠的。硬盘的加个比主存要便宜。
```

[通过 free 命令理解 linux 内存管理](https://cizixs.com/2015/10/01/linux-memory-management-through-free/)

[Dissecting the free command: What the Linux sysadmin needs to know](https://www.redhat.com/sysadmin/dissecting-free-command)
