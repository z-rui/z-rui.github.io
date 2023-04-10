---
date: 2023-04-09T22:44:42-07:00
title: 告别 Wercker
---

2015 年的时候，我设置了 [Wercker 工作流]({{<ref "/post/2015/06/automated-deploy.md">}})。每次本站的源代码仓库更新时，Wercker 的工作流会自动做两件事：1) 由 Markdown 源码构建静态网站；2) 发布到 github.io 上去。

时过境迁，Wercker 从某一年开始被 Oracle 收购了，直到去年的某个时候，我的工作流似乎不再工作了。wercker.com 现在会自动重定向到 Oracle Cloud 的网站；我甚至没法登录原来的 Wercker 账号。因此我需要找新的方法。

Github 在这些年也改变了很多，新增加的 Github Actions 功能在我看来就是一个抄袭了 Wercker 的功能。由于跟 Github 整合的更好，想必拉拢到了不少的用户群（拥有用户群的平台就是可以占便宜推广自家产品）。我也把自动构建的工作流迁移到了 Github Actions。

<!--more-->

在之前的某个时间点，Github Pages 增加了一个功能，允许源代码和生成的静态网站可以是同一个仓库里的不同分支，而在 Github 设置里选择正确的分支即可。我一直沿用了原先的独立的两个仓库的设置，因为懒得修改自动构建的工作流。现在，借移植到 Github Actions 的机会，我把两个仓库也合并了。

移植到 Github Actions 的步骤非常简单：

1. 把目标仓库添加为源代码仓库的一个 remote。
2. `git push -f` 到目标仓库。
3. 在目标仓库的设置里，把 Github Pages 的设置从 branch 改成 Github Actions。搜索 hugo，能找到现成的工作流模板。这一步会创建一个新文件 `.github/workflows/hugo.yml`，直接用模板即可。
4. 删除原来的 Wercker 配置文件。


