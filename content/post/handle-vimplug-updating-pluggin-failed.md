---
title: "处理 vim-plug 更新插件失败的问题"
date: 2021-08-15T09:37:15+08:00
categories: ["Vim"]
tags: ["vim-plug", "Neovim"]
draft: false
---

vim-plug 更新插件失败的原因是与 GitHub 的网络连接不稳定。所以需要通过镜像网站来更新。

修改 vim-plug 的 `plug.vim` 文件。将文件中的

```vim
let fmt = get(g:, 'plug_url_format', 'https://git::@github.com/%s.git')
```

修改为

```vim
let fmt = get(g:, 'plug_url_format', 'https://git::@hub.fastgit.org/%s.git')
```

将

```vim
\ '^https://git::@github\.com', 'https://github.com', '')
```

修改为

```vim
\ '^https://git::@hub.fastgit\.org', 'https://hub.fastgit.org', '')
```

然后将之前安装的插件删除，重新用 `:PlugInstall` 命令下载安装插件。

参考：

- https://blog.csdn.net/htx1020/article/details/114364510
- https://blog.csdn.net/cameozuo/article/details/115698732

