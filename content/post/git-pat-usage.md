---
title: "使用个人 token 访问 GitHub"
date: 2021-08-15T11:24:03+08:00
categories: ["Git"]
tags: ["GitHub"]
draft: false
---

今天突然发现无法向 GitHub 远程库推送，报错：

```bash
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
```

然后在 GitHub 上选择 `Settings=>Developer Settings=>Personal Access Token=>Generate New Token`，填写表格，点击 `Generate token` 按钮生成 token，然后复制 token，在命令行输入：

```bash
git remote set-url origin https://<token>@github.com/<username>/<repo>
```

`<token>` 用生成的 token 代替，`<username>` 和 `<repo>` 则以自己的用户名和仓库名代替，然后就可以进行正常的 git 操作了。

参考：

- https://stackoverflow.com/questions/68775869/support-for-password-authentication-was-removed-please-use-a-personal-access-to

