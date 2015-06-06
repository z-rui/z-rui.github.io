---
date: 2015-06-07T01:29:57+08:00
title: Hugo的自动化部署
---

至今，本站的所有内容都是使用[Hugo]生成的。生成的内容托管在GitHub上，利用[GitHub Pages]服务进行展示。

在GitHub上有两个repo：[z-rui/hugo-site]和[z-rui/z-rui.github.io]。前者保存的是Hugo的源文件（markdown本文），后者保存的是Hugo生成的内容（`public`目录）。

然后我是这样做的：

1. 把`public`目录作为submodule添加到hugo-site repo中。
2. 在本地做修改之后，运行hugo更新`public`目录，然后commit submodule。
3. 最后commit外层的repo，并push。

这个工作流程来自[这里][1]。我把第二步的操作写成了脚本。要发布一篇新的内容，或者修改以前的内容，在命令行中需要这些命令：

```
$ vim content/post/xxxxxx
   ... 编辑了好久 ...
$ sh script/update.sh
$ git add content/post/xxxxxx
$ git add public
$ git commit
$ git push --recursive-submodules=on-demand
   ... 输入两遍用户名和密码 ...
```

后来我靠[git-credential-winstore]工具省去了重复输入用户名和密码的工作。

整个工作流程还是比较麻烦，而且容易出错。想要发布一些东西，效率比其他专门用于管理文章的内容管理系统还要差，并不能体现Git的优势。嗯，我说的就是WordPress。

为了不在这些琐碎的事情上浪费时间，我参考了[这篇文章][2]的说明，做自动构建。现在我的工作流程是：

```
$ vim content/post/xxxxxx
$ git add content/post/xxxxxx
$ git commit
$ git push
```

源文件推送到GitHub以后，[Wercker]会自动在它的服务器端帮我构建，并部署到GitHub Page中去。

这样的话整个工作就轻松地多了。我不用关心[z-rui/z-rui.github.io]这个repo的任何事情，也不需要把public目录添加到[z-rui/hugo-site]这个repo里面。

还有一个好处是，我甚至可以直接在GitHub的web界面上编辑内容。保存以后的更改也会自动地推送到GitHub Page上。

[Wercker]本意是用于软件的自动构建和部署。提供类似服务的网站似乎还有许多。这些服务本质上都是云计算。这也算是我首先体验到的云计算服务了吧 -- 而且是免费的。

我倒也很想了解它们具体的用法。只是若要用在软件开发上，我还不是很熟悉它们的工作原理，另外也不知道做什么样的开发。恰好它也能用于GitHub Pages的自动部署，那我就先试试看了。

[1]: http://gohugo.io/tutorials/github-pages-blog/
[2]: http://gohugo.io/tutorials/automated-deployments/
[Hugo]: http://gohugo.io/
[GitHub Pages]: https://pages.github.com/
[z-rui/hugo-site]: https://www.github.com/z-rui/hugo-site/
[z-rui/z-rui.github.io]: https://www.github.com/z-rui/z-rui.github.io/
[git-credential-winstore]: http://gitcredentialstore.codeplex.com/
[Wercker]: http://www.wercker.com/

<!--more-->

