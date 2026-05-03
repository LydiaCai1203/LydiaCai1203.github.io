+++
title = "断点续传的原理"
description = "工具文档"
tags = [
    "2021",
    "技术",
]
date = "2021-04-20"
categories = [
    "技术",
]
type = "blog"
+++



[参考文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Range_requests)

#### 1. 什么是断点续传？

```markdown
可以让用户从上一次下载中断的位置接着下载的机制。
```

#### 2. 断点续传的原理

```markdown
1. `curl -I {url}` 查看响应头部是否有 `content-range` 字样，有则代表服务端支持断点续传功能。
2. HTTP HEADERS
    1.1 Range(在有 If-Range 的时候才会生效)
        * Range: bytes=0-1023  请求资源的前 1024 个字节
        * Range: bytes=-1023   请求资源的倒数 1023 个字节
        * Range: bytes=1023-   请求资源第 1024 字节开始及以后的资源
        * Range: bytes=0-50, 100-150  请求资源的多个部分

    1.2 If-Range(必传两个中的一个，但不能同时传)
        1.2.1 Etag
            * 675af34563dc-tr34
            * 文件资源的唯一标示，一个字符串
        1.2.2 Last-Modified
            * If-Range: Wed, 21 Oct 2015 07:28:00 GMT
            * 上一次文件资源修改时间
3. RESP HTTP STATUS CODE
    3.1 206 - Partial Content（此次请求成功）
    3.1 416 - Requested Range Not Satisfiable（请求的范围无法满足）

4. RESP HTTP HEADERS
    4.1 Content-Range
        * Content-Range: bytes 0-1023/146515
        * 表示这一部分内容在整个资源中所处的位置
```

#### 3. 实现一个文件上传和断点续传的文件下载功能

```python
import os
import sys

import requests
from requests.exceptions import Timeout


def filesize(file_path: str):
    return os.stat(file_path).st_size


def download(url: str, chunk_size=65535):
    mod = "wb+"
    downloaded = 0
    home_dir = os.environ['HOME']    filename = url.split("/")[-1]    file_path = os.path.join(f"{home_dir}/Desktop", filename)    if os.path.isfile(file_path):        downloaded = filesize(file_path)        print(f"{filename} existed, download from {downloaded + 1} byte ...")    resp = requests.get(        url,        headers={"Range": f"bytes={downloaded}-"},        stream=True,        timeout=15
    )    total_size = int(resp.headers.get("Content-Length", 0))    if resp.status_code == 416:        print("requested range not satisfiable...")        return     elif resp.status_code == 206:        print("file start downloading...")        # bytes 0-43971447/43971448
        content_range = resp.headers.get("Content-Range")        # 如果服务端支持断点续传，则追加方式写入文件，否则重新写文件
        if content_range and int(content_range.split(" ")[-1].split("-")[0]) == downloaded:            mod = "ab+"
            total_size = int(content_range.split(" ")[-1].split("/")[-1])        with open(file_path, mod) as f:            for chunk in resp.iter_content(chunk_size):                f.write(chunk)                downloaded += len(chunk)                process = round((downloaded/total_size) * 100, 2)                print(f"\r{process}% bytes has downloaded...", end="")        print("file downloaded finished...")


if __name__ == "__main__":    url = 'http://dldir1.qq.com/qqfile/QQforMac/QQ_V4.0.4.dmg'
    url = sys.argv[1] if len(sys.argv) == 2 else url    download(url)
```
